# Guía de Instalación de Fedora Server


## 1. Preparación de los Medios de Instalación

### Descarga de la imagen ISO

- Ir al sitio oficial de Fedora:  
  👉 [https://fedoraproject.org/es/server/download/](https://fedoraproject.org/es/server/download/)

- Seleccionar la versión más reciente de **Fedora Server** y **verificar que la arquitectura sea la adecuada** para el hardware del servidor.  
  - En la mayoría de los casos, se debe elegir **x86_64 (64 bits)**.



### Descarga de la herramienta de creación de USB booteable

- Se recomienda utilizar **balenaEtcher** para grabar la imagen ISO en una memoria USB.  
  También se pueden usar herramientas alternativas como **Rufus** o **Ventoy**.

**Pasos:**
1. Instalar balenaEtcher en una computadora con Windows, Linux o macOS.  
2. Abrir la aplicación y seleccionar:
   - **Imagen ISO:** la de Fedora Server descargada.  
   - **Destino:** la memoria USB (mínimo 8 GB).  
3. Presionar **Flash** para iniciar la escritura.

> ⚠️ **Advertencia:** Todos los datos en la memoria USB se eliminarán durante este proceso.


## 2. Configuración de RAID Antes de la Instalación

Antes de comenzar la instalación de Fedora Server, es importante **preparar los discos duros y configurar el arreglo RAID** desde el BIOS o desde el controlador de almacenamiento del servidor.

### Configuración de RAID 1

- Ingresar al **menú de configuración del BIOS/UEFI** o al **controlador RAID** del equipo (por ejemplo, Dell PERC, HP Smart Array, Intel Rapid Storage, etc.).
- Crear un nuevo **arreglo RAID 1 (Mirroring)** utilizando **dos discos físicos del mismo tamaño**.
- Este tipo de RAID almacena **una copia exacta de los datos en ambos discos**, garantizando que, si uno falla, el sistema seguirá funcionando con el otro sin pérdida de información.
- Una vez creado el arreglo, el sistema lo reconocerá como **una sola unidad lógica (Logical Volume)**, lista para la instalación del sistema operativo.

> 💡 **Nota técnica:** RAID 1 no mejora el rendimiento de escritura, pero **duplica la seguridad de los datos**, lo que es ideal para servidores críticos.


## 3. Arranque del Servidor

### Conexión del USB

- Inserta la memoria USB booteable en el servidor físico o virtual.

### Acceso al Menú de Arranque

1. Reinicia el servidor.  
2. Durante el inicio, presiona la tecla correspondiente para acceder al menú de **Boot** o a la **configuración del BIOS/UEFI**.  
   - Las teclas más comunes son: `F2`, `F10`, `F12`, `DEL` o `ESC`.  
   - Si no sabes cuál usar, consulta el manual del fabricante de tu servidor.

### Selección del Medio de Arranque

- En el menú de arranque, selecciona la unidad **USB UEFI** que contiene la imagen de Fedora Server.
- El sistema iniciará el **instalador de Fedora (Anaconda)**.

> 🔧 En equipos modernos con soporte UEFI, es recomendable elegir siempre la opción de arranque **UEFI USB** para garantizar compatibilidad y un mejor manejo del particionado GPT.


## 4. Proceso de Instalación

### Inicio del Instalador

- Una vez que el sistema arranca desde el USB, aparecerá el menú de instalación de Fedora.  
- Selecciona la opción **“Install Fedora Server”** para iniciar el asistente de instalación.


### Configuración General del Sistema

Dentro del instalador gráfico **Anaconda**, se deben configurar los siguientes apartados:

1. **Idioma y Distribución de Teclado:**  
   Selecciona el idioma español (México o España) y la distribución adecuada de teclado.

2. **Fecha, Hora y Zona Horaria:**  
   Configura la ubicación geográfica correcta. Si el servidor tiene acceso a internet, Fedora sincronizará la hora mediante NTP.

3. **Configuración de Red:**  
   - Si hay un servidor DHCP, Fedora asignará automáticamente una dirección IP.  
   - También puedes establecer una **IP estática**, asignando manualmente la dirección, máscara, puerta de enlace y DNS.  
   - Verifica que el nombre del host sea correcto, por ejemplo:  
     `fedora-server.local`.

4. **Destino de la Instalación (Discos):**  
   - Fedora detectará el **volumen lógico RAID 1** creado previamente, mostrándolo como un único disco.  
   - Selecciona ese volumen como destino de instalación.  
   - El particionamiento puede ser automático (LVM por defecto) o manual.  
   - Si se elige manual, asegúrate de crear al menos:  
     - `/boot`  
     - `swap`  
     - `/` (root)  
     - Volúmenes adicionales si deseas separar `/home` o `/var`.

> ⚙️ Fedora usa **LVM (Logical Volume Manager)** sobre el RAID, lo que permite flexibilidad en la gestión de particiones, ampliación de volúmenes o creación de nuevas unidades lógicas sin afectar los datos existentes.

5. **Creación de Usuarios:**  
   - Define una contraseña para el usuario **root (administrador)**.  
   - Crea un usuario normal con permisos administrativos (opcional pero recomendado).


### Finalización de la Instalación

- Una vez configurados todos los parámetros, presiona **“Empezar Instalación”**.  
- El instalador copiará los archivos del sistema al RAID configurado y aplicará las configuraciones seleccionadas.  
- Cuando finalice, **retira la memoria USB** y **reinicia el servidor**.


## 5. Primer Inicio del Sistema

- El servidor arrancará desde el **arreglo RAID 1** y cargará **Fedora Server**.  
- Podrás iniciar sesión por consola o acceder desde otro dispositivo mediante la interfaz web **Cockpit**, disponible por defecto en el puerto **9090**:  
  ```bash
  https://<IP_DEL_SERVIDOR>:9090
  ```

Desde allí podrás administrar el servidor, verificar el estado del RAID, gestionar usuarios, monitorear el rendimiento y realizar actualizaciones.
