# Food_&_Nutrition_RAG_Chatbot

A document-based question-answering chatbot built with Python, ChromaDB, PyPDF, and the Groq API. The project uses Retrieval-Augmented Generation (RAG) to retrieve relevant information from a Healthy Eating Guide PDF and generate context-based responses.

## About the Project

Healthy Food Assistant Bot is a Python-based chatbot that allows users to ask questions about healthy eating and nutrition using information from an uploaded PDF document.

The project implements a basic Retrieval-Augmented Generation (RAG) pipeline. Instead of sending questions directly to a language model, the system first searches the document collection for relevant text passages. These passages are then provided to the language model as context to generate an answer.

The chatbot is designed to keep its responses within the information available in the retrieved document passages. When the retrieved information does not provide an answer, it is instructed to respond with:

`I don't know about this information.`

It also maintains conversation history during the active session, allowing users to ask follow-up questions.

## Features

- **PDF and TXT Support:** Loads PDF and TXT documents from a designated folder.
- **Page-Based Text Extraction:** Extracts PDF text while preserving the corresponding source filename and page number.
- **Text Cleaning:** Removes unnecessary whitespace from extracted document content.
- **Sentence-Based Chunking:** Divides document text into smaller sections while keeping sentences together where possible.
- **Embedding Generation:** Uses ChromaDB's `DefaultEmbeddingFunction` to generate numerical representations of text.
- **Vector Database:** Stores document chunks, embeddings, and associated metadata in ChromaDB.
- **Semantic Retrieval:** Searches for the most relevant document passages based on the user's question.
- **LLM Integration:** Uses the Groq API with the `openai/gpt-oss-120b` model to generate responses.
- **Context-Based Responses:** Provides retrieved document passages to the language model as reference material.
- **Source Tracking:** Displays the source filename and page numbers associated with retrieved passages.
- **Conversation History:** Maintains recent conversation messages during the active session.
- **Interactive Terminal Chat:** Allows users to ask multiple questions until they enter `exit`.

## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| PyPDF | Extracting text from PDF documents |
| Regular Expressions | Sentence splitting and text processing |
| ChromaDB | Vector storage and similarity search |
| DefaultEmbeddingFunction | Generating text embeddings |
| Groq API | Connecting to the language model |
| GPT-OSS-120B | Generating chatbot responses |
| Google Colab | Development and execution environment |

## How It Works

The chatbot follows a Retrieval-Augmented Generation (RAG) workflow consisting of document processing, information retrieval, and response generation.

1. **Document Loading:** The system reads PDF and TXT files from the `docs` folder.
2. **Text Extraction:** PyPDF extracts text from individual PDF pages and records their source information.
3. **Text Cleaning:** Unnecessary whitespace is removed to prepare the extracted content for processing.
4. **Text Chunking:** The document content is divided into smaller chunks using paragraph-based and sentence-based splitting.
5. **Embedding Generation:** ChromaDB's default embedding function converts the chunks into numerical embeddings.
6. **Vector Storage:** The document chunks, embeddings, and metadata are stored in a ChromaDB collection named `foods`.
7. **Information Retrieval:** When a user submits a question, ChromaDB performs a similarity search and retrieves the two most relevant chunks.
8. **Context Preparation:** The retrieved passages are combined with their source filenames and page numbers.
9. **Response Generation:** The Groq API sends the question, retrieved context, system instructions, and recent conversation history to the GPT-OSS-120B model.
10. **Answer Display:** The generated response is displayed along with the references to the retrieved document passages.

## Project Structure

The repository is organized as follows:

```text
Healthy-Food-Assistant-Bot/
│
├── Healthy_Food_Assistant_Bot.ipynb
├── README.md
└── docs/
    └── Healthy_Eating_Guide.pdf
```

| File/Folder | Description |
|---|---|
| `Healthy_Food_Assistant_Bot.ipynb` | Notebook containing the complete chatbot implementation. |
| `README.md` | Documentation describing the project and its setup. |
| `docs/` | Directory containing the PDF and TXT knowledge-source documents. |
| `Healthy_Eating_Guide.pdf` | Example PDF document used as the chatbot's knowledge source. |

## Installation and Setup

The project was developed and executed in Google Colab.

### 1. Clone the Repository

Run the following command to clone the repository:

```bash
git clone https://github.com/YOUR-USERNAME/Healthy-Food-Assistant-Bot.git
```

Navigate to the project directory:

```bash
cd Healthy-Food-Assistant-Bot
```

### 2. Install Dependencies

Install the required Python libraries:

```python
!pip install -q groq chromadb pypdf
```

### 3. Configure the Groq API Key

A Groq API key is required to access the language model.

1. Visit [Groq Console](https://console.groq.com/) and obtain an API key.
2. Open Google Colab and navigate to Secrets.
3. Create a secret named `FoodAPIKey`.
4. Add your Groq API key as its value.
5. Enable notebook access for the secret.

The notebook accesses the key using:

```python
from google.colab import userdata

api_key = userdata.get("FoodAPIKey")
```

The API key is used to initialize the Groq client:

```python
from groq import Groq

client = Groq(api_key=api_key)
MODEL = "openai/gpt-oss-120b"
```

**Security Note:** Do not include your actual API key in the notebook or upload it to GitHub.

### 4. Add the Knowledge Source

Place the required PDF or TXT documents inside the `docs` directory.

The document-loading functions automatically identify supported `.pdf` and `.txt` files and extract their text for further processing.

### 5. Run the Notebook

1. Open `Healthy_Food_Assistant_Bot.ipynb` in Google Colab.
2. Ensure the knowledge-source documents are available in the correct folder.
3. Configure the Groq API key through Colab Secrets.
4. Install the required dependencies.
5. Execute the notebook cells in order.
6. Start asking questions once the chatbot is initialized.

## Example Questions

The following questions can be used to test the chatbot:

- What are the benefits of eating fruits and vegetables?
- What does a healthy diet mean?
- How can people stay within their daily calorie needs?
- What information does the guide provide about healthy eating?
- What are some ways to make healthier food choices?

The chatbot searches the uploaded document for relevant information before generating a response.

Questions that cannot be answered using the retrieved context should receive the predefined fallback response.

## Limitations

- The chatbot relies on the information contained in the uploaded PDF and TXT documents.
- The quality of responses depends on the extracted document text and retrieval results.
- Only two document chunks are retrieved by default for each question.
- Relevant information may not always be retrieved, even when it exists in the document.
- Scanned PDFs without an extractable text layer may not provide usable content.
- The chatbot's responses are generated by a language model and may not always perfectly reflect the retrieved passages.
- Conversation history is maintained only during the active session.
- A valid Groq API key and internet connection are required for response generation.
- The current implementation provides a terminal-based interaction rather than a separate graphical user interface.

## Future Improvements

The project can be extended in several ways:

- **Multiple Document Support:** Expand the knowledge base by adding more food and nutrition-related documents.
- **Improved Retrieval:** Experiment with different chunk sizes, retrieval methods, and similarity thresholds.
- **Graphical User Interface:** Develop a simple interface to make the chatbot easier to use.
- **Persistent Conversation History:** Store conversations so they can be accessed across different sessions.
- **Document Upload:** Allow users to add PDF and TXT documents without manually placing them in the folder.
- **Improved Source References:** Display retrieved passages alongside their document references for easier verification.
- **Retrieval Evaluation:** Test retrieval accuracy using a collection of questions with known answers.

## Knowledge Source

*Healthy Eating Guide* – PDF document used as the chatbot's reference material.

The document serves as the knowledge source for retrieving relevant passages and generating responses to food and healthy-eating-related questions.

---

**Project:** Healthy Food Assistant Bot  
**Language:** Python  
**Approach:** Retrieval-Augmented Generation (RAG)  
**Core Technologies:** ChromaDB, PyPDF, Groq API and GPT-OSS-120B  
**Development Environment:** Google Colab
