# Web Content Summarizer

This project fetches web content from a given URL, embeds the content using Hugging Face's `sentence-transformers/all-mpnet-base-v2` model, and stores the embeddings in a FAISS vector database. Users can interact with the system to summarize the content based on their prompts.

## Features

- Fetches web content using `UnstructuredURLLoader`.
- Embeds content using the `sentence-transformers/all-mpnet-base-v2` model.
- Stores embeddings in a FAISS vector database for efficient similarity search.
- Summarizes content based on user-provided prompts.

---

## Installation

### Prerequisites

- Python 3.8 or above
- pip package manager

### Dependencies

Install the required dependencies:

```bash
pip install sentence-transformers faiss-cpu unstructured
```

---

## Usage

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/web-content-summarizer.git
cd web-content-summarizer
```

### 2. Prepare the Environment

Ensure the necessary libraries are installed:

```bash
pip install -r requirements.txt
```

### 3. Fetch and Process Web Content

Provide the URL of the content you want to summarize. Example usage:

```python
from unstructured.ingest.loader import UnstructuredURLLoader
from sentence_transformers import SentenceTransformer
import faiss

# Load content from URL
url_loader = UnstructuredURLLoader(urls=["https://example.com"])
content = url_loader.load()

# Embed content
model = SentenceTransformer("sentence-transformers/all-mpnet-base-v2")
embeddings = model.encode(content)

# Store in FAISS vector store
dimension = embeddings.shape[1]
faiss_index = faiss.IndexFlatL2(dimension)
faiss_index.add(embeddings)
```

### 4. Query and Summarize

Use user prompts to query the FAISS vector store and summarize the content:

```python
def summarize(prompt, faiss_index, model, content):
    # Encode the prompt
    query_embedding = model.encode([prompt])
    
    # Search in FAISS index
    distances, indices = faiss_index.search(query_embedding, k=5)
    
    # Retrieve relevant content
    relevant_content = [content[idx] for idx in indices[0]]
    summary = " ".join(relevant_content)
    
    return summary

# Example prompt
prompt = "Summarize the main points of the article"
summary = summarize(prompt, faiss_index, model, content)
print("Summary:", summary)
```

---

## Project Structure

```
web-content-summarizer/
├── README.md             # Project documentation
├── requirements.txt      # List of dependencies
├── main.py               # Main script for summarization
├── utils/                # Utility functions
└── tests/                # Unit tests
```

---

## Acknowledgments

- [Hugging Face](https://huggingface.co) for the `sentence-transformers` model.
- [FAISS](https://github.com/facebookresearch/faiss) for the vector database.
- [Unstructured](https://github.com/Unstructured-IO/unstructured) for the content loader.

---

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

---

## Contributing

Contributions are welcome! Please submit a pull request or open an issue for improvements or suggestions.

---

## Contact

For inquiries or support, reach out to `your_email@example.com`.
