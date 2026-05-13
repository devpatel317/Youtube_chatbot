# YouTube Chatbot

YouTube Chatbot is a notebook-based retrieval-augmented generation (RAG) project that answers questions about a YouTube video from its transcript. It fetches the transcript for a selected video, splits it into chunks, creates vector embeddings, stores them in FAISS, and uses a Hugging Face language model to generate answers grounded in the transcript context.

## What It Does

- Extracts captions or transcripts from a YouTube video.
- Splits transcript text into overlapping chunks for retrieval.
- Builds a FAISS vector index from transcript embeddings.
- Retrieves the most relevant transcript passages for a user question.
- Generates grounded answers with a Hugging Face chat model.
- Demonstrates both a step-by-step workflow and a LangChain runnable chain.

## Architecture

The notebook follows a simple RAG pipeline:

1. Transcript ingestion with `youtube-transcript-api`.
2. Text chunking with LangChain text splitters.
3. Embedding generation with `HuggingFaceEmbeddings`.
4. Vector storage and similarity search with FAISS.
5. Prompt construction that instructs the model to answer only from the transcript.
6. Response generation with `HuggingFaceEndpoint` and `ChatHuggingFace`.

## Tech Stack

- Python
- Jupyter Notebook
- LangChain
- FAISS
- Hugging Face Inference API
- `youtube-transcript-api`

## Requirements

Make sure you have:

- Python 3.10+ recommended
- A working virtual environment
- Access to a Hugging Face API token
- An internet connection for transcript fetches and model inference

## Installation

Create and activate a virtual environment, then install the dependencies used in the notebook:

```bash
pip install youtube-transcript-api langchain langchain-core langchain-huggingface transformers huggingface-hub langchain-community faiss-cpu tiktoken python-dotenv requests==2.32.5
```

If you are running the notebook locally, open `Youtube_chatbot.ipynb` in Jupyter, VS Code, or another notebook environment.

## Configuration

Set your Hugging Face token before running the notebook:

```bash
set HUGGINGFACEHUB_API_TOKEN=your_huggingface_token
```

On PowerShell, use:

```powershell
$env:HUGGINGFACEHUB_API_TOKEN = "your_huggingface_token"
```

The notebook reads the token with `os.getenv("HUGGINGFACEHUB_API_TOKEN")`.

## Usage

1. Open `Youtube_chatbot.ipynb`.
2. Run the dependency installation cell.
3. Set the `video_id` variable to the YouTube video you want to query.
4. Execute the notebook cells in order:
   - transcript fetch
   - text splitting
   - embedding and FAISS index creation
   - retrieval test
   - model prompt and response generation
   - chain construction
5. Ask questions about the selected video using the retrieval chain.

The notebook currently includes a sample video ID and example questions such as summarization, topic checks, and speaker/entity lookup.

## Notebook Workflow

### 1. Transcript Ingestion

The notebook fetches the YouTube transcript for the selected video ID and joins the transcript chunks into a single text string.

### 2. Chunking

Transcript text is split into overlapping chunks so retrieval stays focused on the most relevant passages.

### 3. Embedding and Indexing

Chunks are embedded with `sentence-transformers/all-MiniLM-L6-v2` and indexed in FAISS.

### 4. Retrieval

The retriever returns the top matching transcript chunks for a question.

### 5. Generation

The prompt tells the model to answer only from the transcript context and to say it does not know when the context is insufficient.

### 6. Chain Composition

The notebook also demonstrates a LangChain runnable pipeline that combines retrieval, prompt formatting, generation, and output parsing.

## Project Structure

```text
Youtube_chatbot.ipynb
README.md
```

## Notes

- This project is notebook-driven rather than packaged as a web app or CLI.
- The sample notebook uses a specific YouTube video ID, but you can replace it with any transcript-enabled video.
- If a video has no available transcript, the notebook will print a fallback message.

## Troubleshooting

- If you see `No module named 'langchain.text_splitter'`, update to a LangChain version compatible with the notebook or switch to the newer splitters package used by your installed LangChain release.
- If transcript retrieval fails, check whether the video has captions enabled or try another video ID.
- If Hugging Face requests fail, confirm that `HUGGINGFACEHUB_API_TOKEN` is set correctly and has access to the selected model.
- If FAISS installation fails on your platform, reinstall dependencies inside a clean virtual environment.

## Next Improvements

- Add a `requirements.txt` for one-command environment setup.
- Parameterize the video ID and question input in a small UI.
- Add error handling for empty transcripts and API failures.
- Package the notebook flow into a reusable Python module.

## License

No license has been specified yet.