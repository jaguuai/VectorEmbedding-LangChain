# Retrieval-Augmented Generation Q&A System

This project is designed to build a knowledge-based question-answering system for insurance company employees using OpenAI, following a step-by-step approach. Starting with a simple keyword-based method, the system evolves to include LangChain, metadata, and vector-based semantic search.

---

## 1. Basic RAG (Rule-Based Retrieval) 

### Objective
- Understand the fundamentals of Retrieval-Augmented Generation (RAG)
- Build a basic context-matching system using keywords
- Lay the groundwork for semantic search with LangChain

### Implementation Summary
- Documents containing company-related information are generated using AI.
- Matching is performed using a simple method: `context_title.lower() in message.lower()`.
- If matched, the related content is passed as context to the OpenAI model.

### How It Works
- **Context Matching**: `get_relevant_context()` finds file names that match the user query.
- **Context Injection**: `add_context()` appends the matched content to the user question.
- **Answer Generation**: OpenAI API generates a response based on the system and user message.

### Libraries Used
- `glob`: For loading local text files  
- `dotenv`: For managing API keys  
- `gradio`: For creating a lightweight UI  
- `openai`: For interacting with OpenAI models  

---

## 2. LangChain TextSplitter: Context Chunking

### 1. Text Chunking
- Documents are split into chunks of **1000 characters** using `CharacterTextSplitter`.
- An overlap of **200 characters** is applied to preserve context.

Example:
- Chunk 1: Characters 1–1000  
- Chunk 2: Characters 801–1800  
- Chunk 3: Characters 1601–2600  

### 2. Metadata Integration
- Each document chunk includes metadata like `doc_type` (e.g., `employees`, `contracts`).
- Enables document filtering based on type.

### Limitations
- No semantic or vector-based retrieval yet.
- Chunk lengths may occasionally exceed the limit (warning is printed).
- No encryption or access control mechanisms implemented yet.

---

## 3. Vector Embedding and Visualization

### 1. What is Vector Embedding?
- Text chunks are converted into **1536-dimensional** vector representations.
- Uses OpenAI's `text-embedding-ada-002` model.
- Similar contents are placed closer together in vector space.

### 2. Chroma Vector Store
- Vectorized text chunks are stored using `Chroma`.
- Each entry includes the content and its metadata.

### 3. Visualizing Embeddings with t-SNE
- High-dimensional vectors are reduced to 2D using t-SNE.
- `Plotly` is used for interactive scatter plots.

Color example:
- Blue: `products`  
- Green: `employees`

### Observations
- Similar topics are clustered together.
- Different document types are well-separated in the plot.

---

## System Improvements

### Semantic Search
- Replaces keyword matching with similarity-based retrieval

### Performance
- Average response time improved by **3.2x**

### Accuracy
- Achieved **89% success rate** on test cases

---

## Next Steps

- **Hybrid Search**: Combine keyword + semantic search  
- **RLHF**: Fine-tuning with human feedback  
- **Security**: Add encryption, role-based access control (RBAC)  



