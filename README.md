# Cocktail-RAG

En este proyecto usamos la base "Cocktail Recipes" ([ver enlace](Documento_Explicativo_CocktailRAG.pdf)) 
para implementar la metodología de **Retrieval Augmented Generation (RAG)**.  
El sistema toma ingredientes, recetas y datos de los cócteles de la base y propone cocteles para preparar según el input del usuario.  

Usamos **Sentence Transformers** como encoder y **Mistral 7b** como decoder.


# 🍸 Cocktail RAG - Sistema de Recomendación de Cocteles

Un sistema de Retrieval Augmented Generation (RAG) que recomienda cocteles personalizados basándose en ingredientes, recetas y preferencias del usuario.

## 📋 Descripción

Este proyecto implementa un sistema RAG que utiliza la base de datos "Cocktail Recipes" para proporcionar recomendaciones inteligentes de cocteles. El sistema combina:

- **Encoder**: Sentence Transformers (`all-MiniLM-L6-v2`) para generar embeddings semánticos de las recetas
- **Vector Store**: FAISS para búsqueda eficiente de similitud
- **Decoder**: Mistral 7B Instruct para generar respuestas naturales y contextuales

## 🎯 Características

- Búsqueda semántica de cocteles basada en descripciones en lenguaje natural
- Recomendaciones personalizadas según ingredientes disponibles
- Respuestas conversacionales generadas por LLM
- Base de datos de 875+ recetas de cocteles
- Embeddings de 384 dimensiones para captura precisa del contexto

## 🗃️ Dataset

El proyecto utiliza el dataset [Cocktail Recipes](https://huggingface.co/datasets/brianarbuckle/cocktail_recipes) de Hugging Face.

### Estructura del Dataset (ejemplo de un dato)

```json
{
  "title": "Final Ward",
  "ingredients": [
    "0.75 oz. Rye Whiskey",
    "0.75 oz. Lemon Juice",
    "0.75 oz. Maraschino Liqueur",
    "0.75 oz. Green Chartreuse"
  ],
  "directions": ["shake on ice and strain"],
  "misc": [],
  "source": "Death & Co.",
  "ner": ["whiskey", "chartreuse", "maraschino liqueur"]
}
```

### Campos del Dataset

- **title** (`str`): Nombre del coctel
- **ingredients** (`list` of `str`): Lista de ingredientes con cantidades
- **directions** (`list` of `str`): Pasos de preparación
- **source** (`str`): Origen de la receta
- **ner** (`list` of `str`): Ingredientes clave identificados mediante NER

## 🚀 Instalación

```bash
# Subir a Collab
Se debe descargar el archivo y correr en google Collab. Se recomienda hacer uso de una GPU, aunque no es necesario.
```

## 💻 Uso

### Ejecución del Notebook

Abre `Cocktail_RAG_Notebook.ipynb` en Google Colab y ejecuta todas las celdas.
Al final del notebook verás una casilla de input titulada `Haz una pregunta` donde puedes ingresar inputs.

### Ejemplo de Interacción

```python
# Inicializar el sistema
context = encoder("sweet cocktail with rum and pineapple")
response = decoder(context, "sweet cocktail with rum and pineapple")
print(response)
```

### Consultas de Ejemplo
Nota que el decoder usado permite interacciones en español e ingles.
```
"Dime un coctel con Mezcal con sabor ahumado"
"sweet cocktail with rum and pineapple"
"refreshing drink with gin and citrus"
"strong whiskey-based cocktail"
```

## 🏗️ Arquitectura

### 1. Procesamiento de Datos
- Normalización de listas a texto
- Unificación de campos en documento único
- Embeddings generados con Sentence Transformers

### 2. Retrieval (R)
- **Modelo**: `sentence-transformers/all-MiniLM-L6-v2`
- **Dimensiones**: 384
- **Vector Store**: FAISS
- **Búsqueda**: Top-k similarity search (k=3)

### 3. Augmentation (A)
- Construcción de contexto con top-k documentos recuperados
- Formato estructurado para el LLM

### 4. Generation (G)
- **Modelo**: Mistral 7B Instruct v0.2
- **Cuantización**: 4-bit con BitsAndBytes
- **Generación**: Sampling con temperature=0.7

## 📊 Especificaciones Técnicas

| Componente | Especificación |
|------------|----------------|
| Encoder Model | all-MiniLM-L6-v2 |
| Embedding Dim | 384 |
| Max Tokens (Encoder) | 256 |
| Decoder Model | Mistral-7B-Instruct-v0.2 |
| Quantization | 4-bit |
| Vector Store | FAISS |
| Dataset Size | 875 recetas |

## 📁 Estructura del Proyecto

```
cocktail-rag/
│
├── Cocktail_RAG_Notebook.ipynb             # Notebook principal
├── Documento_Explicativo_CocktailRAG.doc   # Documento que explica el modelo
├── README.md                               # Este archivo
└── requirements.txt                        # Dependencias del proyecto

```
* No se requiere la descarga de datos.

## 🔧 Requisitos

- Python 3.8+
- GPU recomendada (para Mistral 7B)
- 15GB+ RAM
- Google Colab

## 📝 Notas Importantes

- Los documentos son cortos, por lo que no se requiere chunking
- El modelo utiliza mean pooling para generar embeddings de documentos completos
- La cuantización 4-bit permite ejecutar Mistral 7B en GPUs consumer
- El notebook usado no tiene un preview, pues al usar un input de chat, GitHub no permite ver previews.

## 📄 Licencia

Este proyecto utiliza:
- Dataset: [Cocktail Recipes](https://huggingface.co/datasets/brianarbuckle/cocktail_recipes)
- Encoder: Apache 2.0 License
- Decoder: Apache 2.0 License

## 🙏 Agradecimientos

- Dataset por [brianarbuckle](https://huggingface.co/brianarbuckle)
- Sentence Transformers por [UKPLab](https://www.sbert.net/)
- Mistral AI por el modelo Mistral 7B

## 👤 Creado por:

Liz Lara, Samuel Suárez, Julian Fandiño, Cesar Augusto Espinoza

---

**Nota**: Este proyecto es con fines educativos y de demostración de técnicas RAG.
