---
id: 03-reto-fs2
aliases: []
tags: []
date: 2026-09-18 08:10
title: 03-Reto-FS2
---

# Reto - Instalación de Arch con Btrfs y Fundamentos de la Shell

### 1\. Objetivo del reto

Desplegar la estructura básica de subvolúmenes Btrfs con compresión nativa sobre la partición de destino y manipular variables de entorno y comandos en la terminal Bash.

---

### 2\. Lista de tareas

1. **Verificar conectividad de red**
  * **Descripción**: Comprueba que el entorno Live de Arch Linux tiene acceso a internet enviando tres paquetes ICMP Echo Request antes de proceder con el particionado o descarga.
  * **Comando(s)**:

```bash
ping -c 3 archlinux.org
```

---

1. **Verificación tipo checksum**: La salida debe mostrar `3 packets transmitted, 3 received, 0% packet loss`[2].
2. **Formatear la partición principal con el sistema de archivos Btrfs**
  * **Descripción**: Formatea la partición asignada al sistema (`/dev/sda2` o tu dispositivo objetivo) aplicando la etiqueta `ARCH_ROOT`[3].
  * **Comando(s)**:

```bash
mkfs.btrfs -f -L "ARCH_ROOT" /dev/sda2
```

---

1. **Verificación tipo checksum**: Al ejecutar `lsblk -f /dev/sda2`, la columna `FSTYPE` debe mostrar `btrfs` y la columna `LABEL` debe indicar `ARCH_ROOT`.
2. **Crear la estructura de subvolúmenes Btrfs**
  * **Descripción**: Monte temporalmente la partición en `/mnt` y cree los subvolúmenes lógicos `@` (para la raíz), `@home` (para usuarios) y `@snapshots` (para instantáneas)[4][5].
  * **Comando(s)**:

```bash
mount /dev/sda2 /mnt
btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
btrfs subvolume create /mnt/@snapshots
```

---

1. **Verificación tipo checksum**: El comando `btrfs subvolume list /mnt` debe listar los subvolúmenes con los identificadores `path @`, `path @home` y `path @snapshots`[6][7].
2. **Montar el subvolumen raíz con compresión transparente ZSTD**
  * **Descripción**: Desmonte `/mnt` y vuelva a montar únicamente el subvolumen `@` activando la opción de compresión transparente `zstd`[5][8].
  * **Comando(s)**:

```bash
umount /mnt
mount -o compress=zstd,subvol=@ /dev/sda2 /mnt
```

---

1. **Verificación tipo checksum**: La ejecución de `findmnt /mnt` debe confirmar en las opciones de montaje los parámetros `compress=zstd` y `subvol=/@`.
2. **Crear y exportar variables de entorno en Bash**
  * **Descripción**: Declara una variable local llamada `ENTORNO_ESTUDIO` con el valor `lpi_arch` y exprótala para convertirla en variable de entorno heredable[9][10].
  * **Comando(s)**:

```bash
ENTORNO_ESTUDIO="lpi_arch"
export ENTORNO_ESTUDIO
```

---

1. **Verificación tipo checksum**: El comando `bash -c 'echo $ENTORNO_ESTUDIO'` debe devolver exactamente la cadena `lpi_arch` en un subshell hijo[11].
2. **Diferenciar tipos de comandos y localización en la variable** **$PATH**
  * **Descripción**: Inspecciona el comando `cd` para confirmar si es un *builtin* e identifica la ruta ejecutable de la herramienta `btrfs` dentro de los directorios de la variable `$PATH`[12][13].
  * **Comando(s)**:

```bash
type cd
which btrfs
```
---

1. **Verificación tipo checksum**: `type cd` debe retornar `cd is a shell builtin`[12] y `which btrfs` debe entregar la ruta absoluta `/usr/bin/btrfs`[13].
2. **Extensión temporal de la variable ejecutable** **$PATH**
  * **Descripción**: Añada el directorio `~/bin` al final de su variable de entorno `$PATH` preservando todas las rutas configuradas por defecto[13].
  * **Comando(s)**:

```bash
export PATH=$PATH:$HOME/bin
```

* **Verificación tipo checksum**: La salida del comando `echo $PATH` debe finalizar con la secuencia `:/root/bin` (o `/home/usuario/bin`)[13].
