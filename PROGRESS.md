# UdaPlay Project Progress Tracker

**Repository:** [ru14/UdaPlay-Intelligent-Analytics-Platform](https://github.com/ru14/UdaPlay-Intelligent-Analytics-Platform)  
**Last Updated:** 2026-10-02  
**Status:** Planning & Implementation Phase

---

## 📌 Overview

UdaPlay is an AI-powered gaming research agent with a two-tier retrieval architecture:
1. **Local RAG** via ChromaDB vector store
2. **Tavily web search fallback** for queries outside local knowledge

**Tech Stack:**
- Python 3.11+ (Jupyter Notebooks)
- ChromaDB (vector DB)
- OpenAI (embeddings & LLM)
- Tavily (web search)
- Pydantic (data validation)

---

## 📊 Part 1: Offline RAG (Vector Database)
**Notebook:** `project/starter/Udaplay_01_starter_project.ipynb`

### Checklist

- [ ] **Setup & Dependencies**
  - [ ] Environment variables loaded (.env file)
  - [ ] Imports configured (chromadb, embeddings_functions)
  - [ ] pysqlite3 workaround (if needed)

- [ ] **ChromaDB Client**
  - [ ] Create PersistentClient at `chromadb/` path
  - [ ] Verify connection to persistent storage
  - [ ] Path: `project/starter/chromadb/`

- [ ] **Embedding Function**
  - [ ] Use OpenAIEmbeddingFunction
  - [ ] Set `api_key_env_var="OPENAI_API_KEY"`
  - [ ] Set `api_base=os.getenv("OPENAI_BASE_URL")`
  - [ ] **Keep identical in Part 2**

- [ ] **Collection Creation**
  - [ ] Name: `udaplay` (required for Part 2)
  - [ ] Use `chroma_client.get_or_create_collection()`
  - [ ] Attach embedding function to collection

- [ ] **Data Processing**
  - [ ] Load all JSON files from `project/starter/games/`
  - [ ] Files: 001.json through 015.json (15 games total)
  - [ ] Each file has: Name, Platform, Genre, Publisher, Description, YearOfRelease

- [ ] **Document Indexing**
  - [ ] Format: `[Platform] Name (Year) - Description`
  - [ ] Use filename (001, 002, etc.) as document ID
  - [ ] Store full game metadata in collection metadata
  - [ ] Call `collection.add()` for each game

- [ ] **Verification**
  - [ ] Perform test semantic search query
  - [ ] Confirm results return games with similarity scores
  - [ ] Save notebook with outputs

### Data Schema
```json
{
  "Name": "Gran Turismo",
  "Platform": "PlayStation 1",
  "Genre": "Racing",
  "Publisher": "Sony Computer Entertainment",
  "Description": "A realistic racing simulator...",
  "YearOfRelease": 1997
}
```

### Sample Test Queries
- "When was Pokémon Gold and Silver released?"
- "Which Mario game was first 3D platformer?"

---

## 🤖 Part 2: Agent Implementation
**Notebook:** `project/starter/Udaplay_02_starter_project.ipynb`

### Checklist

- [ ] **Tool 1: retrieve_game**
  - [ ] Function signature: `retrieve_game(query: str) -> List[Dict]`
  - [ ] Load ChromaDB client: `PersistentClient(path="chromadb")`
  - [ ] Get collection: `get_collection("udaplay")`
  - [ ] Perform semantic search: `collection.query(query_texts=[query], n_results=3)`
  - [ ] Extract and format results with Platform, Name, Year, Description
  - [ ] Docstring included

- [ ] **Tool 2: evaluate_retrieval**
  - [ ] Function signature: `evaluate_retrieval(question: str, retrieved_docs: List[Dict]) -> EvaluationReport`
  - [ ] Use LLM as judge (OpenAI model)
  - [ ] Prompt: "Evaluate if documents are sufficient to answer the question. Give detailed explanation."
  - [ ] Return: `EvaluationReport(useful: bool, description: str, confidence: float)`
  - [ ] Docstring included

- [ ] **Tool 3: game_web_search**
  - [ ] Function signature: `game_web_search(query: str) -> List[Dict]`
  - [ ] Use Tavily client: `tavily_client = TavilyClient(api_key=TAVILY_API_KEY)`
  - [ ] Perform web search: `tavily_client.search(query, topic="general")`
  - [ ] Extract results with URL, title, content
  - [ ] Format for citation: Include source URLs
  - [ ] Docstring included

- [ ] **Agent Class**
  - [ ] Implement as a class or using StateMachine
  - [ ] Initialize with tools, LLM model, system prompt
  - [ ] Maintain conversation state (memory)
  - [ ] Implement agent loop: retrieve → evaluate → fallback
  - [ ] System prompt covers agent role, instructions, tool usage

- [ ] **State Machine Workflow**
  - [ ] State 1: Retrieve from vector DB
  - [ ] State 2: Evaluate results
  - [ ] State 3: Decide (good enough or web search?)
  - [ ] State 4: Web search (if needed)
  - [ ] State 5: Generate final answer with citations

- [ ] **Agent Invocation**
  - [ ] Query 1: "When was Pokémon Gold and Silver released?" ✅ Local dataset
  - [ ] Query 2: "Which one was the first 3D platformer Mario game?" ✅ Local dataset
  - [ ] Query 3: "Was Mortal Kombat X released for PlayStation 5?" ❌ Web fallback
  - [ ] Output shows step-by-step reasoning for each query
  - [ ] Each output includes: retrieve → evaluate → final answer
  - [ ] Citations included when from web

### Supporting Library Modules

Available in `project/starter/lib/`:

| Module | Purpose | Status |
|--------|---------|--------|
| `agents.py` | Agent orchestration framework | ✓ Provided |
| `state_machine.py` | State machine abstraction | ✓ Provided |
| `vector_db.py` | ChromaDB wrapper & utilities | ✓ Provided |
| `rag.py` | RAG pipeline implementation | ✓ Provided |
| `evaluation.py` | Retrieval evaluation logic | ✓ Provided |
| `llm.py` | LLM abstraction (OpenAI) | ✓ Provided |
| `memory.py` | Conversation memory & state | ✓ Provided |
| `tooling.py` | Tool decorator & registration | ✓ Provided |
| `messages.py` | Message classes (UserMessage, etc.) | ✓ Provided |
| `loaders.py` | Data loaders for games | ✓ Provided |

---

## 🎯 Submission Rubric

### 1. RAG Pipeline ✅
- [ ] Part 1 notebook loads and processes game JSON files
- [ ] Data added to persistent ChromaDB with embeddings
- [ ] Vector DB queries demonstrate semantic search
- [ ] **Status:** Ready (Part 1)

### 2. Agent Development ✅
- [ ] At least 3 tools implemented:
  - [ ] retrieve_game (vector DB search)
  - [ ] evaluate_retrieval (quality assessment)
  - [ ] game_web_search (web fallback)
- [ ] Each tool is a function/class integrated in workflow
- [ ] Agent implements state machine
- [ ] Agent remembers context across queries
- [ ] **Status:** Ready (Part 2)

### 3. Demo Queries ✅
- [ ] Part 2 notebook runs 3 example queries
- [ ] Output includes reasoning & tool usage
- [ ] At least 1 query triggers web fallback
- [ ] Results are cited (URLs from web search)
- [ ] **Status:** Ready (Part 2)

### 4. Deliverables ✅
- [ ] `Udaplay_01_starter_project.ipynb` — completed with outputs
- [ ] `Udaplay_02_starter_project.ipynb` — completed with outputs
- [ ] All cells executed and saved
- [ ] **Status:** Ready for submission

---

## 🌟 Stand-Out Features (Optional)

- [ ] **Personalize Dataset**
  - [ ] Add more games to `games/` folder
  - [ ] Show richer queries with custom data

- [ ] **Advanced Memory**
  - [ ] Use persistent ChromaDB for long-term memory
  - [ ] Agent "learns" from web search results
  - [ ] Save insights across sessions

- [ ] **Structured Output**
  - [ ] Return answers as JSON + natural language
  - [ ] Include metadata: confidence, sources, reasoning steps

- [ ] **Visualization**
  - [ ] Dashboard of retrieval process
  - [ ] Knowledge base visualization

- [ ] **Custom Tools**
  - [ ] Sentiment analysis of game reviews
  - [ ] Trending games detection
  - [ ] Platform-specific analytics

---

## 📁 File Structure Reference

```
project/starter/
├── .env                                    # API keys (create from .env.example)
├── requirements.txt                        # Dependencies
├── chromadb/                               # Persistent vector store (created by Part 1)
│   └── [generated by ChromaDB]
├── games/                                  # Game dataset (15 JSON files)
│   ├── 001.json → 015.json
├── lib/                                    # Supporting library
│   ├── __init__.py
│   ├── agents.py                          # Agent class
│   ├── state_machine.py                   # State machine
│   ├── vector_db.py                       # ChromaDB wrapper
│   ├── rag.py                             # RAG pipeline
│   ├── evaluation.py                      # Evaluation logic
│   ├── llm.py                             # LLM abstraction
│   ├── memory.py                          # Memory management
│   ├── tooling.py                         # Tool decorators
│   ├── messages.py                        # Message types
│   ├── documents.py                       # Document types
│   ├── parsers.py                         # Output parsers
│   └── loaders.py                         # Data loaders
├── Udaplay_01_starter_project.ipynb       # Part 1: Vector DB (TO DO)
└── Udaplay_02_starter_project.ipynb       # Part 2: Agent (TO DO)
```

---

## 🔧 Configuration Checklist

### Environment Variables (.env)
```bash
OPENAI_API_KEY="voc-..."
OPENAI_BASE_URL="https://openai.vocareum.com/v1"
TAVILY_API_KEY="tvly-..."
```

### Critical Settings (Must Match Between Part 1 & 2)
| Setting | Value | Used In |
|---------|-------|---------|
| Working directory | `project/starter/` | Both notebooks |
| ChromaDB path | `chromadb/` | Both notebooks |
| Collection name | `udaplay` | Both notebooks |
| Embedding function | `OpenAIEmbeddingFunction` | Both notebooks |

### Troubleshooting
- ❌ **Collection not found in Part 2?** → Check path & name match Part 1
- ❌ **No results from retrieve?** → Verify chromadb/ folder exists and is not deleted
- ❌ **API errors?** → Verify .env variables and API keys are valid

---

## 📝 Notes & Tips

1. **Run from correct directory:** Both notebooks must run from `project/starter/`
2. **Don't delete chromadb/:** Part 1 creates it; Part 2 loads from it
3. **pysqlite3 workaround:** Only needed in Udacity workspace (auto-skips locally)
4. **Test incrementally:** Run Part 1 fully before starting Part 2
5. **Save with outputs:** Notebooks must have cell outputs for grading
6. **Consistent settings:** Any change to embedding function/collection name must be in both notebooks

---

## 🔗 Quick Links

- **Repository:** https://github.com/ru14/UdaPlay-Intelligent-Analytics-Platform
- **Project README:** `/project/README.md`
- **Starter README:** `/project/starter/README.md`
- **Main README:** `/README.md`

---

**Last Updated:** 2026-10-02  
**Maintenance:** Update this file as implementation progresses
