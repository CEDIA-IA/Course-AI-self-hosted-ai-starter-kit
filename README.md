# Self-hosted AI starter kit

**Self-hosted AI Starter Kit** es una plantilla open-source de Docker Compose diseñada para inicializar rápidamente un entorno local integral de AI y desarrollo low-code.

![Terminal- Screenshot](https://raw.githubusercontent.com/n8n-io/self-hosted-ai-starter-kit/main/assets/n8n-demo.gif)


### Qué incluye

✅ [**Self-hosted n8n**](https://n8n.io/) - Plataforma low-code con más de 400
integraciones y componentes avanzados de AI

✅ [**Ollama**](https://ollama.com/) - Plataforma LLM multiplataforma para instalar
y ejecutar los LLMs locales más recientes

✅ [**Qdrant**](https://qdrant.tech/) - Vector store open-source de alto rendimiento
con una API integral

✅ [**PostgreSQL**](https://www.postgresql.org/) -  Caballo de batalla del mundo de
Data Engineering, maneja grandes cantidades de datos de forma segura.

✅ [**Langfuse**](https://langfuse.com/) - Plataforma open-source de LLM observability para hacer tracing, monitorear, evaluar y depurar aplicaciones de AI.

### Qué puedes crear

- **AI Agents** para agendar citas

- **Resumir PDFs de la empresa** de forma segura sin fugas de datos

-  **Bots para Trabajos inteligentes** para mejorar las comunicaciones de la empresa y las operaciones de IT

- **Análisis privado de documentos financieros** con un costo mínimo

## Instalación

### Clonar el repositorio

```bash
git clone https://github.com/CEDIA-IA/Course-AI-self-hosted-ai-starter-kit.git
cd Course-AI-self-hosted-ai-starter-kit
cp .env.example .env # debes actualizar los secrets y passwords dentro
```

### Ejecutar n8n usando Docker Compose

### Configuración de GPU dentro de Docker
 
> [Instrucciones para Configurar GPU dentro de Docker](https://docs.ollama.com/docker).

#### Para usuarios de Nvidia GPU

```bash
git clone https://github.com/CEDIA-IA/Course-AI-self-hosted-ai-starter-kit.git
cd Course-AI-self-hosted-ai-starter-kit
cp .env.example .env # debes actualizar los secrets y passwords dentro
docker compose --profile gpu-nvidia up
```

### Para usuarios de AMD GPU en Linux

```bash
git clone https://github.com/CEDIA-IA/Course-AI-self-hosted-ai-starter-kit.git
cd Course-AI-self-hosted-ai-starter-kit
cp .env.example .env # debes actualizar los secrets y passwords dentro
docker compose --profile gpu-amd up
```

#### Para usuarios de Mac / Apple Silicon

Si estás usando una Mac con un procesador M1 o más reciente, lamentablemente no puedes exponer tu GPU
a la instancia de Docker. En este caso hay dos opciones:

1. Ejecutar el starter kit completamente en CPU, como en la sección "Para todos los demás"
   más abajo
2. Ejecutar Ollama en tu Mac para una inferencia más rápida, y conectarte a eso desde la instancia de n8n

Si quieres ejecutar Ollama en tu Mac, revisa la
[página principal de Ollama](https://ollama.com/)
para ver las instrucciones de instalación, y ejecuta el starter kit de la siguiente manera:

```bash
git clone https://github.com/CEDIA-IA/Course-AI-self-hosted-ai-starter-kit.git
cd Course-AI-self-hosted-ai-starter-kit
cp .env.example .env # debes actualizar los secrets y passwords dentro
docker compose up
```

##### Para usuarios de Mac que ejecutan OLLAMA localmente

Si estás ejecutando OLLAMA localmente en tu Mac (no en Docker), necesitas modificar la variable de entorno OLLAMA_HOST

1. Establece OLLAMA_HOST en `host.docker.internal:11434` en tu archivo .env.
2. Además, después de ver "Editor is now accessible via: [http://localhost:5678/](http://localhost:5678/)":

   1. Ve a [http://localhost:5678/home/credentials](http://localhost:5678/home/credentials)
   2. Haz clic en "Local Ollama service"
   3. Cambia la base URL a "[http://host.docker.internal:11434/](http://host.docker.internal:11434/)"

#### Para todos los demás

```bash
git clone https://github.com/CEDIA-IA/Course-AI-self-hosted-ai-starter-kit.git
cd Course-AI-self-hosted-ai-starter-kit
cp .env.example .env # debes actualizar los secrets y passwords dentro
docker compose --profile cpu up
```

## ⚡️ Inicio rápido y uso

El núcleo del Self-hosted AI Starter Kit es un archivo Docker Compose, preconfigurado con ajustes de red y almacenamiento, minimizando la necesidad de instalaciones adicionales.
Después de completar los pasos de instalación anteriores, simplemente sigue los pasos a continuación para comenzar.

1. Abre [http://localhost:5678/](http://localhost:5678/) en tu navegador para configurar n8n. Solo tendrás
   que hacer esto una vez.
2. Abre el workflow incluido:
   [http://localhost:5678/workflow/srOnR8PAY3u4RSwb](http://localhost:5678/workflow/srOnR8PAY3u4RSwb)
3. Haz clic en el botón **Chat** en la parte inferior del canvas para comenzar a ejecutar el workflow.
4. Si esta es la primera vez que ejecutas el workflow, puede que necesites esperar
   hasta que Ollama termine de descargar Llama3.2. Puedes inspeccionar los logs de la consola
   de Docker para revisar el progreso.

Para abrir n8n en cualquier momento, visita [http://localhost:5678/](http://localhost:5678/) en tu navegador.

Con tu instancia de n8n, tendrás acceso a más de 400 integraciones y a un
conjunto de nodos de AI básicos y avanzados como
[AI Agent](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.agent/),
[Text classifier](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.text-classifier/),
y [Information Extractor](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.information-extractor/)
nodes. Para mantener todo local, solo recuerda usar el nodo Ollama para tu
language model y Qdrant como tu vector store.

> [!NOTE]
> Este starter kit está diseñado para ayudarte a comenzar con workflows de AI
> self-hosted. Aunque no está completamente optimizado para entornos de producción, combina
> componentes robustos que funcionan bien juntos para proyectos proof-of-concept.
> Puedes personalizarlo para satisfacer tus necesidades específicas

## Actualización

* ### Para configuraciones con Nvidia GPU:

```bash
docker compose --profile gpu-nvidia pull
docker compose create && docker compose --profile gpu-nvidia up
```

* ### Para usuarios de Mac / Apple Silicon

```bash
docker compose pull
docker compose create && docker compose up
```

* ### Para configuraciones sin GPU:

```bash
docker compose --profile cpu pull
docker compose create && docker compose --profile cpu up
```

## 👓 Lectura recomendada

n8n está lleno de contenido útil para comenzar rápidamente con sus conceptos
y nodos de AI. Si encuentras un problema, ve a [soporte](#support).

* [AI agents para desarrolladores: de la teoría a la práctica con n8n](https://blog.n8n.io/ai-agents/)
* [Tutorial: Crea un workflow de AI en n8n](https://docs.n8n.io/advanced-ai/intro-tutorial/)
* [Conceptos de Langchain en n8n](https://docs.n8n.io/advanced-ai/langchain/langchain-n8n/)
* [Demostración de diferencias clave entre agents y chains](https://docs.n8n.io/advanced-ai/examples/agent-chain-comparison/)
* [¿Qué son las vector databases?](https://docs.n8n.io/advanced-ai/examples/understand-vector-databases/)

## 🎥 Video walkthrough

* [Instalación y uso de Local AI para n8n](https://www.youtube.com/watch?v=xz_X2N-hPg0)

## 🛍️ Más templates de AI

Para más ideas de workflows de AI, visita la [**galería oficial de templates de AI de n8n
**](https://n8n.io/workflows/categories/ai/). Desde cada workflow,
selecciona el botón **Use workflow** para importar automáticamente el workflow a
tu instancia local de n8n.

### Aprende conceptos clave de AI

* [AI Agent Chat](https://n8n.io/workflows/1954-ai-agent-chat/)
* [AI chat con cualquier data source (usando también la herramienta de workflow de n8n)](https://n8n.io/workflows/2026-ai-chat-with-any-data-source-using-the-n8n-workflow-tool/)
* [Chat con OpenAI Assistant (agregando una memory)](https://n8n.io/workflows/2098-chat-with-openai-assistant-by-adding-a-memory/)
* [Usa un LLM open-source (vía Hugging Face)](https://n8n.io/workflows/1980-use-an-open-source-llm-via-huggingface/)
* [Chat con docs PDF usando AI (citando sources)](https://n8n.io/workflows/2165-chat-with-pdf-docs-using-ai-quoting-sources/)
* [AI agent que puede scrape webpages](https://n8n.io/workflows/2006-ai-agent-that-can-scrape-webpages/)

### Templates de Local AI

* [Tax Code Assistant](https://n8n.io/workflows/2341-build-a-tax-code-assistant-with-qdrant-mistralai-and-openai/)
* [Desglosar documentos en Study Notes con MistralAI y Qdrant](https://n8n.io/workflows/2339-breakdown-documents-into-study-notes-using-templating-mistralai-and-qdrant/)
* [Financial Documents Assistant usando Qdrant y](https://n8n.io/workflows/2335-build-a-financial-documents-assistant-using-qdrant-and-mistralai/) [Mistral.ai](http://mistral.ai/)
* [Recipe Recommendations con Qdrant y Mistral](https://n8n.io/workflows/2333-recipe-recommendations-with-qdrant-and-mistral/)

## Tips & tricks

### Acceder a archivos locales

El self-hosted AI starter kit creará una carpeta compartida (por defecto,
ubicada en el mismo directorio) que se monta en el contenedor de n8n y
permite que n8n acceda a archivos en disco. Esta carpeta dentro del contenedor de n8n está
ubicada en `/data/shared` -- esta es la ruta que necesitarás usar en los nodos que
interactúan con el filesystem local.

**Nodos que interactúan con el filesystem local**

* [Read/Write Files from Disk](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.filesreadwrite/)
* [Local File Trigger](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.localfiletrigger/)
* [Execute Command](https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.executecommand/)
