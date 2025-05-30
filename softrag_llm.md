Minimal **local-first** Retrieval-Augmented Generation (RAG) library powered by **SQLite + sqlite-vec**.  
Everything—documents, embeddings, cache—lives in a single `.db` file.

---

## 🌟 Features

- **Local-first** – All processing happens locally, no external services.
- **SQLite + sqlite-vec** – Documents, embeddings, and cache in a single `.db` file (no separate vector store or account needed).
- **No cloud service dependency** – Plug in any LLM backend; no forced API keys for the core storage layer.
- **Blazing-fast** – Designed for minimal overhead and maximum throughput on small- and medium-scale corpora. <!-- enfatizar performance -->
- **Perfect for small & medium use cases** – Ideal when you need a lightweight, self-contained RAG solution. <!-- destacar o público-alvo -->
- **Configurable chunking** – Default `RecursiveCharacterTextSplitter` (400/100) or your own strategy.
- **Model-agnostic** – Works with OpenAI, Hugging Face, Ollama, etc.
- **Zero heavy deps** – Core pulls only minimal extras (`langchain-text-splitters` optional).

## 📋 Requirements

- Python 3.12+
- Dependencies: sqlite-vec, trafilatura, pymupdf (for PDFs)
- Access to embedding models and LLMs (uses OpenAI by default)

## 🚀 Installation

```bash
pip install softrag
```

## 🔧 Basic Usage

```python
from softrag import Rag
from langchain_openai import ChatOpenAI, OpenAIEmbeddings

chat  = ChatOpenAI(model="gpt-4o")
embed = OpenAIEmbeddings(model="text-embedding-3-small")

rag = Rag(embed_model=embed, chat_model=chat)  # Uses default chunk splitter (RCTS)

# Add documents to your knowledge base
rag.add_file("document.pdf")
rag.add_web("https://example.com/page")

# Query your knowledge base with context-augmented answers
answer = rag.query("What is the main information in this content?")
print(answer)
```

also

`_set_splitter(splitter=None)`: Configure the text chunking strategy.
`_retrieve(query, k)`: Retrieve the most relevant text chunks for a given query.
`_persist(text, metadata)`: Persist raw text into the database with optional metadata.

## 📚 Examples

See the `examples/` folder for more detailed examples:

- `simple.py`: Basic example with OpenAI
- `local.py`: Example using local Transformers models

## 🔄 How It Works

SoftRAG uses a hybrid approach for retrieval:

1. **Extraction**: Content is extracted from documents and web pages
2. **Splitting**: Text is divided into smaller chunks
3. **Indexing**: Each chunk is indexed by text (SQLite FTS5) and vector embedding
4. **Retrieval**: Queries combine keyword search and vector similarity
5. **Generation**: The most relevant chunks are sent to the LLM along with the question

## 🤝 Contributing

Contributions are welcome! Please feel free to submit Pull Requests.

## 📜 License

This project is licensed under the MIT License - see the LICENSE file for details.

Developed with ❤️ for AI community
