---
title: "Mi proyecto 2"
descripcion: "Proyecto de prueba 2"
url: "localhost:4321/proyects/proyecto2"
---
# Proyecto: **Desarrollo de Drivers para Disquetera**

## Descripción del Proyecto
El proyecto **FloppyDriver** tiene como objetivo desarrollar y optimizar un controlador (driver) de software para disqueteras de 3.5" (floppy disk) en sistemas operativos basados en Linux. El driver permite al sistema interactuar con la disquetera, gestionando la lectura y escritura de datos, así como la manipulación de los distintos modos de operación del hardware.

Este proyecto es un ejercicio educativo para comprender cómo funcionan los drivers de dispositivos de bajo nivel, la interacción con el kernel de Linux, y los conceptos fundamentales de I/O (Input/Output) en sistemas embebidos.

## Funcionalidades Clave
- **Detección automática de la disquetera**: El driver detecta la presencia del dispositivo y lo inicializa automáticamente al arranque del sistema.
- **Lectura y escritura de datos**: Soporte para operaciones básicas de lectura y escritura en discos de 3.5" de 1.44 MB.
- **Gestión de errores**: Implementación de mecanismos para detectar y manejar errores comunes como disco no presente, sector ilegible, etc.
- **Formato de disquetes**: Soporte para formateo de disquetes en formato FAT12.
- **Interrupciones de hardware**: Gestión de interrupciones generadas por el controlador de disquetera para optimizar el rendimiento.
- **Compatibilidad con múltiples arquitecturas**: El driver está diseñado para funcionar en arquitecturas x86 y ARM.

## Estructura del Proyecto


## Detalles Técnicos

### 1. **Comunicación con la Disquetera**
La disquetera está controlada por un **Floppy Disk Controller (FDC)**, que se encarga de las operaciones de bajo nivel como la lectura, escritura, y formateo de disquetes. Este controlador se comunica con la CPU a través de puertos de E/S (I/O Ports) en sistemas x86. El driver interactúa con el FDC usando los siguientes registros:

- **Data Register**: Usado para leer y escribir datos entre la disquetera y la memoria del sistema.
- **Main Status Register (MSR)**: Indica el estado de la disquetera y si está lista para operar.
- **Digital Output Register (DOR)**: Controla el motor de la disquetera y su habilitación.

El driver utiliza instrucciones **inb** y **outb** para leer y escribir en los puertos de E/S.

### 2. **Manejo de Interrupciones**
El controlador de disquetera genera interrupciones cuando se completa una operación de lectura o escritura. El driver debe registrarse para escuchar estas interrupciones y manejarlas adecuadamente. En sistemas Linux, esto se hace utilizando la función `request_irq()` del kernel, donde se asigna el controlador de la interrupción.

### 3. **Lectura y Escritura de Sectores**
Los disquetes están divididos en sectores, y el FDC opera en el nivel de sector. La estructura de un disco de 3.5" consiste en 80 pistas, con 18 sectores por pista. El driver debe ser capaz de leer y escribir sectores individuales.

Ejemplo de función para lectura de sectores:
```c
int floppy_read_sector(int sector, char* buffer) {
    // Enviar comandos de lectura al FDC
    outb(FDC_CMD_READ, FDC_DATA_REG);
    // Leer los datos del sector en el buffer
    for (int i = 0; i < SECTOR_SIZE; i++) {
        buffer[i] = inb(FDC_DATA_REG);
    }
    return 0; // Retorna 0 si la lectura fue exitosa
}