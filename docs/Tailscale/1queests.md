---
sidebar_position: 1
---
# ¿Qué es Tailscale?

## Funcionamiento General
**Tailscale** es un servicio que crea una **VPN de malla (mesh)** basada en **WireGuard**, simplificando su configuración y gestión. Convierte todos tus dispositivos en **nodos** capaces de comunicarse de forma segura, sin importar su ubicación.

---

## Nodos y la Tailnet
- **Nodos:** Cada dispositivo con el cliente de Tailscale instalado y autenticado.  
- **Tailnet:** Conjunto de nodos conectados bajo una misma cuenta, cada uno con una IP privada fija.  
- **Estructura Mesh:** En lugar de un servidor central (como en las VPN tradicionales), los nodos se conectan **punto a punto (peer-to-peer)** para mejorar velocidad y privacidad.

---

## Servidor de Coordinación (Control Server)
El **Control Server** administra la red, pero **no transmite los datos cifrados**, solo la información de control.

### Funciones principales:
- **Gestión de identidad y claves:** Autenticación mediante proveedores (ej. Google) y manejo automático de claves WireGuard.  
- **Intercambio de metadatos:** Informa a cada nodo sobre los demás nodos, direcciones IP y rutas disponibles.  
- **Conexiones directas:** Una vez obtenida la información, los nodos intentan conectarse directamente usando WireGuard.

---

## DERP (Designated Encrypted Relay for Packets)
Si una conexión directa falla (por NAT o firewalls), Tailscale usa su red **DERP** para retransmitir los datos **sin descifrar** el contenido.  
Los paquetes permanecen **cifrados de extremo a extremo**.

---

## NAT Traversal (Agujereado de NAT)
Tailscale **no requiere abrir puertos** en el router.  
Emplea técnicas automáticas para que los nodos se descubran y establezcan conexiones seguras, iniciando el tráfico desde dentro de la red.

---

## Nodo de Salida (Exit Node)
Permite usar un nodo como **punto de acceso a Internet**, redirigiendo todo el tráfico público por él.

### Beneficios:
- **Privacidad:** Oculta tu IP real.  
- **Acceso remoto:** Permite navegar como si estuvieras en la ubicación del nodo (útil para restricciones geográficas).

### Funcionamiento:
Al activar un Nodo de Salida, Tailscale cambia la **ruta predeterminada** del dispositivo, enviando todo el tráfico de Internet a través del túnel cifrado hacia el nodo seleccionado, que luego lo reenvía a Internet.

---

