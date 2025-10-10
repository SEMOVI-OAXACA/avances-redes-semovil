---
sidebar_position: 1
---

# ¿Qué es Headscale y Cómo Funciona?

**Headscale** es una implementación de código abierto del **servidor de control** de Tailscale. Permite construir tu propia red privada virtual (VPN) de malla, dándote control total sobre la infraestructura de red. En lugar de depender del servicio gestionado de Tailscale, se aloja y gestiona un propio servidor de coordinación.

## Funcionamiento General

Headscale organiza una **VPN de malla (mesh)** basada en el protocolo **WireGuard**, simplificando enormemente su configuración y gestión. Convierte todos los dispositivos en **nodos** capaces de comunicarse de forma segura y directa, sin importar su ubicación física.

### Servidor de Coordinación (Tu Instancia de Headscale)

 Su función es administrar las conexiones, pero **nunca transmite los datos cifrados de los nodos**. El tráfico de datos viaja directamente entre ellos.

Las funciones principales de Headscale son:

1.  **Gestión de Identidad y Claves:** Autentica los nodos (usualmente mediante claves pre-autenticadas que genera) y distribuye de forma segura las claves públicas de WireGuard entre ellos.
2.  **Intercambio de Metadatos:** Informa a cada nodo sobre los demás nodos de la red, sus direcciones IP públicas y las rutas necesarias para establecer una conexión.
3.  **Facilitar Conexiones Directas:** Una vez que los nodos tienen la información de Headscale, intentan conectarse directamente entre sí usando WireGuard.

### Servidores DERP (Designated Encrypted Relay for Packets)

Si una conexión directa punto a punto falla (generalmente debido a configuraciones de NAT complejas o firewalls restrictivos), el tráfico se retransmite a través de un servidor **DERP**.

  - Con Headscale, se tiene la flexibilidad de usar la red DERP pública de Tailscale o, para un control total, **desplegar servidores DERP propios**.
  - Los paquetes siempre viajan **cifrados de extremo a extremo**. El servidor DERP solo retransmite el tráfico, pero no puede descifrarlo.

### NAT Traversal (Agujereado de NAT)

La red no requiere abrir puertos en el router para que los nodos se conecten. Los clientes de Tailscale utilizan técnicas avanzadas de **NAT Traversal** para descubrirse mutuamente y establecer conexiones seguras, iniciando el tráfico desde dentro de sus respectivas redes locales.

## Diagrama de Funcionamiento con Headscale

### 🔁 Flujo General con Headscale

> 1.  Cada nodo se registra y autentica con **el servidor Headscale**.
> 2.  El servidor Headscale les informa cómo contactarse entre sí (direcciones IP, claves públicas, etc.).
> 3.  Los nodos intentan establecer una conexión **directa P2P** (sin necesidad de abrir puertos).
> 4.  Si la conexión directa no es posible, utilizan un **relevo DERP** (público o el propio).
> 5.  Si se configura un **Nodo de Salida**, los demás nodos pueden enrutar todo su tráfico de internet a través de él de forma segura.