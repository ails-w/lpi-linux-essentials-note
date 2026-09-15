---
id: 01-teoria-fs2
aliases: []
tags: []
date: 2026-09-08 22:31
title: 01-Teoria-FS2
---

# Instalación de Arch con Btrfs y Fundamentos de la Shell



---

## 1. Objetivos de la fase

- **Comprender los conceptos fundamentales de la shell (Bash)**, la sintaxis general de comandos (`comando [opciones] [argumentos]`), la interpretación del prompt (`$` vs `#`) y la diferencia estructural entre comandos internos (builtins) y externos.
- **Manejar la definición y manipulación de variables** locales y de entorno (`export`, `unset`), comprendiendo el rol crítico que juega la variable de entorno `$PATH` en la localización de ejecutables.
- **Aplicar las reglas de escape y comillas** (dobles `"`, simple `'` y barra invertida `\`) para controlar la expansión de variables y caracteres especiales.
- **Aprender el procedimiento de instalación independiente de Arch Linux** desde un entorno en vivo, abarcando la verificación de red, particionado de disco, despliegue del sistema base y cambio de entorno raíz (chroot).
- **Configurar una estructura avanzada de almacenamiento Btrfs** mediante la creación de subvolúmenes y la activación de comprensión transparente `zstd`.

---

## 2. Conceptos Clave

### La Shell y la Línea de Comandos

- **Definiciín de Shell:** Es una interfaz basada en texto que lee e interpreta las instrucciones por el usuario para ejecutarlas en el sistema operativo. Aunque existen múltiples intérpretes como Zsh o Ksh, Bash (Bourne-Again Shell) es el estándar en la certificación LPI Essentials.
- **Estructura del Prompt:** El indicador o prompt muestra el estado contextual (`usuario@host:directorio`). El carácter `$` especifica un usuario sin privilegios administrativos, mientras que `#` advierte que la sehll la ejecuta el superusuario `root`.
- **Estructura de un comando:** La sintaxis estándar sigue el formato `comando [opción(es)]` `[argumento(s)]`. Las opciones modifican el comportamiento del programa (suelen iniciar con `-` o `--`), mientras que los argumentos indican el objeto sobre el que actúa el comando (ficheros, directorios, variables).

### Tipos de Comandos

- **Comandos Internos (Builtins):** Están integrados directamente dentro del código binario de la shell y no existen como programas separados en el disco. Su ejecución es inmediata y controlan el entorno propio de la shell (ejemplos: `cd`, `export`, `set`, `exit`).
- **Comandos Externos:** Son programas o scripts almacenados como archivos independientes en el sistema de archivos (ejemplos: `ls`, `man`, `cat`, `pacman`). Cuando se invocan, la shell consulta la variable `$PATH` para localizar su ejecutable.
- **Inspección con `type`:** La utilidad `type` permite conocer la naturaleza exacta de cualquier instrucción introducida en la terminal.

### Gestión de Variables y la Variable `$PATH`

- **Variables Locales:** Se definen con la sintaxis `nombre=valor` (sin espacios alrededor del signo `=`). Solo están disponibles en el proceso de shell donde fueron creadas y no son heredadas por ningún subproceso hijo.
- **Variables de Entorno (Globales):** Se crean o promueven utilizando el comando `export` `nombre=valor`. Están disponibles tanto en la shell actual como en todos los programas y subprocesos derivados de ella. Se eliminan mediante el comando `unset nombre`.
- **La Variable `$PATH`:** Almacena una lista de rutas de directorios delimitadas por dos puntos (`:`). Cuando ejecutas un comando externo, la shell busca en orden secuencial dentro de esos directorios de izquierda a derecha hasta encontrar el primer archivo ejecutable coincidente.

### Reglas de Comillas (Quoting) y Escape

- **Comillas Dobles ("") / Débiles:** Evitan que los espacios dividan los argumentos en palabras separadas, pero permiten la expansión de variables mediante el signo `$` (ejemplo: `"Hola $USER"` imprimer `Hola tom`).
- **Comillas Simples (`'`) / Fuertes:** Desactivan todo significado especial de cualquier carácter que encierren. Tratan el texto de forma estrictamente literal (ejemplo: `'Hola $USER` imprime `Hola $USER`).
- **Barra Invertida (\) / Escape:** Anula el significado especial del carácter inmediatamente posterior (ejemplo: `echo \$USER` imprime `$USER`).

---

## 3. Contexto Arch

### Despliegue Manual Base

- A diferencia de distribuciones de consumo masivo como Ubuntu (que utilizan instaladores gráficos asistidos como Ubiquity o Calamares), la instalación de Arch Linux se realiza completamente desde una terminal de comandos.
  1. **Entorno Live:** El medio de instalación arranca directamente en un entorno Zsh como usuario `root`. Lo primero es verificar la red mediante `ping archlinux.org` y actualizar el llavero de firmas con `pacman -Sy archlinux-keyring` para asegurar las descargas.
  2. **Particionado con GPT:** Mediante la herramienta `cfdisk`, se define una tabla de particiones `GPT` con una partición EFI (`/dev/sdX1` de 512mb), una Swap opcional y una partición raíz de Btrfs.
  3. **Instalación con `pacstrap`:** En lugar de extraer una imagen precompilada del sistema operativo, el script `pacstrap` descarga e instala directamente desde los repositorios los paquetes indispensables dentro del directorio de montaje `/mnt`:
    - *pacstrap -K /mnt base linux linux-firmware btrfs-progs*

  - **Generación de `fstab` y Chroot:** Se genera el archivo de montaje permanente identificando los volúmenes por su UUID (`genfstab -U /mnt >> /mnt/etc/fstab`) y se ingresa al nuevo sistema instalado mediante `arch-chroot /mnt`.

### Estructura Avanzada Btrfs y Comprensión Nativa

- Arch Linux aprovecha al máximo las características modernas del sistema de archivos Btrfs frente al tradicional `ext4`:
  - **Subvolúmenes:** En lugar de fijar tamaños rígidos por partición, en Btrfs formateas la partición raíz (`mkfs.btrfs /dev/sdX3`) y crea subvolúmenes que comparten dinámicamente todo el espacio disponible:
    - `@` (montado en `/` para la raíz del sistema).
    - `@home` (montado en `/home` para los datos de usuario).
    - `@snapshots` (montado en `/.snapshots` para copias de seguridad).
    - `@log` y `@cache` (montadas en `/var/log` y `/var/cache` para evitar llenar las instantáneas con registros temporales).

  - **Comprensión transparente `zstd`:** Durante el montaje de la partición raíz en la instalación y en la configuración final del `fstab`, se aplica el parámetro de montaje `-o compress=zstd`. Btrfs comprime y descomprime automáticamente los archivos al escribir o leer del almacenamiento, reduciendo el desgaste de las unidades SSD/NVMe y ahorrando espacio sustancial en disco.
  - **Aislamiento de snapshots:** Al ubicar `@snapshots` como un subvolumen independiente en el nivel superio (`subvolid=5`) en lugar de anidarlo dentro de `@`, se garantiza que, si el sistema sufre una avería grave por una actualización, se puede borrar o renombrar el subvolumen `@` y restaurar una instantánea funcional mediante un simple movimiento de carpetas sin perder el historial de backups.

---

## 4. Glosario

1. **Shell:** Intérprete de comandos basado en texto que sirve de interfaz entre el usuario y el kernel de Linux.
2. **Bash:** Bourne-Again Shell, el intérprete de comandos por defecto en el estándar LPI y en la mayoría de distribuciones.
3. **Prompt:** Indicador visual en la terminal que señala que la shell está lista para recibir comandos (`$` para usuario normal, `#` para superusuario).
4. **Builtin:** Comando interno que forma parte del código propio de la shell y se ejecuta sin invocar un archivo ejecutable del disco.
5. **PATH:** Variable de entorno que contiene la lista de directorios donde la shell busca los archivos ejecutables de comandos externos.
6. **pacstrap:** Utilidad exclusiva de Arch Linux utilizada durante la instalación para descargar e instalar paquetes en la partición montada.
7. **arch-chroot:** Script de Arch que permite ingresar al entorno del nuevo sistema instalado montando de forma automática los sistemas de archivos virtuales (`/proc, /sys, /dev`).
8. **Btrfs:** Sistema de archivos avanzado basado en el principio Copy-on-Write (CoW), que permite subvolúmenes, comprensión integrada e instantáneas (snapshots).
9. **Subvolumen:** Unidad lógica independiente de archivos dentro de Btrfs que se comporta como un punto de montaje sin requerir un tamaño rigido predeterminado.
10. **zstd:** Algoritmo de comprensión de alto rendimiento (Zstandard) integrado en Btrfs para comprimit datos de manera transparente.

