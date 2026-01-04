# RAG Sistema Agente de Asesinos Seriales

Sistema RAG (Retrieval-Augmented Generation) profesional y escalable para consultar información sobre asesinos seriales desde documentos PDF. Construido con LangChain, LangGraph, ChromaDB y Groq.

## 🎯 Características

- **Procesamiento Inteligente de PDFs**: Carga y extracción automática de texto de múltiples documentos PDF con detección automática de archivos nuevos
- **Búsqueda Semántica**: Sistema de embeddings multilengua gratuito con ChromaDB para recuperación precisa de información
- **Memoria Conversacional**: LangGraph para orquestar el flujo RAG y gestionar el estado de las conversaciones
- **Interfaz Gráfica Moderna**: UI elegante y académica integrada en Jupyter Notebook con diseño profesional
- **Integración Groq**: LLM rápido y eficiente (Llama 3.3 70B) para generación de respuestas de alta calidad
- **Persistencia de Datos**: Base de datos vectorial persistente que evita reprocesamiento innecesario

## 📋 Requisitos

- Python 3.8 o superior
- API Key de Groq (obtener en [groq.com](https://groq.com))
- Jupyter Notebook o JupyterLab (incluido en las dependencias)

## 🚀 Instalación

### 1. Clonar o descargar el repositorio

```bash
git clone <url-del-repositorio>
cd RAG_Agente_Serial_Killers
```

### 2. Instalar las dependencias

```bash
pip install -r requirements.txt
```

### 3. Configurar la API Key de Groq

Crear un archivo `.env` en la raíz del proyecto:

```bash
# Crear archivo .env
GROQ_API_KEY=tu-api-key-de-groq-aqui
```

**Nota**: También puedes configurar la API key como variable de entorno del sistema, o ingresarla directamente en el notebook.

## 📁 Estructura del Proyecto

```
RAG_Agente_Serial_Killers/
│
├── RAG_Agente_Serial_Killers.ipynb  # Notebook principal con todo el sistema
├── requirements.txt                  # Dependencias del proyecto
├── README.md                         # Este archivo
├── .env                              # Variables de entorno (crear manualmente)
├── pdfs/                             # Directorio para colocar los PDFs
│   └── *.pdf                         # Archivos PDF a procesar
└── chroma_db/                        # Base de datos vectorial (se crea automáticamente)
    ├── chroma.sqlite3                # Base de datos SQLite de ChromaDB
    └── [UUID]/                        # Índices vectoriales
```

## 💻 Uso

### Inicialización

1. **Abrir el notebook**: Abre `RAG_Agente_Serial_Killers.ipynb` en Jupyter Notebook o JupyterLab

2. **Colocar PDFs**: Coloca los archivos PDF en el directorio `pdfs/` (se crea automáticamente si no existe)

3. **Configurar API Key**: Asegúrate de tener configurada tu `GROQ_API_KEY` en el archivo `.env` o como variable de entorno

4. **Ejecutar celdas**: Ejecuta todas las celdas del notebook en orden:
   - Las primeras celdas configuran el sistema y cargan dependencias
   - La celda de "Procesamiento de PDFs" detecta y procesa automáticamente los PDFs nuevos
   - Las últimas celdas inicializan la interfaz gráfica

### Procesamiento de PDFs

El sistema incluye **detección automática de PDFs nuevos**:
- La primera vez procesa todos los PDFs en el directorio `pdfs/`
- En ejecuciones posteriores, detecta automáticamente PDFs nuevos y solo procesa esos
- La base de datos vectorial se actualiza incrementalmente sin perder datos previos
- Para agregar nuevos PDFs, simplemente colócalos en `pdfs/` y ejecuta la celda de procesamiento

### Consultas

Una vez inicializado el sistema:

1. **Usar la interfaz gráfica**: Se muestra automáticamente después de ejecutar todas las celdas
2. **Ingresar consultas**: Escribe tu pregunta en el campo de texto (ej: "¿Quién fue Ted Bundy?")
3. **Obtener respuestas**: El sistema busca información relevante y genera una respuesta académica
4. **Ver historial**: El historial de conversación se muestra debajo de las respuestas

## 🔧 Configuración Técnica

### Parámetros Principales

El sistema está configurado con los siguientes parámetros (editables en el notebook):

- **Chunk Size**: 1000 caracteres
- **Chunk Overlap**: 200 caracteres
- **Documentos Recuperados**: 5 documentos por consulta
- **Temperatura LLM**: 0.3 (para respuestas más precisas y deterministas)

### Modelos Utilizados

- **Embeddings**: `paraphrase-multilingual-MiniLM-L12-v2`
  - Modelo multilengua gratuito
  - Soporta español, inglés y otros idiomas
  - Se descarga automáticamente la primera vez (puede tardar varios minutos)

- **LLM**: `llama-3.3-70b-versatile` (Groq)
  - Modelo principal configurado
  - Otros modelos disponibles: `llama-3.1-8b-instant`, `mixtral-8x7b-32768`, `gemma-7b-it`
  - Configurable en el notebook cambiando `GROQ_MODEL`

## 🏗️ Componentes Técnicos

- **LangChain**: Framework RAG principal para orquestar el pipeline
- **LangGraph**: Gestión de estado y flujo del agente con grafo de estados
- **ChromaDB**: Base de datos vectorial para almacenar y buscar embeddings
- **Sentence-Transformers**: Modelos de embeddings multilengua gratuitos
- **Groq**: API de LLM para generación rápida de respuestas
- **ipywidgets**: Interfaz gráfica interactiva en Jupyter
- **PyPDF**: Procesamiento y extracción de texto de archivos PDF

## 📊 Flujo del Sistema

1. **Carga de PDFs**: Los PDFs se cargan y se dividen en chunks con overlap
2. **Generación de Embeddings**: Cada chunk se convierte en un vector usando el modelo de embeddings
3. **Almacenamiento**: Los embeddings se almacenan en ChromaDB con metadatos
4. **Consulta del Usuario**: El usuario ingresa una pregunta a través de la interfaz
5. **Recuperación**: Se buscan los documentos más relevantes usando búsqueda semántica
6. **Generación**: El LLM genera una respuesta basada en el contexto recuperado
7. **Visualización**: La respuesta se muestra en la interfaz con formato markdown

## ⚙️ Arquitectura LangGraph

El sistema utiliza LangGraph para orquestar el flujo RAG:

- **Estado del Grafo**: Mantiene mensajes, contexto y pregunta actual
- **Nodo de Recuperación**: Busca documentos relevantes en ChromaDB
- **Nodo de Generación**: Genera la respuesta usando Groq con el contexto recuperado
- **Flujo**: `retrieve → generate → END`

## 📝 Notas Importantes

- **Primera Ejecución**: La primera vez que ejecutes el sistema, el modelo de embeddings se descargará automáticamente (puede tardar varios minutos dependiendo de tu conexión)
- **Persistencia**: La base de datos vectorial se guarda en `chroma_db/` y persiste entre sesiones
- **Agregar PDFs**: Para agregar nuevos PDFs, simplemente colócalos en `pdfs/` y ejecuta la celda de procesamiento
- **Memoria**: El sistema mantiene un historial de conversación durante la sesión actual
- **API Key**: Asegúrate de tener una API key válida de Groq (hay límites de uso gratuitos)

## 📄 Licencia

Este proyecto es de uso educativo y de investigación.

## 🙏 Contribuciones

Las contribuciones son bienvenidas. Por favor, abre un issue o pull request si deseas mejorar el proyecto.
