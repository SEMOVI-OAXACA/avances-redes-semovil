# AnythingLLM
## Introducción

AnythingLLM es una plataforma de inteligencia artificial de código abierto (licencia MIT) diseñada como una aplicación todo-en-uno, enfocada en la privacidad y la operación local. Su arquitectura "local-first" (local por defecto) permite a individuos y organizaciones aprovechar las capacidades de los Modelos de Lenguaje Grandes (LLM) sin que los datos salgan de su infraestructura.

La plataforma funciona principalmente como una aplicación de escritorio (para macOS, Windows y Linux) y también ofrece una opción de despliegue multi-usuario a través de Docker. Su funcionalidad central es la Generación Aumentada por Recuperación (RAG), permitiendo a los usuarios "chatear" e interactuar con sus propios documentos, datos y repositorios de código en un entorno seguro.

## Características Principales
AnythingLLM integra un conjunto de herramientas de IA en una única interfaz, eliminando la necesidad de configuraciónes complejas.

- **Arquitectura y Privacidad por Defecto**: El diseño de la plataforma prioriza la privacidad. Por defecto, todos los componentes críticos se ejecutan localmente en la máquina del usuario:

- **Proveedor de LLM**: La versión de escritorio incluye un motor de LLM integrado que permite la descarga y ejecución de modelos populares (como Llama 3 o Phi-3) con un solo clic.

- **Motor de Embeddings**: Utiliza un modelo de embeddings local por defecto (ej. all-MiniLM-L6-v2) que se ejecuta en la CPU.

- **Base de Datos Vectorial**: Incluye una base de datos vectorial integrada (LanceDB) para almacenar los vectores de los documentos de forma local.

- **Almacenamiento**: Todos los documentos, chats y configuraciones se almacenan en el sistema de archivos local.

- **Ingesta y Gestión de Documentos (RAG)**: Permite la ingesta de una amplia variedad de tipos de documentos para construir una base de conocimiento personalizada, incluyendo:

    * Documentos PDF
    * Archivos de Microsoft Word (.docx)
    * Archivos CSV
    * Repositorios de código
    * Importación desde ubicaciones en línea

- **Soporte Flexible de Modelos (LLM)**: Además de su proveedor local integrado, AnythingLLM ofrece conectividad nativa con múltiples proveedores de modelos, tanto locales como en la nube:

    -   Proveedores Locales: Soporte para Ollama, LM Studio y otros servidores compatibles con la API de OpenAI.

    - Proveedores de Nube (Empresariales): Integración con OpenAI, Azure OpenAI, Anthropic, Google Gemini, AWS, entre otros.

## Ecosistema y Extensibilidad
-  **Agentes de IA (AI Agents)**: Permite la configuración de agentes de IA autónomos que pueden realizar tareas personalizadas y automatizar flujos de trabajo complejos dentro de la plataforma.

- **API para Desarrolladores**:AnythingLLM expone una API integrada que permite a los desarrolladores utilizar sus capacidades de RAG, gestión de modelos y chat como un servicio backend para productos personalizados o integraciones existentes.

- **Community Hub**: Dispone de un repositorio comunitario para descubrir y compartir extensiones, incluyendo:

    * Habilidades de Agente (Agent Skills): Capacidades pre-construidas para los agentes.

    * Prompts de Sistema: Plantillas de prompts para definir el comportamiento de la IA.

    * Comandos (Slash Commands): Atajos para ejecutar tareas comunes.