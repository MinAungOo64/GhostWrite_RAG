# ✍️ GhostWrite_RAG
GhostWrite_RAG is a privacy-first, local-only "Smart Writing Assistant." It uses a Dual-Agent architecture to help researchers and writers complete sentences based on their own local documents (PDFs, DOCX, etc.) without ever sending data to the cloud.

# 🚀 Key Features
Dual AI Agent System:

The Researcher: A RAG-based chat agent that retrieves specific context from your local files.

The Autocompleter: A "Ghost-Text" agent that suggests sentence completions in real-time as you type.

Persistent Context Caching: Manually "pin" research findings from the chat into a global memory bank to guide the autocompleter.

100% Local & Private: Runs entirely in your browser via Transformers.js and a local Ollama instance. No API keys, no data leaks.

Zero Installation: A single index.html file is all you need.

# 🛠️ Prerequisites
Ollama: Install Ollama to run the LLM locally.
Model: Pull the default model (or your preferred version):

Bash
ollama pull qwen2.5-coder:3b
CORS: Since this is a browser-based tool, you must allow Ollama to talk to your browser:

Mac/Linux: OLLAMA_ORIGINS="*" ollama serve

Windows: Set Environment Variable OLLAMA_ORIGINS to * and restart Ollama.

📖 How to Use
Open index.html in any modern web browser.

Select Folder: Click "📁 Select Docs Folder" to load your research documents.

Build Index: Click "Build Index" to vectorize your documents locally.

Chat & Pin: Use the sidebar to ask questions. Relevant chunks will be retrieved and cached.

Write: Start typing in the main editor. The "Ghost Text" suggestions will appear based on your pinned research. Press Tab to accept a suggestion.

⚙️ Configuration (Changing Models)
The system is configured to use Qwen-2.5-Coder (3B) by default for its balance of speed and technical accuracy. To change the model (e.g., to llama3 or mistral), modify the fetch calls in index.html:

JavaScript
// Search for this block in index.html to change the LLM
const response = await fetch('http://localhost:11434/api/generate', {
    method: 'POST',
    body: JSON.stringify({
        model: 'qwen2.5-coder:3b', // <--- Change model name here
        prompt,
        options: {
            temperature: 0.2,
            num_predict: 32, // Lower for faster autocomplete
            stop: ["\n\n"]
        }
    })
});
🏗️ Technical Stack
LLM Engine: Ollama (Local Inference)

Embeddings: Transformers.js (all-MiniLM-L6-v2)

Parsing: Mammoth.js (DOCX)

Frontend: Vanilla JS, HTML5, CSS3
