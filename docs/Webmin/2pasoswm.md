# Instalación y configuración de Webmin
La forma más simple y recomendada de obtener Webmin es usar el script automático `` webmin-setup-repo.sh `` para configurar los repositorios en los sistemas basados en RHEL o Debian.
Esto se puede hacer en dos pasos sencillos:

```
curl -o webmin-setup-repo.sh https://raw.githubusercontent.com/webmin/webmin/master/webmin-setup-repo.sh
```

```
sudo sh webmin-setup-repo.sh 
```

Este script configurará automáticamente el repositorio e instalará las claves GPG en el sistema, además de proporcionar el paquete de Webmin para su instalación y facilitar las actualizaciones en el futuro.

Si el repositorio de Webmin se configuró utilizando el script `` webmin-setup-repo.sh `` como se describió anteriormente, entonces Webmin se puede instalar fácilmente de la siguiente manera:

Para RHEL y derivados:

```
sudo dnf install webmin
```

Para Debian y derivados:

```
sudo apt-get install webmin --install-recommends
```

Después de una instalación exitosa de Webmin, puedes acceder a su interfaz ingresando en tu navegador:

``https://<IP-de-tu-servidor>:10000``


**Asegúrate de que la configuración de tu firewall permita el acceso a través del puerto 10000**.
