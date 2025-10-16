# Guía de Instalación de Fedora Server

### 1. Preparación de Medios de Instalación

1.  **Descarga del ISO:** Descargar la imagen **ISO de Fedora Server** desde la página oficial: [https://fedoraproject.org/es/server/download/].
    * *Nota:* Elegir la arquitectura correcta para el servidor (generalmente **x86_64**).

2.  **Descarga de la Herramienta:** Descargar e instalar **balenaEtcher**. También se puede usar Rufus o una herramienta similar. Esta utilidad es para escribir imágenes de disco en unidades USB.
3.  **Creación del USB Booteable:** Utilizar **balenaEtcher** para escribir la imagen ISO de Fedora en una **unidad USB** de al menos 8 GB.
    * *Advertencia:* Todos los datos en la unidad USB serán eliminados.
---

### 2. Arranque del Servidor
4.  **Conexión del USB:** Conectar la unidad USB booteable al servidor.
5.  **Acceso al Menú de Arranque (Boot Menu):** Reiniciar el servidor e inmediatamente presionar la tecla de acceso al menú de arranque o a la configuración del BIOS/UEFI.
    * ❗ **Importante:** La tecla para acceder a este menú **varía** según el fabricante del servidor (puede ser **F2**, **F10**, **F12**, **DEL**, o **ESC**). Consulte el manual de tu servidor.
6.  **Selección de Arranque:** En el menú, seleccionar la unidad USB que contiene la imagen de Fedora para iniciar el proceso.
    * *Nota:* En sistemas modernos, es posible que se deba seleccionar la opción **UEFI** para la unidad USB.

---

### 3. Proceso de Instalación

7.  **Inicio del Instalador:** El servidor arrancará desde la USB y presentará el menú de instalación. Seleccionar la opción para comenzar la instalación.

8.  **Configuración e Instalación:** Seguir los pasos del instalador gráfico (**Anaconda**) para configurar:
    * Idioma y teclado.
    * Opciones de red y hora.
    * **Particionamiento de discos** (Atención al uso de LVM y la separación de datos).
    * La contraseña de `root` y la creación de usuarios.

9.  **Finalización:** Una vez que la instalación haya terminado, retirar la unidad USB y **reiniciar** el servidor para arrancar en el nuevo sistema operativo Fedora Server.