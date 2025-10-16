
# Conexión y Registro de Clientes en Headscale

Para incorporar un nuevo dispositivo a la red gestionada por Headscale, el cliente debe tener instalada la aplicación de **Tailscale** y utilizar el comando `tailscale up` para apuntar al servidor Headscale con una clave de autenticación válida.

## 1\. Requisitos Previos en el Cliente

El dispositivo que se desea conectar debe contar con la aplicación oficial de **Tailscale** instalada y ejecutándose en el sistema operativo correspondiente (Linux, Windows, macOS, Android, iOS, etc.).

## 2\. Comando de Conexión

En el dispositivo cliente (terminal o *shell*), el administrador debe ejecutar el comando `tailscale up`, especificando la dirección del servidor Headscale (el **`server_url`** o **`listen_addr`** configurado) y la **clave de pre-autenticación** generada para el usuario correspondiente.

### Sintaxis del Comando

```bash
tailscale up --login-server http://<DIRECCION_HEADSCALE>:<PUERTO_HEADSCALE> --authkey <TU_CLAVE_DE_AUTENTICACION>
```

**Consideración Importante:**
Si en el `config.yaml` de Headscale se configuró un `server_url` con un dominio y HTTPS (por ejemplo, `https://headscale.tudominio.com:443`), el comando de conexión debe reflejar esa dirección y protocolo.

## 3\. Verificación de la Conexión

Una vez ejecutado el comando en el cliente, el dispositivo intentará registrarse.

### En el Dispositivo Cliente

El estado de la conexión se puede verificar ejecutando:

```bash
tailscale status
```

El dispositivo debería mostrarse como conectado al *login server* configurado y asignársele una IP dentro del rango de Tailscale (por lo general, en el rango `100.x.x.x`).

### En el Servidor Headscale

Para confirmar que el nodo ha sido registrado y está activo en el servidor, se utiliza el comando de listado de nodos:

```bash
sudo headscale nodes list
```

El nuevo dispositivo aparecerá en la lista asociado al usuario para el cual se generó la clave de pre-autenticación.

-----

**Recordatorio:** El uso de HTTP para el `login-server` (como en el ejemplo) implica que la clave de autenticación inicial viaja en texto plano. Se recomienda encarecidamente la configuración de un *proxy* inverso y TLS/HTTPS para el servidor Headscale para garantizar la seguridad durante el registro.