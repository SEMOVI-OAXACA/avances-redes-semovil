# Instalación en Portainer 
## 1. Preparativos 
Para este caso, es necesario tener Docker y Docker compose instalado. Una vez instaladas estas herramientas, se procede a desplegar el contenedor de Portainer.
``` bash
docker volume create portainer_data
# Despliega Portainer
docker run -d -p 9443:9443 -p 8000:8000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```
Es necesario crear un directorio base en el servidor con la configuración de Headscale ya funcional. Se tomará de ejemplo la ruta /opt/headscale/. por lo que se debe crear la estructura de directorios y el archivo de DB

``` bash
mkdir -p /opt/headscale/config
mkdir -p /opt/headscale/data
```
Una vez creadas, se descarga la configuración de ejemplo de Headscale dentro de `/opt/headscale/config`
``` bash
curl -o config.yaml https://raw.githubusercontent.com/juanfont/headscale/main/config-example.yaml
```
Aquí se pueden ajustar algunas configuraciones:

- **server_url:** Se establecela URL pública del servidor Headscale. Debe comenzar con https:// (recomendado) o http://. Por ejemplo: `https://headscale.tudominio.com:443`. Si no se tiene un dominio, se puede configurar con la ip pública. (Para saber cual es, con ``hostname -I`` o tambien ``ip a`` )
- **listen_addr:** La dirección en la que Headscale escuchará. Generalmente 0.0.0.0:8080.
- **ip_prefixes:** Puedes dejar los valores predeterminados.
- **dns_config:** Configura servidores DNS para tus clientes.
- **log_level:** Puedes establecerlo en info o debug para más detalles.

## 2. Contenedores
Entrar a porteinar con la dirección IP del servidor y el puerto definido. Ejemplo: `https://10.13.122.246:9443`


