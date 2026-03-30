# ✍️ GhostWrite_RAG

**GhostWrite_RAG** is a privacy-centric, local-first "Smart Writing Assistant." It employs a **Dual-Agent** architecture to provide real-time sentence completions grounded in your personal research documents (PDFs, DOCX, etc.) without ever sending data to the cloud.

## 🚀 Key Features

* **Dual AI Agent System**: 
    * **The Researcher**: A RAG-based chat agent that retrieves specific context from your local files.
    * **The Autocompleter**: A "Ghost-Text" agent that suggests completions in real-time as you type.
* **Persistent Context Caching**: A "Memory Bank" that allows you to "pin" research findings from the chat to guide the Autocompleter.
* **100% Local & Private**: Runs entirely on-device via Transformers.js and a local Ollama instance. No API keys or external data transmission required.
* **Zero Installation**: A portable, single-file `index.html` solution.

---

## 🛠️ Prerequisites

1.  **Ollama**: Install [Ollama](https://ollama.com/) to handle local LLM inference.
2.  **Model**: Pull the default high-performance coder model:
    ```bash
    ollama pull qwen2.5-coder:3b
    ```
3.  **CORS Configuration**: To allow the browser to communicate with Ollama, you must set the `OLLAMA_ORIGINS` environment variable to `*`.
    * **Windows (PowerShell)**: `$env:OLLAMA_ORIGINS="*"; ollama serve`
    * **Mac/Linux**: `OLLAMA_ORIGINS="*" ollama serve`

---

## 📖 How to Use

1.  Open `index.html` in any modern web browser.
2.  **Select Folder**: Click **"📁 Select Docs Folder"** to load your research documents.
3.  **Build Index**: Click **"Build Index"** to vectorize your documents locally (powered by Transformers.js).
4.  **Chat & Pin**: Use the sidebar to ask questions. Relevant chunks will be retrieved and stored in the cache.
5.  **Write**: Start typing in the main editor. Gray "Ghost Text" suggestions will appear. Press **`Tab`** to accept a suggestion.

---

## ⚙️ Configuration (Changing Models)

The system is optimized for **Qwen-2.5-Coder (3B)**. To use a different model (e.g., `llama3` or `mistral`), update the `model` parameter in the `fetch` calls within `index.html`:

```javascript
// Locate this block in index.html to switch models
// RAG
const response = await fetch('http://localhost:11434/api/generate', {
    method: 'POST',
    body: JSON.stringify({
        model: 'qwen2.5-coder:3b', // <--- Change model name here
        prompt: prompt,
        options: {
            temperature: 0.2,
            num_predict: 32, // Keeps autocomplete snappy
            stop: ["\n\n"]
        }
    })
});
// AutoCompleter
const response = await fetch('http://localhost:11434/api/generate', {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        model: "qwen2.5-coder:3b",
                        prompt,
                        stream: false,
                        options: {
                            temperature: 0.2,
                            num_predict: 32,
                            stop: ["\n\n"]
                        }
                    })
                });
```
