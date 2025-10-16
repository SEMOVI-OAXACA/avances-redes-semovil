

# Instalación del Servidor Headscale Mediante Archivos Binarios

Para el despliegue del servidor Headscale, existen dos metodologías principales: el uso de **Docker** o la instalación directa a través de **archivos binarios**. A continuación, se detallan los pasos para el montaje utilizando la opción de archivos binarios, específicamente mediante paquetes `.deb` en sistemas basados en Debian/Ubuntu.

## 1\. Descarga del Paquete Binario `.deb`

Se debe localizar el número de la versión más reciente de Headscale en la página oficial de [Versiones de Headscale en GitHub](https://github.com/juanfont/headscale/releases).

El paquete `.deb` correspondiente puede descargarse directamente al servidor utilizando la herramienta `wget`. El ejemplo siguiente utiliza la versión `v0.26.1`:

```bash
wget https://github.com/juanfont/headscale/releases/download/v0.26.1/headscale_0.26.1_linux_amd64.deb
```

-----

## 2\. Instalación y Gestión del Servicio

### Instalación del Paquete

Se procede a la instalación del paquete descargado utilizando `apt`. El uso del comodín (`*`) en el nombre del archivo garantiza que el comando funcione sin importar el número de versión específico que se haya descargado.

```bash
sudo apt install ./headscale_*.deb
```

### Habilitación e Inicio del Servicio

Una vez instalado, se habilita e inicia el servicio de Headscale en el sistema mediante `systemctl`:

```bash
sudo systemctl enable --now headscale
```

Se recomienda verificar el estado del servicio para confirmar que se esté ejecutando correctamente:

```bash
sudo systemctl status headscale
```

-----

## 3\. Configuración Inicial del Servidor

El siguiente paso es crear o editar el archivo de configuración principal de Headscale.

```bash
sudo nano /etc/headscale/config.yaml
```

En este archivo, se deben ajustar las siguientes configuraciones esenciales:

  * **`server_url`**: Se establece la **URL pública** del servidor Headscale. Es una buena práctica comenzar con `https://` (ej. `https://headscale.tudominio.com:443`). Si no se dispone de un dominio, se puede configurar con la **dirección IP pública** del servidor, la cual puede obtenerse con comandos como `hostname -I` o `ip a`.
  * **`listen_addr`**: Indica la dirección y puerto donde Headscale escuchará las conexiones. El valor predeterminado usual es `0.0.0.0:8080`.
  * **`ip_prefixes`**: Los valores predeterminados suelen ser los adecuados.
  * **`dns_config`**: Permite configurar los servidores DNS que serán asignados a los clientes de la red.
  * **`log_level`**: Se puede establecer en `info` o `debug` para un mayor nivel de detalle en los registros.

### Reinicio para Aplicar Cambios

Tras modificar el archivo `config.yaml`, se debe reiniciar el servicio de Headscale para que los cambios surtan efecto:

```bash
sudo systemctl restart headscale
```

-----

## 4\. Creación de Usuarios y Claves de Autenticación

### Creación de un Usuario

Se crea un usuario para agrupar los dispositivos dentro de la red. En el ejemplo se utiliza `miusuario`:

```bash
sudo headscale users create miusuario
```

### Generación de Clave de Pre-autenticación

Se genera una clave que los clientes de **Tailscale** utilizarán para registrarse en el servidor Headscale.

Para una clave que expira después de 24 horas y que puede reutilizarse:

```bash
sudo headscale preauthkeys create --reusable --expiration 24h --user miusuario
```

Para una clave que no tiene fecha de expiración:

```bash
sudo headscale preauthkeys create --user miusuario
```

-----

## 5\. Conexión de Clientes y Verificación

### Conexión de Dispositivos Cliente

Una vez que un cliente tiene instalada la aplicación **Tailscale**, se conecta al servidor Headscale apuntando al URL del *login* y proporcionando la clave de autenticación generada.

```bash
tailscale up --login-server http://<IP_O_DOMINIO_HEADSCALE>:8080 --authkey TU_CLAVE_DE_AUTENTICACION
```

> ***Advertencia de Seguridad:*** *La clave de autenticación y los tokens de registro iniciales se transmiten en texto plano por internet si se utiliza HTTP. Se recomienda encarecidamente configurar TLS/HTTPS para el `server_url`. Una vez que el dispositivo se ha conectado, el tráfico de la VPN (WireGuard) queda **cifrado de punto a punto**.*

### Verificación de Nodos

Para confirmar que los dispositivos se han conectado correctamente al servidor Headscale:

**En el Servidor Headscale:**

```bash
sudo headscale nodes list
```

La lista debe mostrar los dispositivos registrados.

**En el Dispositivo Cliente (Tailscale):**

```bash
tailscale status
```

Esto mostrará el estado de la conexión en el lado del cliente.

-----

## Consideraciones Adicionales

### Apertura de Puertos

Dependiendo de la configuración del *firewall* del servidor (ej. UFW), puede ser necesario habilitar el puerto de escucha configurado para Headscale (por defecto, el `8080/tcp`).

```bash
sudo ufw allow 8080/tcp
sudo ufw reload
```