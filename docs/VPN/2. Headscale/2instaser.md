# Prueba de montaje en servidor
Para el montaje de servidor Headscale, existen 2 metodos: Mediante el uso de Docker o el uso de los archivos binarios. A continuación se explicaran los pasos para montarlo usandos los archivos binarios.


## 1. Descarga del último paquete .deb de Headscale:
Ve a la página de versiones de Headscale en GitHub para encontrar el número de la versión más reciente.

Se puede descargar directamente en tu servidor usando ``wget.`` 
``` Bash
wget https://github.com/juanfont/headscale/releases/download/v0.26.1/headscale_0.26.1_linux_amd64.deb
```

## 2. Instala el paquete:
El uso del * en el nombre del archivo ayuda a que el comando funcione sin importar la versión que se haya descargado.
```Bash
sudo apt install ./headscale_*.deb
```
Habilita e inicia el servicio de Headscale:

```Bash
sudo systemctl enable --now headscale
```
Verifica que el servicio esté corriendo:

```Bash
sudo systemctl status headscale
```
## 3. Configuración 
Crear archivo de configuración 
```Bash
sudo nano /etc/headscale/config.yaml
```
Aquí se pueden ajustar algunas configuraciones:

- **server_url:** Se establecela URL pública del servidor Headscale. Debe comenzar con https:// (recomendado) o http://. Por ejemplo: `https://headscale.tudominio.com:443`. Si no se tiene un dominio, se puede configurar con la ip pública. (Para saber cual es, con ``hostname -I`` o tambien ``ip a`` )
- **listen_addr:** La dirección en la que Headscale escuchará. Generalmente 0.0.0.0:8080.
- **ip_prefixes:** Puedes dejar los valores predeterminados.
- **dns_config:** Configura servidores DNS para tus clientes.
- **log_level:** Puedes establecerlo en info o debug para más detalles.

**Ejemplo:**
```bash
version: '3.8'
services:
  headscale:
    image: headscale/headscale:latest
    container_name: headscale
    restart: unless-stopped
    volumes:
      - ./config:/etc/headscale
    ports:
      - "8080:8080"
    command: headscale serve
```

## 4. Se reinicia Headscale para aplicar los cambios
```bash
sudo systemctl restart headscale
```
## 5. Crear un usuario 
```bash
sudo headscale users create miusuario
```
## 6. Genera una clave de pre-autenticación:
```bash
sudo headscale preauthkeys create --reusable --expiration 24h --user miusuario
```
O sin expirar:
```bash.
headscale preauthkeys create --user mi usuario
```

## 7. Conecta tus clientes:
Al conectar un nuevo dispositivo, el comando tailscale up cambiará para apuntar a tu IP:
```Bash
tailscale up --login-server http://0.0.0.0:8080 --authkey TU_CLAVE_DE_AUTENTICACION
```
***Advertencia de Seguridad:** La clave de autenticación y los tokens de registro iniciales se enviarán en texto plano por internet. Una vez que el dispositivo está conectado, el tráfico de la VPN (WireGuard) sí está cifrado de punto a punto. El riesgo está durante el registro.*

Es necesario que el dispositivo cuente con la aplicación de **Tailscale** instalado 

Para confirmar su correcta configuración en el servidor
```Bash
sudo headscale nodes list
```
Deben de aparecer los dispositivos

Para confirmar su correcta configuración en el cliente

```Bash
tailscale status
```

## 8. Tomar en cuenta
Dependiendo de la configuración, hay que habilitar los puertos correspondientes
Ejemplo:
```Bash
sudo ufw allow 8080/tcp
sudo ufw reload
```