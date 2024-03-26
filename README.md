# MLX RAG GGUF
Minimal, clean code implementation of RAG with mlx inferencing for GGUF models.

The code here builds on <a href="https://github.com/vegaluisjose/mlx-rag">https://github.com/vegaluisjose/mlx-rag</a>, it has been optimized to support RAG-based inferencing for .gguf models. I am using <a href="https://huggingface.co/BAAI/bge-small-en">BAAI/bge-small-en</a> for the embedding model, <a href="https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF/blob/main/tinyllama-1.1b-chat-v1.0.Q4_0.gguf">TinyLlama-1.1B-Chat-v1.0-GGUF</a> as base model.

## Usage
Download Models
- <a href="https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF/blob/main/tinyllama-1.1b-chat-v1.0.Q4_0.gguf">tinyllama-1.1b-chat-v1.0.Q4_0.gguf</a> put it in the tinyllama folder.
- <a href="https://huggingface.co/Jaward/mlx-bge-small-en">mlx-bge-small-en</a> converted mlx format of BAAI/bge-small-en, put it in the mlx-bge-small-en folder.
- <a href="https://huggingface.co/Jaward/mlx-bge-small-en">bge-small-en</a> BERT format of BAAI/bge-small-en, put it in the bge-small-en folder.

Convert pdf into mlx compatible vector database
```
python3 create_vdb.py --pdf mlx_docs.pdf --vdb vdb.npz
```

Query the model
```
python3 create_vdb.py --pdf mlx_docs.pdf --vdb vdb.npz
```
