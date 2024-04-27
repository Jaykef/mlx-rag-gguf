# MLX RAG With GGUF Model Weights
Minimal, clean code implementation of RAG with mlx using gguf model weights.

The code here builds on <a href="https://github.com/vegaluisjose/mlx-rag">https://github.com/vegaluisjose/mlx-rag</a>, it has been optimized to support RAG-based inferencing for .gguf models. I am using <a href="https://huggingface.co/BAAI/bge-small-en">BAAI/bge-small-en</a> for the embedding model, <a href="https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF/blob/main/tinyllama-1.1b-chat-v1.0.Q4_0.gguf">TinyLlama-1.1B-Chat-v1.0-GGUF</a> (you can choose from supported models below) as base model and the custom vector database script for indexing texts in a pdf file. Inference speeds can go up to ~413 tokens/sec for prompts and ~36 tokens/sec for generation on my 8G M2 Air.

## Update
- Added support for [phi-3-mini-4k-instruct.gguf](https://huggingface.co/Jaward/phi-3-mini-4k-instruct.Q4_0.gguf) and other `Q4_0`, `Q4_1` & `Q8_0` quantized models, download and save model in models/phi-3-mini-instruct folder

## Demo

https://github.com/Jaykef/mlx-rag-gguf/assets/11355002/e97907ed-1142-4f3e-b2fd-95690c4b50f3

## Usage
Download Models (you can use hf's snapshot_download but I recommend downloading separately to save time). Save in models folder.
> [!NOTE]
> MLX is able to read most quantization formats from GGUF directly. However,
> only a few quantizations are supported directly: `Q4_0`, `Q4_1`, and `Q8_0`.
> Unsupported quantizations will be cast to `float16`.

### Tested/Supported models

Tinyllama Q4_0 and Q8_0
- <a href="https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF/blob/main/tinyllama-1.1b-chat-v1.0.Q4_0.gguf">tinyllama-1.1b-chat-v1.0.Q4_0.gguf</a> 
- <a href="https://huggingface.co/TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF/blob/main/tinyllama-1.1b-chat-v1.0.Q8_0.gguf">tinyllama-1.1b-chat-v1.0.Q8_0.gguf</a>

Phi-3-mini Q4_0
- <a href="https://huggingface.co/Jaward/phi-3-mini-4k-instruct.Q4_0.gguf">phi-3-mini-4k-instruct.Q4_0.gguf</a>

Mistral Q4_0 and Q8_0
- <a href="https://huggingface.co/TheBloke/Mistral-7B-v0.1-GGUF/blob/main/mistral-7b-v0.1.Q4_0.gguf">mistral-7b-v0.1.Q4_0.gguf</a>
- <a href="https://huggingface.co/TheBloke/Mistral-7B-v0.1-GGUF/blob/main/mistral-7b-v0.1.Q8_0.gguf">mistral-7b-v0.1.Q8_0.gguf</a>

Embedding models
- <a href="https://huggingface.co/Jaward/mlx-bge-small-en">mlx-bge-small-en</a> converted mlx format of BAAI/bge-small-en, save it in the mlx-bge-small-en folder.
- <a href="https://huggingface.co/BAAI/bge-small-en/tree/main">bge-small-en</a> Only need the model.safetensors file, save it in the bge-small-en folder.


Install requirements
```
python3 -m pip install -r requirements.txt
```

Convert pdf into mlx compatible vector database
```
python3 create_vdb.py --pdf mlx_docs.pdf --vdb vdb.npz
```

Query the model
```
python3 rag_vdb.py \
    --question "Teach me the basics of mlx" \
    --vdb "vdb.npz" \
    --gguf "models/phi-3-mini-instruct/phi-3-mini-4k-instruct.Q4_0.gguf"
```

The files in the repo work as follow:

- <a href="https://github.com/Jaykef/mlx-rag-gguf/blob/main/gguf.py">gguf.py</a>: Has all stubs for loading and inferencing .gguf models.
- <a href="https://github.com/vegaluisjose/mlx-rag/blob/main/vdb.py">vdb.py</a>: Holds logic for creating a vector database from a pdf file and saving it in mlx format (.npz) .
- <a href="https://github.com/Jaykef/mlx-rag-gguf/blob/main/create_vdb.py">create_vdb.py</a>: It inherits from vdb.py and has all arguments used in creating a vector DB from a PDF file in mlx format (.npz).
- <a href="https://github.com/Jaykef/mlx-rag-gguf/blob/main/rag_vdb.py">rag_vdb.py</a>: Retrieves data from vdb used in querying the base model.
- <a href="https://github.com/Jaykef/mlx-rag-gguf/blob/main/model.py">model.py</a>: Houses logic for the base model (with configs), embedding model and transformer encoder.
- <a href="https://github.com/Jaykef/mlx-rag-gguf/blob/main/utils.py">utils.py</a>: Utility function for accessing GGUF tokens.

Queries make use of both .gguf (base model) and .npz (retrieval model) simultaneouly resulting in much higher inferencing speeds.

Checkout other cool mlx projects here: https://github.com/ml-explore/mlx/discussions/654#discussioncomment

## License
MIT
