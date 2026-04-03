# 🧠 Deep Intel Study Agent (Gemma 4 RAG Pipeline)

An autonomous, local-first Retrieval-Augmented Generation (RAG) study assistant built to parse, index, and query chaotic, multi-format university study materials. 

Built with **Gemma 4**, **LangChain**, and **Ollama**, this agent operates entirely without cloud LLM API keys, ensuring zero rate limits, total privacy, and unlimited study sessions.

## 🚀 The Problem
University notes are messy. Students deal with a chaotic mix of scanned PDFs, legacy PowerPoint slides (`.ppt`/`.pptx`), Word documents, and images of whiteboard notes. Traditional cloud-based AI tools struggle with these formats and quickly hit API rate limits or "resource exhausted" errors during heavy study sessions.

## 💡 The Solution
The Deep Intel Study Agent solves this by offloading compute to Google Colab and utilizing a 100% local, agentic AI architecture. It deep-parses multi-format files, saves the vectorized "brain" permanently to Google Drive, and uses an autonomous agent to decide when to search local notes versus searching the web.

## ⚙️ Architecture & Tech Stack
* **LLM Engine:** [Gemma 4](https://ai.google.dev/gemma) (Hosted locally via Ollama)
* **Agent Orchestration:** LangChain & LangGraph (ReAct Agent format)
* **Vector Database:** ChromaDB (Persistently mounted via Google Drive)
* **Embeddings:** HuggingFace (`all-MiniLM-L6-v2`)
* **Data Ingestion:** Unstructured (`[all-docs]`) integrated with Tesseract OCR (for images) and LibreOffice (for legacy `.ppt`/`.doc` files)
* **Web Fallback Tool:** DuckDuckGo Search API

## ✨ Key Features
* **Multi-Format Ingestion:** Seamlessly reads text from PDFs, DOCX, PPTs, and uses OCR to extract text directly from images.
* **Resilient Parsing Pipeline:** Automatically skips overly massive files (like 500-page textbooks) and gracefully handles corrupted files without crashing the pipeline.
* **Persistent Memory:** Vector embeddings are saved directly to Google Drive, meaning the heavy 25+ minute ingestion process only needs to be run *once*.
* **Agentic Reasoning:** The LLM actively evaluates user prompts. It is instructed to search the local `university_notes_search` tool first. If the notes lack the answer, it autonomously falls back to the web search tool to fill in the gaps.

---

## 🛠️ Usage (The "Daily Driver" Setup)

Because Google Colab environments wipe temporary files upon disconnecting, the daily usage is streamlined into two simple cells. 

🛡️ License & Privacy
This project runs open-weights models locally. No user prompts or personal study materials are ever sent to OpenAI, Google, or any external API servers.

Built by Abel at Deep Intel.

### 1. Wake up the Local Server
Run this bash command to spin up the background Ollama server and ensure the Gemma 4 weights are loaded into Colab's temporary memory:
```bash
!curl -fsSL [https://ollama.com/install.sh](https://ollama.com/install.sh) | sh
!nohup ollama serve > ollama.log 2>&1 &
!sleep 3
!ollama pull gemma4
