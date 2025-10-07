---
sidebar_position: 1
---
# ¿Qué es?
## Funcionamiento de Tailscale
**Tailscale** es un servicio que crea una Red Privada Virtual (VPN) de malla (mesh) basada en la tecnología WireGuard, pero que simplifica drásticamente la configuración y gestión de la red. Convierte todos tus dispositivos en nodos que pueden comunicarse de forma segura, estén donde estén.
## Nodos y la Tailnet
**Nodos:** En Tailscale, cada dispositivo (ordenador, móvil, servidor, etc.) con el cliente de Tailscale instalado y autenticado se considera un nodo.
**Tailnet:** El conjunto de todos estos nodos conectados de forma segura bajo la cuenta de Tailscale se denomina **Tailnet**. Dentro de la Tailnet, cada nodo recibe una dirección IP privada y fija de Tailscale, lo que les permite comunicarse entre sí de forma directa y cifrada.
**Estructura de Malla (Mesh):** A diferencia de las VPN tradicionales de "Hub & Spoke" (donde todo el tráfico pasa por un servidor central), Tailscale busca establecer conexiones punto a punto (peer-to-peer) entre los nodos. Esto significa que la comunicación entre, por ejemplo, un móvil y un servidor de casa, viaja directamente entre ellos, no a través de un servidor intermedio, lo que mejora la velocidad y la latencia.
## El Servidor de Coordinación (Control Server)
El componente central que gestiona la red es el Servidor de Coordinación de Tailscale (a menudo llamado Control Server). Su función es: 

- **Gestión de Identidad y Claves:** Se encarga de la autenticación de los usuarios (a través de un proveedor de identidad, como Google) y de la gestión automática de las claves criptográficas de WireGuard para cada nodo. Esto elimina la tediosa configuración manual de claves.

- **Intercambio de Metadatos:** Le dice a cada nodo qué otros nodos están en la red, cuáles son sus direcciones IP asignadas y cómo pueden contactarse (información de túneles).

- **La Clave:** Los Datos NO Pasan por el Servidor de Coordinación
Es fundamental entender que el Servidor de Coordinación solo maneja metadatos de control, NO el tráfico de datos cifrado que viaja entre los nodos.
Una vez que un nodo sabe cómo contactar a otro (gracias a la información del Servidor de Coordinación), intenta establecer una conexión directa y cifrada (WireGuard) con ese nodo.
Si la conexión directa falla (a menudo por firewalls estrictos o NAT complejos), Tailscale utiliza su red de retransmisión llamada DERP (Designated Encrypted Relay for Packets) para que el tráfico pueda llegar. Incluso cuando se utiliza DERP, el tráfico sigue estando cifrado de extremo a extremo entre los nodos, por lo que el servidor DERP solo retransmite datos cifrados sin poder leer su contenido.

- **No Abre Puertos**
Tailscale utiliza una técnica conocida como NAT Traversal (o agujereado de firewall/NAT) para establecer las conexiones punto a punto sin requerir que abras puertos en tu router.
Los nodos de Tailscale envían paquetes a través de la red intentando "descubrir" cómo llegar al otro lado del NAT o firewall. Esto se hace de forma automática y es crucial para el funcionamiento de WireGuard y Tailscale.
Dado que el tráfico se inicia desde dentro de la red (por el nodo), el firewall o router generalmente permite que la respuesta regrese, estableciendo la conexión sin necesidad de configuración manual de puertos.

- **Nodo de Salida (Exit Node)**
El Nodo de Salida o Exit Node es una característica clave para el uso de Tailscale como una VPN  para acceso remoto a Internet.
Función: Te permite dirigir todo tu tráfico de Internet público (no solo el tráfico interno de la Tailnet) a través de uno de los nodos de tu Tailnet.
Acceso Remoto a Internet: Al activar un nodo como "Nodo de Salida", puedes usarlo como tu punto de conexión a Internet, estés donde estés. Esto es útil para:
Ocultar tu IP real al navegar (la IP que verán los sitios web será la del Nodo de Salida).
Acceder a servicios con restricción geográfica desde la ubicación del Nodo de Salida.

- **Mecanismo:** Cuando configuras tu dispositivo para usar un Nodo de Salida, Tailscale actualiza la ruta predeterminada del dispositivo. En lugar de enviar el tráfico de Internet a un router local (la ruta predeterminada), lo envía a través del túnel cifrado de WireGuard hacia el Nodo de Salida seleccionado. El Nodo de Salida se encarga de enviarlo a Internet.
 




