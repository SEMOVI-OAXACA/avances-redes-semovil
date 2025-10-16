
# Comandos Útiles de la CLI de Headscale

La utilidad principal de la línea de comandos (`headscale`) permite a los administradores gestionar usuarios, nodos, claves de autenticación y configuraciones generales del servidor.

**Nota:** Cuando Headscale se instala con archivos binarios, los comandos suelen ejecutarse con `sudo`, por ejemplo: `sudo headscale [comando]`. Si se utiliza un contenedor CLI (Stack 2 en la documentación de Portainer), es necesario acceder al *shell* del contenedor y ejecutar el comando sin `sudo`.

---

## 1. Gestión de Usuarios

| Comando | Descripción |
| :--- | :--- |
| `headscale users list` | Muestra una lista de todos los usuarios registrados en el servidor Headscale. |
| `headscale users create [nombre_usuario]` | Crea un nuevo usuario en el sistema. Ejemplo: `headscale users create juanito`. |
| `headscale users rename [nombre_actual] [nombre_nuevo]` | Renombra un usuario existente. |
| `headscale users delete [nombre_usuario]` | Elimina un usuario y desconecta todos los nodos asociados a él. |

---

## 2. Gestión de Nodos (Dispositivos Conectados)

Los nodos son los dispositivos que se han registrado y están conectados a la red Tailscale/Headscale.

| Comando | Descripción |
| :--- | :--- |
| `headscale nodes list` | Muestra una lista detallada de todos los nodos (dispositivos) registrados, incluyendo su dirección IP, usuario asociado y última conexión. |
| `headscale nodes delete [id_o_nombre]` | Elimina un nodo (dispositivo) de la red. El ID se obtiene con `headscale nodes list`. |
| `headscale nodes rename [id] [nombre_nuevo]` | Cambia el nombre de visualización de un nodo. |
| `headscale nodes expire [id_o_nombre]` | Marca un nodo como expirado, forzando una reconexión o re-autenticación. |
| `headscale nodes register [nombre_usuario] [clave_autenticacion]` | Registra un nodo en un usuario específico utilizando una clave de autenticación generada previamente. |

---

## 3. Gestión de Claves de Pre-autenticación

Las claves de pre-autenticación se utilizan para registrar clientes sin requerir una interacción manual inmediata en el servidor.

| Comando | Descripción |
| :--- | :--- |
| `headscale preauthkeys create --user [nombre_usuario]` | Genera una clave de un solo uso para un usuario específico. |
| `headscale preauthkeys create --user [nombre_usuario] --reusable` | Genera una clave que puede ser utilizada por múltiples dispositivos del mismo usuario. |
| `headscale preauthkeys create --user [nombre_usuario] --expiration [duración]` | Genera una clave con un tiempo de vida limitado (ej. `--expiration 24h`). |
| `headscale preauthkeys list` | Muestra todas las claves de pre-autenticación activas en el servidor. |
| `headscale preauthkeys revoke [clave]` | Revoca una clave de pre-autenticación específica, impidiendo su uso futuro. |

---

## 4. Configuración y Diagnóstico

| Comando | Descripción |
| :--- | :--- |
| `headscale routes list` | Muestra la lista de subredes que se están anunciando por los nodos. |
| `headscale routes enable --node [id_o_nombre] --route [subred]` | Habilita una subred anunciada por un nodo para ser utilizada por otros nodos. |
| `headscale apikeys create --expiration [duración]` | Crea una clave de API para integraciones con herramientas externas (ej. automatización). |
| `headscale apikeys list` | Muestra la lista de claves de API activas. |
| `headscale cert generate` | Utilidad para generar certificados, a menudo usado para la configuración de TLS. |