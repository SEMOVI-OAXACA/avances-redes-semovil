### **Asunto:** Plantilla de Despliegue: AnythingLLM con Proxy Inverso Traefik en AWS

---

### 1. Objetivo del Despliegue

El objetivo de este proyecto fue implementar la plataforma **AnythingLLM** en una instancia de **Ubuntu Linux en un proveedor de nube (ej. AWS EC2)**. La implementación requería que la aplicación fuera accesible públicamente a través de un dominio específico, utilizando **Traefik** como proxy inverso para gestionar automáticamente el tráfico y la provisión de certificados **SSL/TLS (HTTPS)** mediante Let's Encrypt.

### 2. Configuración de Prerrequisitos

Previo al despliegue de software, se validaron los siguientes prerrequisitos de infraestructura:

* **Instancia de Servidor:** Una instancia de servidor con sistema operativo Ubuntu.
* **Resolución de DNS:** Un registro DNS de tipo `A` para `tudominio.com`, apuntando a la dirección IP pública del servidor.
* **Reglas de Firewall (Grupo de Seguridad):** Se configuraron las reglas de entrada para permitir el tráfico en los siguientes puertos:
    * **TCP 80 (HTTP):** Requerido para el desafío `http-01` de Let's Encrypt.
    * **TCP 443 (HTTPS):** Para el acceso seguro a la aplicación.
    * **TCP 22 (SSH):** Para la administración del servidor.

### 3. Instalación del Entorno de Contenedores

Se instalaron los componentes necesarios para la orquestación de contenedores:

1.  **Actualización del Sistema:** Se actualizaron los repositorios de paquetes con `sudo apt update`.
2.  **Instalación de Docker:** Se instalaron el motor de Docker (`docker.io`) y Docker Compose (`docker-compose`).
3.  **Gestión de Permisos:** Se resolvió un error común de permisos (`permission denied... docker.sock`) añadiendo el usuario actual al grupo `docker` mediante el comando `sudo usermod -aG docker $USER`, seguido de un reinicio de la sesión SSH para aplicar los cambios.


### 4. Configuración de Traefik (traefik.yml)

Se definió un archivo de configuración estática para Traefik, instruyéndole:
* Definir los puntos de entrada (EntryPoints) `http` (puerto 80) y `httpss` (puerto 443).
* Implementar una redirección global permanente de HTTP a HTTPS.
* Habilitar el proveedor Docker para la detección automática de contenedores.
* Configurar el *resolver* de certificados `letsencrypt` (ACME), especificando el email de contacto y la ruta de almacenamiento `acme.json`.

Se creó el archivo `traefik_data/acme.json` y se le asignaron permisos restrictivos (`chmod 600`) por seguridad.

**Contenido de `traefik.yml`:**
```yaml
global:
  checkNewVersion: true
  sendAnonymousUsage: false

# Define los puntos de entrada (HTTP y HTTPS)
entryPoints:
  http:
    address: ":80"
    # Redirección global de HTTP a HTTPS
    http:
      redirections:
        entryPoint:
          to: "httpss"
          scheme: "https"
  https:
    address: ":443"

# Habilita el proveedor de Docker para que Traefik detecte contenedores
providers:
  docker:
    endpoint: "unix:///var/run/docker.sock"
    exposedByDefault: false # Solo expondremos contenedores con etiquetas

# Define el "resolvedor" de certificados (Let's Encrypt)
certificatesResolvers:
  letsencrypt:
    acme:
      email: "tu-email-de-contacto@dominio.com" # CAMBIAR ESTO
      storage: "/etc/traefik/acm/acme.json"
      httpChallenge:
        entryPoint: "http"
```


#### 4.1. Variables de Entorno (.env)
Se creó un archivo `.env` para configurar AnythingLLM. Se generaron valores criptográficos aleatorios (vía openssl rand -base64) para las claves de seguridad críticas.

Contenido de .env (plantilla):
```yaml
Ini, TOML

# --- Configuración del Servidor ---
# Puerto interno en el que corre AnythingLLM (no cambiar)
SERVER_PORT=3001

# --- Ruta de Almacenamiento ---
# Directorio interno para guardar todos los datos (¡MUY IMPORTANTE!)
STORAGE_DIR=/app/server/storage

# --- Claves de Seguridad ---
# Valores generados aleatoriamente para asegurar la instancia
ENCRYPTION_KEY_SECRET="[VALOR_GENERADO_1]"
JWT_SECRET="[VALOR_GENERADO_2]"
SIG_KEY="[VALOR_GENERADO_3]"
SIG_SALT="[VALOR_GENERADO_4]"

# --- Configuración de LLM por Defecto (Opcional) ---
# Define "gemini" como el proveedor de LLM por defecto al iniciar
DEFAULT_LLM=gemini

# Clave de API para el proveedor de LLM
GEMINI_API_KEY="[TU_CLAVE_API_DE_GEMINI]"

# Modelo de Gemini a usar
GEMINI_LLM_MODEL_NAME=gemini-1.5-pro-latest
```

### 5. Orquestación de Servicios (docker-compose.yml)
Se definió un archivo docker-compose.yml para orquestar los servicios traefik y anythingllm, conectándolos a través de una red proxy_net y utilizando etiquetas (labels) para el enrutamiento dinámico.

Contenido de docker-compose.yml:

```YAML

version: '3.8'

services:
  # --- Servicio de Traefik (Reverse Proxy) ---
  traefik:
    image: "traefik:v2.11"
    container_name: "traefik"
    ports:
      - "80:80"   # Puerto HTTP
      - "443:443" # Puerto HTTPS
    volumes:
      # Monta el archivo de config estática
      - "./traefik.yml:/etc/traefik/traefik.yml:ro"
      # Monta la carpeta de certificados
      - "./traefik_data:/etc/traefik/acm"
      # Monta el socket de Docker para detectar contenedores
      - "/var/run/docker.sock:/var/run/docker.sock:ro"
    networks:
      - proxy_net
    restart: unless-stopped

  # --- Servicio de AnythingLLM ---
  anythingllm:
    image: mintplexlabs/anythingllm
    container_name: "anythingllm"
    # Carga las variables de entorno del archivo .env
    env_file: .env
    volumes:
      # Monta el volumen para persistir los datos de AnythingLLM
      - "./storage:/app/server/storage"
    networks:
      - proxy_net
    restart: unless-stopped
    # --- Etiquetas de Traefik para AnythingLLM ---
    labels:
      - "traefik.enable=true" # Habilita Traefik para este contenedor
      # Define el router para el tráfico HTTPS
      - "traefik.http.routers.anythingllm.rule=Host(`tudominio.com`)" # CAMBIAR ESTO
      - "traefik.http.routers.anythingllm.entrypoints=httpss"
      - "traefik.http.routers.anythingllm.tls.certresolver=letsencrypt" # Usa el resolvedor de Let's Encrypt
      # Define el servicio y el puerto interno de AnythingLLM (que es 3001)
      - "traefik.http.services.anythingllm.loadbalancer.server.port=3001"
      # Middleware para asegurar que se usen headers de SSL
      - "traefik.http.middlewares.sslheader.headers.customrequestheaders.X-Forwarded-Proto=https"
      - "traefik.http.routers.anythingllm.middlewares=sslheader@docker"

networks:
  proxy_net: # Red interna para que los contenedores se comuniquen
    name: proxy_net
```

### 6. Despliegue y Configuración Final
Ejecución: Los servicios se iniciaron en modo desacoplado (-d) utilizando `sudo docker-compose up -d.`


Verificación: Se monitorearon los registros de Traefik, confirmando la correcta generación del certificado SSL para el dominio configurado. Los registros de AnythingLLM mostraron una conexión exitosa con la API del LLM (tras resolver un error 400 inicial, comúnmente relacionado con la facturación de la cuenta de API).

### 7. Personalización (Branding):
Se accedió al panel de administración de AnythingLLM y se personalizó la apariencia (ej. favicon), utilizando una URL pública directa a un archivo de imagen.
