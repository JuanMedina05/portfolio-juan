---
title: "Mi proyecto 1"
descripcion: "Proyecto de prueba 1"
url: "localhost:4321/proyects/proyecto1"
---

# Proyecto: **Linux Automation Scripts**

## Descripción del Proyecto
**Linux Automation Scripts** es una colección de scripts Bash diseñados para automatizar tareas comunes de administración de sistemas en entornos Linux. Este conjunto de herramientas tiene como objetivo facilitar el manejo de servidores, la gestión de archivos, el monitoreo de recursos y la seguridad del sistema.

Cada script está diseñado para ser reutilizable, configurable y modular, de forma que pueda adaptarse a diferentes distribuciones de Linux, como Ubuntu, CentOS, Debian, etc.

## Funcionalidades Clave
- **Monitoreo del sistema**: Scripts para monitorear el uso de CPU, memoria, y disco, así como la disponibilidad de servicios.
- **Gestión de usuarios y permisos**: Scripts para crear, eliminar, y gestionar usuarios y grupos, incluyendo copias de seguridad de configuraciones.
- **Gestión de archivos**: Automatización de la copia, compresión y sincronización de archivos entre servidores.
- **Actualización del sistema**: Scripts para realizar actualizaciones automáticas y limpiar el sistema de archivos temporales o paquetes obsoletos.
- **Copia de seguridad (Backups)**: Herramientas para automatizar backups locales y remotos (usando `rsync` o `scp`).
- **Manejo de logs**: Scripts para la rotación automática de logs y eliminación de archivos de registro antiguos.
- **Seguridad básica**: Scripts para configurar cortafuegos (iptables), bloquear IPs sospechosas y asegurar SSH.

## Estructura del Proyecto

## Scripts Destacados

### 1. **Monitoreo de CPU (cpu_usage.sh)**

Este script revisa el uso de CPU en tiempo real y envía alertas si el uso supera un umbral predefinido.

```bash
#!/bin/bash
THRESHOLD=85
CPU_USAGE=$(grep 'cpu ' /proc/stat | awk '{usage=($2+$4)*100/($2+$4+$5)} END {print usage}')
if (( ${CPU_USAGE%%.*} > THRESHOLD )); then
  echo "Alerta: El uso de CPU es demasiado alto: $CPU_USAGE%"
  # Enviar alerta por email o mensaje (ejemplo con mail)
  echo "El uso de CPU ha superado el $THRESHOLD%" | mail -s "Alerta de CPU Alta" admin@ejemplo.com
fi