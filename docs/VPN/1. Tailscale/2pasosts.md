# Instalación y Configuración de Tailscale

Para comenzar a utilizar **Tailscale**, sigue los pasos descritos a continuación:

---

## 1. Crear una cuenta institucional
Crea una **cuenta institucional en Tailscale**, que servirá como punto central para conectar, autenticar y administrar los dispositivos y usuarios dentro de la red privada (**Tailnet**).

---

## 2. Instalar Tailscale en los dispositivos
Instala la aplicación de **Tailscale** en los equipos que formarán parte de la red: **Windows**, **macOS**, **Linux**, **Android** o **iOS**.

### Instalación en un servidor Linux
Ejecuta el siguiente comando para instalar Tailscale:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```
Una vez instalado, inicia sesión con la cuenta institucional:
```bash
sudo tailscale login
```
El sistema generará una URL de autenticación, la cual deberás abrir en un navegador para registrar el servidor dentro de la Tailnet.

---

## 3. Configurar el servidor como Exit Node

Un Exit Node permite que otros dispositivos enruten todo su tráfico de Internet a través del servidor, actuando como una puerta de salida segura.

### Pasos de configuración:

- Iniciar Tailscale:
```bash
sudo tailscale up
```
- Verificar el estado de conexión:
Comprueba que el cliente esté conectado a la Tailnet y tenga asignada una IP de Tailscale.
```bash
tailscale status
```
- Detener Tailscale (opcional):
```bash
tailscale down
```
- Habilitar Subnet Router:
Permite que los usuarios remotos accedan a la red interna del servidor.
```bash
sudo tailscale up --advertise-routes=10.0.2.0/24
```
- Activar como Exit Node:
Permite que todo el tráfico de Internet de otros dispositivos pase a través del servidor.
```bash
sudo tailscale up --advertise-exit-node
```

Con estos pasos, el servidor quedará integrado a la red de Tailscale y podrá actuar como nodo de salida o router de subred, según las necesidades.