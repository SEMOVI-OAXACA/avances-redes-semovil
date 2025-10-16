# ¿Qué es Fedora?
Fedora Server es una versión del sistema operativo Linux Fedora, diseñada específicamente para servidores y centros de datos. Su objetivo es proporcionar una base estable y flexible que incluya las últimas tecnologías de centros de datos, lo que la hace ideal para alojar servicios y aplicaciones web. A diferencia de la versión de escritorio, Fedora Server no trae un entorno gráfico 
por defecto.
**Características principales**
- Flexibilidad y estabilidad: Ofrece una base sólida y adaptable para la prestación de servicios e información digitales. 
- Sin entorno de escritorio predeterminado: Viene sin una interfaz gráfica instalada, lo que la hace más ligera y eficiente para su propósito principal como servidor, aunque se puede añadir una. 
- Compatibilidad: Funciona bien con tecnologías de virtualización y contenedores, como Podman y Flatpak. 
- Especializado para servidores: Está optimizado para ejecutar servicios y aplicaciones en un entorno de servidor, en lugar de para uso de escritorio general. 

## Requisitos Mínimos y Hardware Sugerido

Técnicamente, el sistema Fedora usa 2.5 GiB de espacio y 1 GiB de RAM, pero esto no tiene aplicación práctica. Para un servidor pequeño a mediano hoy en día, se sugiere hardware comprable: 64 GiB de almacenamiento y al menos 8 GiB de RAM (doble canal).

## Organización del Almacenamiento (Particionamiento)

Fedora utiliza por defecto una tabla GPT y Volúmenes Lógicos (LVM) para priorizar la fiabilidad y la separación de datos. El particionamiento por defecto crea particiones de arranque (/boot, EFI/BIOS boot) y un gran Grupo de Volúmenes LVM (ej. fedora).

Dentro del LVM, asigna unos 15 GiB a un Volumen Lógico root para el sistema operativo. El objetivo clave es separar el sistema de los datos de usuario. Los datos de las aplicaciones deben ir en Volúmenes Lógicos creados aparte por el administrador.

## RAID y Red
Si hay varios discos, se recomienda configurar RAID para redundancia, no solo añadirlos al LVM. Por otro lado, la instalación configura la red con DHCP si está disponible, o permite una configuración estática.
