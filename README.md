# RAG Sistema Agente de Asesinos Seriales

Sistema RAG (Retrieval-Augmented Generation) profesional y escalable para consultar información sobre asesinos seriales desde documentos PDF. Construido con LangChain, LangGraph, ChromaDB y Groq.

## Características

- **Procesamiento de PDFs**: Carga y extracción automática de texto de múltiples documentos PDF
- **Búsqueda Semántica**: Sistema de embeddings multilengua gratuito con ChromaDB
- **Memoria Conversacional**: LangGraph para mantener contexto y reutilizar información del prompt
- **Interfaz Gráfica**: UI moderna y académica integrada en Jupyter Notebook
- **Integración Groq**: LLM rápido y eficiente para generación de respuestas

## Requisitos

- Python 3.8 o superior
- API Key de Groq (obtener en [groq.com](https://groq.com))

## Instalación

### Opción 1: Instalación Local

1. Clonar o descargar el repositorio
2. Instalar las dependencias:

```bash
pip install -r requirements.txt
```

3. Configurar la API Key de Groq en el notebook (se puede ingresar directamente o usar variables de entorno)

### Opción 2: Instalación con Docker (Recomendado)

1. **Crear archivo `.env`** con tu API Key de Groq:

```bash
# Crear archivo .env en la raíz del proyecto
GROQ_API_KEY=tu-api-key-de-groq-aqui
JUPYTER_TOKEN=  # Opcional: dejar vacío para acceso sin token
```

2. **Construir y ejecutar con Docker Compose**:

```bash
# Construir la imagen
docker-compose build

# Iniciar el contenedor
docker-compose up -d

# Ver los logs
docker-compose logs -f
```

3. **Acceder a Jupyter Lab**:
   - Abre tu navegador en: `http://localhost:8888`
   - Si configuraste un token, úsalo para acceder

4. **Detener el contenedor**:

```bash
docker-compose down
```

**Nota**: Los directorios `pdfs/` y `chroma_db/` se montan como volúmenes, por lo que los datos persisten entre reinicios del contenedor.

## Estructura del Proyecto

```
RAG_Agente_Serial_Killers/
│
├── RAG_Agente_Serial_Killers.ipynb  # Notebook principal con todo el sistema
├── requirements.txt                  # Dependencias del proyecto
├── README.md                         # Este archivo
├── Dockerfile                        # Configuración de Docker
├── docker-compose.yml                # Configuración de Docker Compose
├── .dockerignore                     # Archivos a ignorar en Docker
├── .env                              # Variables de entorno (crear manualmente)
├── pdfs/                             # Directorio para colocar los PDFs (crear manualmente)
└── chroma_db/                        # Base de datos vectorial (se crea automáticamente)
```

## Uso

1. Abrir el notebook `RAG_Agente_Serial_Killers.ipynb` en Jupyter
2. Colocar los PDFs en un directorio (por defecto: `pdfs/`)
3. Ejecutar todas las celdas del notebook en orden
4. La primera vez, el sistema procesará los PDFs y creará la base de datos vectorial
5. Usar la interfaz gráfica para hacer consultas sobre los documentos

## Componentes Técnicos

- **LangChain**: Framework RAG principal
- **LangGraph**: Gestión de estado y flujo del agente
- **ChromaDB**: Base de datos vectorial para almacenar embeddings
- **Sentence-Transformers**: Modelos de embeddings multilengua gratuitos
- **Groq**: LLM para generación de respuestas
- **ipywidgets**: Interfaz gráfica interactiva

## Modelos Utilizados

- **Embeddings**: `paraphrase-multilingual-MiniLM-L12-v2` (multilengua, gratuito)
- **LLM**: Groq (configurable en el notebook)

## Notas

- Los modelos de embeddings se descargan automáticamente la primera vez (puede tardar algunos minutos)
- La base de datos vectorial se persiste en disco para evitar reprocesamiento
- El sistema mantiene memoria conversacional entre consultas

## Licencia

Este proyecto es de uso educativo y de investigación.
