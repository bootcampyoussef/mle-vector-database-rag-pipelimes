# Vector Databases, Similarity Search and RAG Pipelines

A notebook-based learning path for building retrieval-augmented generation (RAG) systems from their retrieval foundation upward. The notebooks make document splitting, vector similarity, FAISS indexing, grounded generation, and retrieval evaluation inspectable before you move to managed vector-database deployments or orchestration frameworks.

The notebooks use a small shared course-handbook corpus and real FAISS indexes. The generation notebook can call [Groq](https://console.groq.com/docs) or [Mistral](https://docs.mistral.ai/) through the OpenAI Python library; without an API key it uses an explicit deterministic dry-run response so the full path can still be executed locally.

## Learning Objectives

By the end of this repository, you should be able to:

- Implement a document chunking strategy and explain its effect on retrieval quality and grounding evidence.
- Build a vector index with FAISS and perform approximate nearest neighbor search.
- Assemble a complete RAG pipeline that embeds documents, stores and retrieves vectors, and generates a cited answer from retrieved evidence.
- Evaluate retrieval quality with HitRate@k and MRR, then diagnose failures.

## Learning Path

| File / Folder | Description |
|---|---|
| [**01 - Chunking for Retrieval**](01-chunking-for-retrieval.ipynb) | Split source documents into retrieval units, compare overlap choices, and reason about evidence lost at chunk boundaries. |
| [**02 - Embeddings and Similarity Search**](02-embeddings-and-similarity-search.ipynb) | Build transparent normalized embeddings with NumPy, compute cosine similarity, and inspect top-k retrieval results. |
| [**03 - FAISS and ANN Indexes**](03-faiss-and-ann-indexes.ipynb) | Build exact and approximate FAISS indexes, map retrieved vector IDs back to metadata, and measure the accuracy-speed trade-off. |
| [**04 - RAG Pipeline and Grounded Generation**](04-rag-pipeline-and-grounded-generation.ipynb) | Reuse a FAISS index, connect retrieval to a guarded citation-aware provider prompt, and generate a grounded answer. |
| [**05 - Retrieval Evaluation**](05-retrieval-evaluation.ipynb) | Evaluate retrievers with HitRate@k and mean reciprocal rank (MRR) and write an evidence-based improvement decision. |

### Additional Folders and Files

| File / Folder | Description |
|---|---|
| [**assets**](assets/) | Visual aids referenced in the repository. |
| [**data**](data/) | Course handbook corpus shared across notebooks. |
| [**.env.example**](.env.example) | Provider configuration template. |
| [**pyproject.toml**](pyproject.toml) | Project configuration and dependencies. |
| [**uv.lock**](uv.lock) | Dependency lock file. |

## Setup

> [!NOTE]
> Throughout these steps, text in angle brackets like `<repo-name>` is a
  **placeholder**. Replace it, including the `< >` brackets, with your own
  value. For example, `cd <repo-name>` becomes `cd mle-vector-databases-rag-pipelines`.

### 1. Create the Repository from the Template

Click **Use this template** on GitHub.

When creating the repository:

- Set yourself as the **Owner**
- Choose a repository name
- Disable **Include all branches**
- Click **Create repository**

> [!IMPORTANT]
> If you are working in pairs or groups, only **one person** should complete this step.

---

### 2. Add Collaborators (Pairs/Groups Only)

If working with teammates:

1. Open the repository on GitHub
2. Go to **Settings → Collaborators**
3. Add your teammates as collaborators
4. Share the repository link with your team

Teammates should accept the invitation before continuing.

---

### 3. Clone the Repository

Copy the SSH URL from the **Code** button on GitHub, then run:

```bash
git clone <copied-ssh-url>
```

The copied SSH URL will look like `git@github.com:<your-username>/<repo-name>.git`.

---

### 4. Move into the Project Folder and Install Dependencies

This installs all dependencies and creates a virtual environment in `.venv/`.

```bash
cd <repo-name>
uv sync
```

---

### 5. Set Up Environment Variables

Copy the example file and fill in your API keys:

```bash
cp .env.example .env
```

Open `.env` and set `LLM_PROVIDER` to `groq` or `mistral`, then add the matching key:

- **Groq (primary):** Create a key at [console.groq.com/keys](https://console.groq.com/keys) and set `GROQ_API_KEY`.
- **Mistral (alternative):** Create a key following the [Mistral API key documentation](https://docs.mistral.ai/admin/security-access/api-keys) and set `MISTRAL_API_KEY`.

Notebooks 01-03 and the retrieval evaluation in notebook 05 run without an API key. Notebook 04 also executes without a key in dry-run mode, but a real generation call is necessary to observe how a model uses retrieved evidence.

> [!CAUTION]
> Never commit `.env` to version control. It contains your personal API keys.

---

### 6. Open the Repository in VS Code

> [!NOTE]
> Make sure you open VS Code from the project root so it automatically detects the environment created by `uv sync`.

Launch VS Code in the project root folder:

```bash
code .
```

Then open a notebook and select the Python environment created by `uv sync` as the kernel.

## API Usage Notes

- Provider free tiers, rate limits, model names, and billing rules can change. Check the provider console before running live experiments.

## Professional Tooling Context

These notebooks keep the essential mechanics visible. Production systems often substitute or extend individual layers:

- Sentence Transformers and provider embedding APIs produce stronger dense text embeddings than the transparent local baseline.
- FAISS is a vector similarity library; managed databases such as Qdrant or Chroma add persistence, metadata filtering, operational APIs, and scaling choices.
- LangChain retrieval and LlamaIndex provide orchestration abstractions after the underlying retrieval decisions are understood.
- Evaluation libraries such as Ragas and TruLens extend retrieval checks with groundedness and answer-quality evaluation.
