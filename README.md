# ✈️ Multi-Agent Trip Planner.

An AI-powered travel planning system where four specialized agents collaborate through a **LangGraph** state graph to research flights, hotels, and generate a complete, personalized itinerary — end to end, from a single natural-language request.

## How it works

The system is a sequential multi-agent pipeline built with LangGraph:

```
START → Flight Agent → Hotel Agent → Itinerary Agent → Final Agent → END
```

- **✈️ Flight Agent** — searches for flights matching the user's query
- **🏨 Hotel Agent** — finds hotel options via Tavily web search
- **🗓️ Itinerary Agent** — uses Groq's LLaMA 3.3 70B to build a day-by-day itinerary from the flight and hotel data
- **🧠 Final Agent** — synthesizes everything into a polished, final travel recommendation

Conversation state is persisted in **PostgreSQL** via `langgraph-checkpoint-postgres`, so trip-planning sessions can be resumed and reviewed by `thread_id`.

## Tech Stack

- **LangGraph** — multi-agent orchestration and state management
- **Groq** (LLaMA 3.3 70B) — fast LLM inference for itinerary generation
- **Tavily Search API** — real-time hotel/web search
- **AviationStack API** — flight search data
- **PostgreSQL** — persistent conversation checkpointing
- **Streamlit** — interactive frontend UI
- **Python** (`psycopg`, `python-dotenv`, `langchain-groq`, `langchain-community`)

## Features

- 🔄 Live agent pipeline visualization — watch each agent run in real time in the UI
- 💾 Auto-saves every generated plan as a Markdown file, with a one-click download button
- 🧵 Session memory via `thread_id` — resume past planning sessions
- 🎨 Custom dark-themed UI with quick-pick destination chips

## Project Structure

```
├── main.py           # LangGraph agent pipeline (backend)
├── frontend.py        # Streamlit UI
├── tools/
│   ├── flight_tool.py  # Flight search integration
│   └── tavily_tool.py  # Hotel/web search integration
├── .env               # API keys & DB connection (not committed)
└── .gitignore
```

## Setup

1. **Clone the repo**

```bash
git clone https://github.com/Bindukeshav/trip-planner--multi-agent-system-demo.git
cd trip-planner--multi-agent-system-demo
```

2. **Create a virtual environment and install dependencies**

```bash
python -m venv venv
source venv/Scripts/activate   # Windows (Git Bash)
pip install langgraph langchain langchain-openai langchain-groq langchain-community langchain-tavily "psycopg[binary]" psycopg_pool langgraph-checkpoint-postgres python-dotenv tavily-python requests streamlit
```

3. **Set up your `.env` file**

```
GROQ_API_KEY=your_groq_key
TAVILY_API_KEY=your_tavily_key
DATABASE_URL=postgresql://user:password@localhost:5432/your_db
```

4. **Run it**

CLI mode:

```bash
python main.py
```

Web UI:

```bash
streamlit run frontend.py
```

## Example

> "Plan a complete 7-day Japan trip including flights, hotels and sightseeing under ₹2 lakhs"

The system searches flights, finds hotels, builds a day-by-day itinerary, and returns a complete downloadable travel plan.

---

Built by [Bindu Keshav](https://github.com/Bindukeshav)
