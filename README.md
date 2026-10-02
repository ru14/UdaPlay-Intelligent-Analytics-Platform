<img width="1536" height="1024" alt="Arch and agen flow" src="https://github.com/user-attachments/assets/9e4d704f-4cee-4f7c-a52e-436e2430b224" />
# UdaPlay — AI Research Agent for the Video Game Industry

UdaPlay is an AI-powered research agent for the video game industry. It combines a local retrieval pipeline with web search fallback to answer questions about games, platforms, genres, release dates, publishers, and other game metadata with grounded, citation-backed responses.

## What this repository contains

This repository is organized around the Udacity *Building Agents* coursework and includes both the project materials and the implementation workspace:

- `project/` — the main UdaPlay project materials and starter code
- `module_01_Extending_Agents_with_Tools/` through `module_10_Evaluating_Agents/` — course notebooks and supporting exercises
- `README.md` — this guide
- `LICENSE` / `LICENSE.md` — licensing information

## Project summary

The UdaPlay agent uses a two-tier retrieval architecture:

1. **Local RAG** over a persistent ChromaDB vector store for fast, structured answers from the curated game dataset.
2. **Tavily web search fallback** when local retrieval is insufficient, ambiguous, or low-confidence.

The agent also includes:

- retrieval evaluation
- confidence scoring
- source attribution
- grounded response generation
- conversation state handling
- structured outputs
- optional memory extensions

## Getting started

1. Install the dependencies and create the `.env` file as described in [Setup requirements](#setup-requirements).
2. From `project/starter/`, work through the notebooks in order:
   - [Part 1: Build the vector database](project/starter/Udaplay_01_starter_project.ipynb)
   - [Part 2: Build the agent](project/starter/Udaplay_02_starter_project.ipynb)

## Setup requirements

- Python 3.11+
- Install dependencies with `python -m pip install -r project/starter/requirements.txt`
- For local notebook execution, install Jupyter with `python -m pip install notebook`
- Create a `.env` file in `project/starter/` with the variables below
- API keys for OpenAI and Tavily

Typical environment variables:

```bash
OPENAI_API_KEY="voc-..."
OPENAI_BASE_URL="https://openai.vocareum.com/v1"
TAVILY_API_KEY="tvly-..."
```

## Key implementation areas

The starter project focuses on building and integrating the following components:

- persistent vector database creation
- retrieval with ChromaDB
- evaluation of retrieved results
- tool-based agent orchestration
- fallback web search
- memory and state management
- structured response formatting

## Validation ideas

After implementation, test the agent with questions such as:

- When was Pokémon Gold and Silver released?
- Which Mario game was the first 3D platformer?
- Was Mortal Kombat X released for PlayStation 5?
- Who published a specific game on a specific platform?

## Notes

- Keep the vector store path and collection naming consistent across notebooks.
- Do not remove the `chromadb/` directory once Part 1 has built it.
- The `pysqlite3` workaround is only needed in the Udacity workspace.
- Long-term memory is optional and should be made persistent if you want it to survive across sessions.

## License

This project is distributed under the MIT License. See [`LICENSE.md`](LICENSE.md).
