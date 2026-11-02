# Dream11 Genie 🏏
### AI-Powered Cricket Fantasy Team Selection Agent

Dream11 Genie is an AI-powered cricket fantasy team selection project that demonstrates how an LLM agent can use tools and structured data to produce a fantasy playing XI.

Built with **Python, LangChain, LangGraph, and Groq**, the application follows the **ReAct (Reason → Act → Observe)** pattern. The agent decides when to call its tools, uses the returned cricket information to make selections, and produces a validated team with a captain, vice-captain, and a concise reason for every pick.

The project currently runs on deterministic, built-in sample data, so it can be tested offline. Its modular tool layer is designed so real cricket or sports API calls can replace the sample data without requiring changes to the agent, output schema, or UI.

## Key Features

- **ReAct agent workflow:** Uses an LLM-driven reason, act, and observe loop.
- **Native tool calling:** The agent can decide when to invoke its four cricket-data tools.
- **Fantasy XI generation:** Produces 11 player selections, including captain and vice-captain recommendations.
- **Structured output:** Returns a validated `Team` object for consistent rendering.
- **Rule-based validation:** Enforces team-composition constraints, including role quotas, a maximum of 100 credits, and no more than 7 players from one team. Includes deterministic credit repair.
- **Streamlit interface:** Provides a web UI to run the agent and view its recommendations.
- **Offline testing:** Sample data and tests allow the tools and output schema to be checked without a live sports API or Groq key.
- **API-ready design:** Tool implementations can be connected to a real sports data provider while keeping the agent and UI unchanged.

## Tech Stack

| Technology | Purpose |
|---|---|
| Python | Core application and agent logic |
| LangChain `create_agent` | Agent orchestration |
| LangGraph | Underlying agent execution graph |
| Groq | LLM inference |
| Streamlit | Interactive web interface |
| Pydantic / structured schema | Validated team output |

## How It Works

1. The user provides a fixture, such as `CSK vs MI`.
2. The ReAct agent determines which cricket-data tools it needs.
3. The tools return information from the bundled sample dataset.
4. The agent uses the observations to select a fantasy XI.
5. The validation layer checks the team against the configured constraints and repairs credit issues deterministically where possible.
6. The application returns the team, captain, vice-captain, and a one-line explanation for each selection.

## Project Structure

| File | Responsibility |
|---|---|
| `data.py` | Sample squads, player form notes, and pitch report |
| `tools.py` | Four tools available to the agent |
| `validate.py` | Team-composition rules, credit checks, and deterministic credit repair |
| `schema.py` | Structured `Team` and `Pick` output schema |
| `agent.py` | LangGraph ReAct agent and `pick_team()` function |
| `run.py` | Command-line runner and agent trace |
| `app.py` | Streamlit web interface |
| `test_offline.py` | Offline checks for tools and schema |
| `requirements.txt` | Python dependencies |
| `.env.example` | Example environment configuration |

## Getting Started

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd <repository-folder>
```

### 2. Create and activate a virtual environment

```bash
python3 -m venv .venv

# macOS / Linux
source .venv/bin/activate

# Windows PowerShell
.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure your Groq API key

Copy the example environment file:

```bash
cp .env.example .env
```

Add your Groq API key to `.env`. You can create a key at [Groq Console](https://console.groq.com/keys).

> The offline test does not require a Groq API key. Running the LLM-powered agent does.

## Run the Project

### Offline validation

Run the offline checks for the tools and structured schema:

```bash
python test_offline.py
```

### Command-line agent

Run with the default fixture:

```bash
python run.py
```

Or specify a fixture:

```bash
python run.py "CSK vs MI"
```

Hide the reasoning trace:

```bash
python run.py "CSK vs MI" -q
```

### Streamlit web application

```bash
streamlit run app.py
```

## Sample Output

The agent returns a fantasy team containing:

- **11 selected players**
- **Captain and vice-captain**
- **A one-line selection rationale for each player**
- **A team validated against the configured fantasy constraints**

The exact recommendations depend on the fixture data and the agent's tool observations.

## Extending the Project with Live Data

The current implementation uses sample data in `data.py` so the project remains deterministic and can run offline.

To connect a live cricket data provider, replace the sample-data logic inside `tools.py` with API integrations while preserving the existing tool signatures. The agent, structured output schema, validation layer, and Streamlit UI are designed to remain unchanged.

## Project Scope

This project focuses on demonstrating an agentic workflow, tool orchestration, structured LLM output, and application-level validation. It is a technical demonstration and does not connect to Dream11 or submit teams to any fantasy sports platform. Current player information and recommendations are based on the bundled sample dataset, not live match data.
