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
mkdir -p shared
```

`.env.example` trae valores de ejemplo (`POSTGRES_PASSWORD=password`,
`N8N_ENCRYPTION_KEY=super-secret-key`, etc.). Reemplaza al menos `POSTGRES_PASSWORD`,
`N8N_ENCRYPTION_KEY` y `N8N_USER_MANAGEMENT_JWT_SECRET` en tu `.env` con valores
aleatorios, por ejemplo con:

```bash
openssl rand -base64 32
```

### Ejecutar n8n usando Docker Compose

### Configuración de GPU dentro de Docker
 
> [Instrucciones para Configurar GPU dentro de Docker](https://docs.ollama.com/docker).

**Importante**: Docker **no instala drivers de GPU automáticamente**. Antes de levantar el
stack necesitas tener lo siguiente instalado en tu sistema (no el CUDA Toolkit completo,
solo lo que se indica):

| Escenario | Qué instalar ANTES en el host | ¿CUDA Toolkit completo? |
|---|---|---|
| NVIDIA GPU en Windows (WSL2) | Driver NVIDIA reciente con soporte WSL2 + Docker Desktop con GPU support habilitado | No — la imagen de Ollama ya incluye el runtime CUDA necesario |
| NVIDIA GPU en Linux nativo | Driver NVIDIA + [NVIDIA Container Toolkit](https://docs.ollama.com/docker) (`nvidia-ctk runtime configure`) | No |
| AMD GPU en Linux (ROCm) | Driver/kernel ROCm que exponga `/dev/kfd` y `/dev/dri` | No aplica (usa ROCm) |
| Mac / Apple Silicon | Nada — Docker no puede exponer la GPU de Apple | No aplica |
| Sin GPU, GPU no-NVIDIA, o no quieres instalar drivers | Nada extra, solo Docker | No — todo corre en CPU |

Si tu GPU no es NVIDIA (por ejemplo GPU integrada Intel) o no quieres instalar drivers,
usa directamente el perfil `cpu` (sección "Para todos los demás" más abajo) — no requiere
ninguna configuración adicional, solo es más lento en la inferencia.

#### Para usuarios de Nvidia GPU (Windows con WSL2 o Linux nativo)

```bash
git clone https://github.com/CEDIA-IA/Course-AI-self-hosted-ai-starter-kit.git
cd Course-AI-self-hosted-ai-starter-kit
cp .env.example .env # debes actualizar los secrets y passwords dentro
mkdir -p shared
docker compose --profile gpu-nvidia up
```

> ✅ **Descarga automática de modelo**: al usar `--profile gpu-nvidia`, el contenedor
> `ollama-pull-model-gpu` descarga automáticamente el modelo definido en
> `DEFAULT_OLLAMA_MODEL` (por defecto `qwen3.5:2b`). Ver
> [Modelo de Ollama por defecto](#-modelo-de-ollama-por-defecto).

### Para usuarios de AMD GPU en Linux

```bash
git clone https://github.com/CEDIA-IA/Course-AI-self-hosted-ai-starter-kit.git
cd Course-AI-self-hosted-ai-starter-kit
cp .env.example .env # debes actualizar los secrets y passwords dentro
mkdir -p shared
docker compose --profile gpu-amd up
```

> ✅ **Descarga automática de modelo**: al usar `--profile gpu-amd`, el contenedor
> `ollama-pull-model-gpu-amd` descarga automáticamente el modelo definido en
> `DEFAULT_OLLAMA_MODEL`. Ver [Modelo de Ollama por defecto](#-modelo-de-ollama-por-defecto).

#### Para usuarios de Mac / Apple Silicon

Si estás usando una Mac con un procesador M1 o más reciente, lamentablemente no puedes exponer tu GPU
a la instancia de Docker. En este caso hay dos opciones:

1. Ejecutar el starter kit completamente en CPU, usando el perfil `cpu` (**recomendado para
   la mayoría de estudiantes** — incluye descarga automática de modelo, ver más abajo)
2. Ejecutar Ollama en tu Mac para una inferencia más rápida, y conectarte a eso desde la instancia de n8n
   (más rápido, pero sin descarga automática de modelo)

**Opción 1 — todo en Docker (recomendado):**

```bash
git clone https://github.com/CEDIA-IA/Course-AI-self-hosted-ai-starter-kit.git
cd Course-AI-self-hosted-ai-starter-kit
cp .env.example .env # debes actualizar los secrets y passwords dentro
mkdir -p shared
docker compose --profile cpu up
```

> ✅ **Descarga automática de modelo**: la imagen `ollama/ollama` soporta Apple Silicon
> (linux/arm64) de forma nativa. Con `--profile cpu`, el contenedor `ollama-pull-model-cpu`
> descarga automáticamente el modelo de `DEFAULT_OLLAMA_MODEL` (por defecto `qwen3.5:2b`),
> igual que en Windows/Linux. Ver [Modelo de Ollama por defecto](#-modelo-de-ollama-por-defecto).

**Opción 2 — Ollama nativo en macOS (más rápido, sin descarga automática):**

Revisa la [página principal de Ollama](https://ollama.com/) para instalarlo en tu Mac, y
ejecuta el starter kit sin ningún perfil:

```bash
git clone https://github.com/CEDIA-IA/Course-AI-self-hosted-ai-starter-kit.git
cd Course-AI-self-hosted-ai-starter-kit
cp .env.example .env # debes actualizar los secrets y passwords dentro
mkdir -p shared
docker compose up
```

> ⚠️ **Sin `--profile`, ningún contenedor de Ollama se levanta dentro de Docker** (ni el de
> pull automático). Debes descargar el modelo tú mismo en tu Mac antes de usar el workflow:
> `ollama pull qwen3.5:2b` (o el modelo que hayas elegido).

##### Para usuarios de Mac que ejecutan OLLAMA localmente

Si estás ejecutando OLLAMA localmente en tu Mac (no en Docker), necesitas modificar la variable de entorno OLLAMA_HOST

1. Establece OLLAMA_HOST en `host.docker.internal:11434` en tu archivo .env.
2. Además, después de ver "Editor is now accessible via: [http://localhost:5678/](http://localhost:5678/)":

   1. Ve a [http://localhost:5678/home/credentials](http://localhost:5678/home/credentials)
   2. Haz clic en "Local Ollama service"
   3. Cambia la base URL a "[http://host.docker.internal:11434/](http://host.docker.internal:11434/)"

#### Para todos los demás (Windows/Linux sin GPU, o que no quieren instalar drivers)

```bash
git clone https://github.com/CEDIA-IA/Course-AI-self-hosted-ai-starter-kit.git
cd Course-AI-self-hosted-ai-starter-kit
cp .env.example .env # debes actualizar los secrets y passwords dentro
mkdir -p shared
docker compose --profile cpu up
```

> ✅ **Descarga automática de modelo**: igual que en los demás perfiles, `ollama-pull-model-cpu`
> descarga automáticamente `DEFAULT_OLLAMA_MODEL` — no necesitas GPU, driver, ni CUDA/ROCm
> para que esto funcione. Ver [Modelo de Ollama por defecto](#-modelo-de-ollama-por-defecto).

## ⚡️ Inicio rápido y uso

El núcleo del Self-hosted AI Starter Kit es un archivo Docker Compose, preconfigurado con ajustes de red y almacenamiento, minimizando la necesidad de instalaciones adicionales.
Después de completar los pasos de instalación anteriores, simplemente sigue los pasos a continuación para comenzar.

1. Abre [http://localhost:5678/](http://localhost:5678/) en tu navegador para configurar n8n. Solo tendrás
   que hacer esto una vez.
2. Abre el workflow incluido:
   [http://localhost:5678/workflow/srOnR8PAY3u4RSwb](http://localhost:5678/workflow/srOnR8PAY3u4RSwb)
3. Haz clic en el botón **Chat** en la parte inferior del canvas para comenzar a ejecutar el workflow.
4. Si esta es la primera vez que levantas el stack con un perfil (`cpu`, `gpu-nvidia` o
   `gpu-amd`), Ollama descargará automáticamente el modelo definido en `DEFAULT_OLLAMA_MODEL`
   (ver sección [Modelo de Ollama por defecto](#-modelo-de-ollama-por-defecto) más abajo).
   Puedes inspeccionar los logs del contenedor `ollama-pull-model-*` para revisar el progreso.
   Si dejaste `DEFAULT_OLLAMA_MODEL` vacío, no se descargará nada automáticamente y deberás
   bajar un modelo manualmente con `docker exec ollama ollama pull <modelo>` antes de usar el workflow.

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

## 🤖 Modelo de Ollama por defecto

Al levantar el stack con `docker compose --profile cpu up` (o `gpu-nvidia` / `gpu-amd`),
un contenedor auxiliar (`ollama-pull-model-*`) descarga automáticamente el modelo indicado
en la variable `DEFAULT_OLLAMA_MODEL` de tu archivo `.env`. Si dejas esa variable vacía
(`DEFAULT_OLLAMA_MODEL=`), **no se descarga ningún modelo** — útil si no tienes GPU y
prefieres no levantar un modelo local, o si vas a gestionar los modelos manualmente.

### Modelos recomendados para VRAM limitada (4-6 GB)

Pensado para que estudiantes con laptops modestas (GPU de 4-6 GB de VRAM, o incluso solo CPU)
puedan levantar el kit sin problema:

| Modelo | Tag | Descarga | RAM (solo CPU) | VRAM (con GPU) | Uso recomendado |
|---|---|---|---|---|---|
| **Qwen3.5 2B** ⭐ | `qwen3.5:2b` | 2.7 GB | ~6 GB | ~3.5-4 GB | **Por defecto.** Soporta tool-calling (necesario para los nodos AI Agent de n8n), visión y contexto de 256K |
| Qwen3.5 0.8B | `qwen3.5:0.8b` | 1.0 GB | ~3 GB | ~1.5-2 GB | Para equipos muy limitados o sin GPU dedicada |
| Qwen3.5 4B | `qwen3.5:4b` | 3.4 GB | ~8 GB | ~4.5-5 GB | Si tienes 6 GB+ de VRAM y quieres mejor calidad |
| MiniCPM-V 4.6 | `minicpm-v4.6:1b` | 1.6 GB | ~4 GB | ~2-3 GB | Especializado en visión (multi-imagen/video, OCR de documentos). No confirma soporte de tool-calling, úsalo para workflows centrados en análisis de imágenes/PDFs, no para AI Agents con herramientas |

> Regla general: la RAM/VRAM necesaria ronda 1.3-1.5× el tamaño de descarga del modelo
> (por el overhead del contexto). Cuantos más tokens de contexto uses, más memoria adicional
> se necesita.

### Cambiar el modelo por defecto

Edita `DEFAULT_OLLAMA_MODEL` en tu `.env` con cualquier tag de la
[librería de Ollama](https://ollama.com/search) y vuelve a levantar el stack:

```bash
# .env
DEFAULT_OLLAMA_MODEL=qwen3.5:2b   # o minicpm-v4.6:1b, qwen3.5:4b, etc. Vacío = no descargar nada

docker compose --profile cpu up
```

Recuerda que también debes seleccionar ese mismo modelo en el nodo **Ollama Chat Model**
dentro del workflow de n8n (por defecto viene configurado con `llama3.2:latest`).

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
