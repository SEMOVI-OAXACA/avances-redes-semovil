---
id: prueba-tailscale
title: Prueba de Conectividad con Tailscale
sidebar_label: Prueba de Tailscale
---

# 🧪 Prueba de Conectividad con Tailscale

En esta sección se documenta la **prueba realizada con Tailscale** para verificar la conectividad segura entre diferentes nodos dentro de la red de SEMOVI.  
El objetivo fue comprobar la capacidad de **Tailscale** para establecer una red privada virtual (VPN) sin la necesidad de abrir puertos, simplificando la comunicación entre servidores y equipos remotos.

---

## 🎯 Objetivo General

Validar el funcionamiento de **Tailscale** como herramienta de interconexión segura entre servidores y estaciones de trabajo distribuidas en distintas ubicaciones, sin requerir configuraciones de firewall complejas.

---

## ⚙️ Configuración Inicial

### Equipos utilizados

| Dispositivo | Sistema Operativo | Rol dentro de la prueba |
|--------------|------------------|--------------------------|
| Servidor Principal | Ubuntu Server 22.04 | Servidor DNS y aplicación interna |
| Equipo Cliente 1 | Windows 10 Pro | Nodo de administración remota |
| Equipo Cliente 2 | xiaomi | Nodo de monitoreo y pruebas |

### Requisitos previos

- Tener una cuenta activa en [Tailscale](https://tailscale.com/)
- Permisos administrativos en cada equipo
- Acceso a Internet
- No se requieren puertos abiertos en el firewall local
- Conocimientos básicos de red y terminal

---

## 🧩 Procedimiento de la Prueba

### 1. Instalación de Tailscale

**En Ubuntu/Debian:**
```bash
sudo apt update
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up 
```

**En Windows:**

Se descargó el instalador desde https://tailscale.com/download

Se ejecutó y se inició sesión con la cuenta institucional

El dispositivo apareció automáticamente en el panel de administración


### 2. Autenticación y Vinculación

Cada equipo se autenticó usando la misma cuenta de Tailscale, quedando registrados como nodos dentro de la misma red virtual.

El panel de administración mostró correctamente los dispositivos conectados con direcciones del rango 100.x.x.x asignadas por Tailscale.

![Equipos conectados a Tailscale](/img/Captura2.PNG)



### 4. Activación de Exit Node

Uno de los dispositivos (el Servidor Principal) se configuró como Exit Node, lo cual permite enrutar el tráfico de otros equipos a través de él:

En el servidor principal:
```bash
sudo tailscale up --advertise-routes=100.0.2.0/24
``` 
Salida servidor monitoreo:

![Servidor Exit Node](/img/Servidor-exit-node.PNG)


