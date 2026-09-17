---
id: 02-comandos-fs2
aliases: []
tags: []
date: 2026-09-07 15:11
title: 02-Comandos-FS2
---

# Comandos - Instalación de Arch con Btrfs y Fundamentos de la Shell



---


| Comando | Sintaxis | Ejemplo en Arch | Notas LPI | Notas Arch |
| --------------- | --------------- | --------------- | --------------- | --------------- |
| `ping` | `ping [opciones] destino` | `ping -c 3 archlinux.org` | *Objetivo 4.4 LPI:* Envía paquetes ICMP Echo Request para comprobar la conectividad de red con un host remoto. | Indispensable en el entorno Live para confirmar acceso a internet antes de ejecutar `pacstrap`. |
| `arch-chroot` | `arch-chroot /punto_montaje` | `arch-chroot /mnt` | No entra en LPI Essentials. | Script propio de Arch que monta los sistemas de archivos virtuales (`/dev, /proc, /sys`) y entra al sistema instalado. |
| `mkfs.btrfs` | `mkfs.btrfs [opciones] /dev/dispositivo` | `mkfs.btrfs -L "ARCH_ROOT" /dev/sda2` | Btrfs se estudia como ampliación técnica. | Formatea la partición asignada habilitando el soporte de subvolúmenes y características avanzadas de Btrfs. |
| `pacstrap` | `pacstrap [punto_montaje] [paquetes]` | `pacstrap /mnt base linux btrfs-progs` | Fuera de LPI. | Instala el sistema base, el kernel y las utilidades del sistema de archivos en el directorio objetivo. |
| `echo` | `echo [opciones] [texto_o_variable]` | `echo "Mi PATH es: $PATH"` | *Objetivo 2.1 LPI:* Se usa para imprimir cadenas de texto y evaluar valores de variables. | Comando integrado de Bash sin diferencias entre distribuciones. |
| `export` | `export VARIABLE=valor` | `export EDITOR=nano` | *Objetivo 2.1 LPI:* Exporta una variable local para convertirla en variable de entorno heredable. | Permite definir configuraciones de sesión para procesos hijos en el shell. |
| `type` | `type comando` | `type cd` | *Objetivo 2.1 LPI:* Identifica el tipo de comando (integrado, ejecutable, alias). | Permite verificar si un comando es un builtin de Bash o un ejecutable externo en el sistema. |
| `which` | `which comando` | `which btrfs` | Muestra la ruta absoluta del binario buscando en la variable de entorno `$PATH`. | Útil para localizar la ubicación exacta de herramientas instaladas en `/usr/bin/`. |


