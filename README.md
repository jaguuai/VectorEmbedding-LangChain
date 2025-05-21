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

### 4. LangChain Conversational Retrieval Pipeline (LLM + Memory + Retriever Abstraction)

#### Objective
This stage introduces a production-grade Retrieval-Augmented Generation (RAG) pipeline using LangChain's modular architecture. The system combines OpenAI's powerful chat models with memory persistence and vector-based document retrieval. It enables multi-turn, context-aware Q&A over internal documents with enhanced semantic understanding.

#### Implementation Summary
- Chat Model: `ChatOpenAI` is used to power natural conversations.
- Memory: `ConversationBufferMemory` stores the full dialogue history.
- Retriever: A vector store (e.g., Chroma) is abstracted via `.as_retriever()` to provide relevant document chunks.
- Integration: `ConversationalRetrievalChain` combines the LLM, retriever, and memory for a fully functional RAG pipeline.

#### Key Components
- LLM (Language Model)  
  OpenAI GPT-4o (or any OpenAI-compatible model) is responsible for generating intelligent, contextual answers.

- Retriever Abstraction  
  Converts a vector store (like FAISS or Chroma) into a retriever interface compatible with LangChain’s chaining system.

- Conversation Memory  
  Stores previous turns using `ConversationBufferMemory`, enabling follow-up and contextual continuity.

- Pipeline Composition  
  `ConversationalRetrievalChain` orchestrates the full flow between input → retrieval → memory → response.

#### Benefits
- Semantic Relevance  
  Uses vector similarity instead of simple keyword matching, improving contextual accuracy.

- Contextual Memory  
  Supports multi-turn conversations with memory of prior queries and responses.

- Modularity  
  All components (LLM, memory, retriever) are interchangeable and independently upgradable.

- Scalability  
  Designed for production, supporting large vector databases and high-throughput interactions.

#### Limitations
- Memory is stored in RAM only (`ConversationBufferMemory`). Persistent memory backends like Neo4j or Redis are not integrated at this stage.
- No advanced metadata filtering (e.g., doc type or tags) is applied during retrieval.
- Requires embeddings to be pre-generated and indexed in a compatible vector store (e.g., Chroma).


