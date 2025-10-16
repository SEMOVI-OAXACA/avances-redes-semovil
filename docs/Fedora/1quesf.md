# Fedora Server y Administración de Almacenamiento

## ¿Qué es Fedora Server?

**Fedora Server** es una edición del sistema operativo **Linux Fedora**, diseñada especialmente para su uso en **servidores y centros de datos**. Su propósito principal es ofrecer una **base estable, segura y flexible**, incorporando las tecnologías más recientes del ecosistema Linux para entornos empresariales y de administración de servicios.

A diferencia de la versión de escritorio, **Fedora Server no incluye un entorno gráfico (GUI)** por defecto, lo que lo hace **más liviano, rápido y seguro** para tareas de servidor. Sin embargo, **sí cuenta con una interfaz web llamada *Cockpit***, que permite administrar el sistema de forma visual desde cualquier navegador. A través de Cockpit, el administrador puede supervisar el estado del sistema, administrar usuarios, servicios, redes, almacenamiento y actualizaciones sin necesidad de usar exclusivamente la terminal.


## Características Principales

- **Flexibilidad y estabilidad:** Fedora Server proporciona una plataforma confiable y adaptable, ideal para alojar aplicaciones, servicios web, bases de datos o entornos de virtualización.  
- **Sin entorno gráfico predeterminado:** El sistema se maneja principalmente por línea de comandos o mediante la interfaz web *Cockpit*, evitando el consumo innecesario de recursos.  
- **Compatibilidad con virtualización y contenedores:** Fedora Server trabaja eficientemente con tecnologías modernas como **Podman**, **libvirt**, **KVM** y **Flatpak**.  
- **Optimizado para servidores:** Diseñado para ejecutar procesos y servicios de forma continua, priorizando la seguridad, el rendimiento y la administración remota.


## Requisitos Mínimos y Hardware Recomendado

Aunque Fedora puede instalarse con **1 GiB de RAM** y **2.5 GiB de espacio**, esos valores solo son útiles para pruebas o entornos muy limitados.  
Para un servidor funcional moderno, se recomienda:  

- **Memoria RAM:** 8 GiB (preferentemente en doble canal)  
- **Almacenamiento:** 64 GiB o más  
- **Procesador:** CPU de 64 bits con soporte para virtualización (Intel VT-x o AMD-V)  


## Organización del Almacenamiento (Particionamiento)

Fedora utiliza por defecto:  

- **Tabla de particiones GPT**, que mejora la compatibilidad con discos grandes.  
- **Volúmenes Lógicos (LVM)**, que permiten gestionar el espacio en disco de forma flexible, redimensionando o añadiendo volúmenes sin afectar los datos existentes.  

Durante la instalación, Fedora crea:  
- Una partición de arranque (**/boot**)  
- Una partición EFI o BIOS Boot (según el tipo de firmware)  
- Un **Grupo de Volúmenes LVM** (por ejemplo, *fedora*) donde se almacenan los distintos **Volúmenes Lógicos**, como:  
  - `root` → (aprox. 15 GiB): Contiene el sistema operativo.  
  - `swap` → Espacio de intercambio de memoria.  
  - Otros volúmenes creados por el administrador para aplicaciones o datos.

Separar el sistema del almacenamiento de datos garantiza una administración más segura y sencilla, especialmente en entornos donde se manejan múltiples servicios o bases de datos.


## RAID y Configuración de Red

### ¿Qué es RAID?

**RAID (Redundant Array of Independent Disks)** es una tecnología que combina varios discos duros físicos en una sola unidad lógica para mejorar el **rendimiento**, la **capacidad** o la **tolerancia a fallos**.  
Su objetivo es garantizar la **disponibilidad de los datos** en caso de que uno de los discos falle, o bien mejorar la **velocidad de lectura y escritura** en sistemas con altas cargas de trabajo.

### Tipos comunes de RAID

| Nivel RAID | Características | Ventajas | Desventajas |
|-------------|----------------|-----------|--------------|
| **RAID 0 (Striping)** | Divide los datos entre dos o más discos sin redundancia. | Máximo rendimiento. | Si un disco falla, se pierden todos los datos. |
| **RAID 1 (Mirroring)** | Duplica los datos en dos discos. | Alta seguridad y redundancia. | La capacidad total es igual a un solo disco. |
| **RAID 5 (Paridad distribuida)** | Requiere mínimo tres discos. Guarda información de paridad que permite reconstruir los datos si un disco falla. | Equilibrio entre rendimiento, capacidad y seguridad. | La reconstrucción tras un fallo puede ser lenta. |
| **RAID 6 (Doble paridad)** | Similar al RAID 5, pero permite que fallen dos discos simultáneamente. | Mayor tolerancia a fallos. | Menor rendimiento de escritura. |
| **RAID 10 (1+0)** | Combina RAID 1 y 0: espeja y distribuye los datos entre pares de discos. | Alto rendimiento y redundancia. | Requiere al menos 4 discos. |

En Fedora Server, los arreglos **RAID** pueden configurarse durante la instalación (a través del instalador Anaconda) o mediante la interfaz **Cockpit**, lo que facilita su administración incluso sin entorno gráfico.


### Configuración de red

Durante la instalación, Fedora Server detecta automáticamente la red y utiliza **DHCP** si está disponible.  
También permite establecer una **configuración estática**, definiendo manualmente la dirección IP, la máscara de red, la puerta de enlace y los servidores DNS.  
La administración posterior de la red puede hacerse mediante comandos (`nmcli`, `nmtui`) o desde la interfaz web *Cockpit*.
