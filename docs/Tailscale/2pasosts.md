# Instalación y configuración de Tailscale

Para utilizar **Tailscale**, es necesario seguir los siguientes pasos:

1. **Crear una cuenta institucional en Tailscale**, la cual servirá como punto central para conectar y administrar las cuentas de los usuarios.  
2. **Instalar la aplicación de Tailscale** en los dispositivos que formarán parte de la red, ya sean **Android**, **Windows**, **macOS** o **Linux**. 
Si se desea instalar Tailscale en un servidor, se hace mediante la siguiente línea de comandos:

```
 curl -fsSL https://tailscale.com/install.sh | sh 
 ```

3. Configurar el servidor como **Exit Node**
    - Encender Tailscale

    ```
    sudo tailscale up
    ```

    - Comprobar que el cliente este conectado a Tailscale confirmar que el equipo remoto está en la tailnet y tiene IP Tailscale.

    ```
    tailscale status
    ```

    - Apagar tailscale

    ```
    tailscale down
    ```

    - Activar como Subnet Router permitirá que los usuarios remotos accedan a la red interna

    ```
    tailscale up --advertise-routes=10.0.2.0/24
    ```

    - Activar el modo exit node Permite que todo el tráfico de Internet de otros dispositivos pase a través de este servidor, no solo el tráfico de red interna.

    ```
    tailscale up--advertise-exit-node
    ```
    
