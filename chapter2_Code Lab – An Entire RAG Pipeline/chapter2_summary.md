# 🧪 Chapter 2 – Code Lab: An Entire RAG Pipeline

In Chapter 2, we take the first practical step toward building a fully functional **Retrieval-Augmented Generation (RAG)** system using LangChain. The chapter is focused on constructing a **minimal but working RAG pipeline** and explaining its key components.

---

## 🎯 Objective

To create a simple, working RAG pipeline that:
- Loads documents from the web.
- Splits them into manageable chunks.
- Converts those chunks into vector embeddings.
- Stores and retrieves those embeddings from a vector database (Chroma).
- Uses a language model (LLM) to generate answers based on the retrieved context.

---

## 🧱 Tech Stack Used

| Component           | Tool/Library                      |
|---------------------|-----------------------------------|
| Document Loader     | `langchain.document_loaders.WebBaseLoader` |
| Text Splitter       | `RecursiveCharacterTextSplitter` |
| Embedding Model     | `OpenAIEmbeddings`               |
| Vector Store        | `Chroma`                         |
| Retriever           | `vectorstore.as_retriever()`     |
| Prompt Template     | `langchain.prompts.PromptTemplate` |
| LLM (Generator)     | `ChatOpenAI`                     |
| Orchestration Tool  | `LangChain Expression Language` (LCEL) |

---

## 🔄 Step-by-Step Pipeline Overview

### 1. **Load a Web Page**

The pipeline begins by using `WebBaseLoader` to load the content of a web page (like a blog, Wikipedia article, or documentation page). This content becomes the source corpus for our question-answering system.

### 2. **Split Text into Chunks**

Since LLMs have a context window limit, the full document cannot be passed as is. Therefore, the loaded text is split using `RecursiveCharacterTextSplitter` to create overlapping chunks that are semantically consistent and manageable in size.

### 3. **Generate Embeddings**

Each chunk is transformed into a high-dimensional vector using the `OpenAIEmbeddings` model. These embeddings capture the semantic meaning of the text, enabling similarity-based retrieval.

### 4. **Store in Chroma Vector DB**

The vector representations are stored using `Chroma`, a lightweight, open-source vector database. This allows us to later perform similarity searches against this database to retrieve relevant chunks based on a user query.

### 5. **Retrieve Top-k Chunks**

A retriever is created from the vectorstore (`vectorstore.as_retriever()`), which fetches the most semantically similar chunks given a query.

### 6. **Prompt Engineering**

A well-crafted prompt is used to instruct the LLM to answer using only the retrieved context. This prompt is designed to discourage hallucination and ensure grounded, factual answers.

### 7. **Combine into a RetrievalQA Chain**

The system is wired using `RetrievalQA` from LangChain, which connects:
- The retriever
- The LLM
- The prompt

All working in unison to generate grounded, explainable answers.

---

## 📌 Code Highlights

```python
from langchain.document_loaders import WebBaseLoader
loader = WebBaseLoader("https://example.com")
docs = loader.load()
