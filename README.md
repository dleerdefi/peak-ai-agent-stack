RinAI Advanced Chat Agent

RinAI is an advanced chat companion, powered by graph RAG (Retrieval-Augmented Generation), LLM-based context management, and real-time tool usage. The project is designed to be modular and extensible, helping enthusiasts and developers experiment with:

Components
1. Front-end web interface for chat interactions (Node + React-like stack).
2. Backend orchestrator (Node.js) handling API calls between front end and python servics.
3. Python services that perform the heavy lifting for:
    - Agent orchestration and message generation.
    - Active summarization of session context.
    - Retrieval and scoring from a large conversation corpus.
    - Hybrid query analysis using Graph RAG capabilities and semantic embeddings.
    - Tool calls to handle crypto price checks and web searches.
    - Parallel execution of tools, RAG and LLM calls.

By default, you can run all three services locally on the following ports:
- Backend (Node.js): Port 3000
- Front-end: Port 3003
- Python Services: Port 8000 (auto-starts when backend is running)
Once running, navigate to http://localhost:3003 to access the Rin web app.

Features
Multi-tier Architecture
- Node.js backend orchestrating API calls between front end and python servics.
- Python microservices for specialized tasks and LLM calls.
- A front-end single-page application for user interaction.

Rin Chat Agent
- Roleplaying & flirty conversation style (via a specialized fine-tuned LLM).
- Automatic context summarization when token limits are reached.
- Dynamic usage of a Smart LLM Gateway to decide which model to employ.
- Hybrid Query with Graph RAG

Graph RAG
- Leverages approx 14,000 messages stored in a Neo4j or other DB with advanced indexing.
- Retrieval pipeline using semantic embeddings and sentiment + subject classification.
- Hybrid search combining vector lookups and rating-based filtering.

Tool-Enabled
- Crypto Price Checker (via CoinGecko or similar APIs) for real-time or historical pricing.
- Perplexity API integration for advanced web search when queries involve current events, specialized knowledge, or critical reasoning powered by DeepSeek R1.

Summarization & Context Management
- Automated summarization once conversation tokens exceed a threshold.
- Summaries keep the latest 25% of messages untouched and summarize the older 75% in the background.

Front-End (Port 3003)
- A user-facing web app providing an interactive, “cute” interface to chat with Rin.
- All user inputs are captured here and forwarded to the Node backend.

Backend (Port 3000)
- Routes user messages to the right services.

Python Services (Port 8000)
- Applies logic to decide whether a tool should be used based on a “grok LLM call.”
- Keeps track of the conversation’s token usage.
- Integrates the final system prompt, RAG data, and tool outputs into one cohesive prompt for the LLM.
- Contains specialized scripts and endpoints for retrieval, summarization, and tool invocations.
- Maintains the Graph RAG pipeline, interacting with the conversation corpus (in Neo4j or another DB).
- Summarizes conversation when tokens exceed thresholds.
- Calls external APIs (CoinGecko, Perplexity, etc.) as needed.

Prerequisites
Before installing and running the stack, ensure you have the following:

Node.js (v16+ recommended)
Python (3.9+ recommended)
pip and/or virtual environment manager (e.g., venv, conda)
Neo4j or another graph database for storing the conversation corpus.
(Optional) Docker (if you prefer containerized deployment).

Installation and Quick Start
Follow these steps to launch the entire ecosystem quickly:

## 1. Clone the Repository
git clone https://github.com/<your-username>/peak-ai-agent.git
cd peak-ai-agent

## 2. Node.js Setup
1. Install Node.js (v18+ recommended)
   - Download from [nodejs.org](https://nodejs.org/)
   - Or use nvm (Node Version Manager):
     ```bash
     nvm install 18
     nvm use 18
     ```

2. Install Node.js dependencies:
   ```bash
   cd backend/node
   npm install
   ```

## 3. Python Environment Setup
1. Create Python virtual environment (Python 3.10+ recommended):
   ```bash
   cd backend/python_services
   python -m venv venv
   ```

2. Activate virtual environment:
   - Windows:
     ```bash
     .\venv\Scripts\activate
     ```
   - Unix/MacOS:
     ```bash
     source venv/bin/activate
     ```

3. Install Python dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## 4. Environment Configuration
Create `.env` file in `/backend` directory:

## 5. Start the Application
1. Start the server (this will launch both Node.js and Python services):
   ```bash
   cd backend/node
   node server.js
   ```

2. Access the application:
   - Open your browser and navigate to: `http://localhost:3003`
   - The backend API will be running on: `http://localhost:3000`
   - The Python service will be running on: `http://localhost:8000` it will start automatically when the backend is running.

## Common Issues
1. Port conflicts: Ensure ports 3000, 3003, and 8000 are available
2. Python venv: Make sure you're in the virtual environment when installing requirements
3. Node modules: If you get module not found errors, try `npm install` again
4. MongoDB connection: Ensure MongoDB is running and URI is correct

Observe Real-Time Conversation:

The Node.js backend checks with the Python services to see if any tools should be called (e.g., a crypto price check or web search).
The Graph RAG pipeline may retrieve relevant conversation snippets from the large message corpus, factoring in sentiment or subject matter.
Summarization triggers automatically once token thresholds are reached.
Look for Tool Usage:

If your query involves crypto prices, you’ll see the system reference the Crypto Price Checker.
If your query is about current news or specialized info, the system may invoke the Perplexity API for web search.
Receive Final Answer:

The system compiles the final answer from the tool outputs, context from the RAG engine, and the system prompt designed for Rin’s personality.

Configuration
Most configuration parameters are housed in environment variables or small config files within each subfolder. Some examples:

Backend:

PORT (default 3000)
FRONTEND_PORT (default 3003)
API keys for external services.
Python Services:

PYTHON_SERVICE_PORT (default 8000)
Paths or credentials for the DB (Neo4j, vector store).
API keys for Perplexity, CoinGecko, or other integrated tools.
Example .env for Backend

PORT=3000
FRONTEND_PORT=3003
PERPLEXITY_API_KEY=your-perplexity-key
COINGECKO_API_URL=https://api.coingecko.com/api/v3/
Example .env for Python Services

PYTHON_SERVICE_PORT=8000
NEO4J_URI=bolt://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=your-password
EMBEDDINGS_MODEL=all-mpnet-base-v2

Needing a DB Schema
For the Graph RAG functionality, you’ll want a Neo4j instance (or equivalent). We will soon provide:

DB Schema: Node labels (e.g., Message), relationships (e.g., HAS_SENTIMENT, HAS_SUBJECT), indices, constraints.
Processing Script: Bulk imports the 14,000+ Rin messages, setting attributes like sentiment, subject, effectiveness rating, etc.
Stay tuned for an update in which we release these schema details and a sample dataset or seeds for your own usage.

Extended Tooling
Additional APIs and specialized tools could be integrated (e.g., coding assistance, image generation, scheduling, and more).

Advanced Orchestration
Enhancements to the summarization logic, better concurrency handling, and plugin-based expansions for the agent’s toolkit.

Contributing
We heartily welcome and appreciate any and all contributions! To get started:

Fork this repository.
Create a new branch for your feature or fix.
Submit a Pull Request describing the changes you’ve made.
We will review PRs as quickly as we can. Please read our CONTRIBUTING.md (coming soon) for more detailed guidelines on style, commits, and testing.

License
This project is licensed under the MIT License. Feel free to use, modify, and distribute this software as stated within the license terms.