# ✈️ Multi-Agent Trip Planner

An AI-powered travel planning system where four specialized agents collaborate through a **LangGraph** state graph to research flights, hotels, and generate a complete, personalized itinerary — end to end, from a single natural-language request.

**Live demo:** https://multi-agent-trip-planner-ic0r.onrender.com (free tier, so the first load may take about a minute)

![App screenshot](123.png)



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
- **Groq** (GPT-OSS 120B) — fast LLM inference for itinerary generation
- **Tavily Search API** — real-time hotel/web search
- **AviationStack API** — flight search data
- **PostgreSQL** — persistent conversation checkpointing
- **Streamlit** — interactive frontend UI
- **Python** (`psycopg`, `python-dotenv`, `langchain-groq`)

## Features

- 🔄 Live agent pipeline visualization — watch each agent run in real time in the UI
- 💾 Auto-saves every generated plan as a Markdown file, with a one-click download button
- 🧵 Session memory via `thread_id` — resume past planning sessions
- 🎨 Custom dark-themed UI with quick-pick destination chips

## Project Structure

```
├── main.py             # LangGraph agent pipeline (backend)
├── frontend.py         # Streamlit UI
├── tools/
│   ├── flight_tool.py  # Flight search integration
│   └── tavily_tool.py  # Hotel/web search integration
├── requirements.txt    # Python dependencies
├── .env.example        # Template for API keys and DB connection
└── .gitignore
```

## Setup

**Prerequisites:** Python 3.11+, a running PostgreSQL instance, and free API keys from [Groq](https://console.groq.com), [Tavily](https://tavily.com) and [AviationStack](https://aviationstack.com).

1. **Clone the repo**

```bash
git clone https://github.com/Bindukeshav/multi-agent-trip-planner.git
cd multi-agent-trip-planner
```

2. **Create a virtual environment and install dependencies**

```bash
python -m venv venv
source venv/Scripts/activate   # Windows (Git Bash); on macOS/Linux: source venv/bin/activate
pip install -r requirements.txt
```

3. **Create the PostgreSQL database**

```bash
psql -U postgres -c "CREATE DATABASE tripplanner;"
```

4. **Set up your `.env` file** (copy `.env.example` to `.env` and fill it in)

```
GROQ_API_KEY=your_groq_key
TAVILY_API_KEY=your_tavily_key
AVIATIONSTACK_API_KEY=your_aviationstack_key
DATABASE_URL=postgresql://postgres:YOUR_PASSWORD@localhost:5432/tripplanner
```

5. **Run it**

CLI mode:

```bash
python main.py
```

Web UI:

```bash
python -m streamlit run frontend.py
```

## Example

> "Plan a complete 7-day Japan trip including flights, hotels and sightseeing under ₹2 lakhs"

The system searches flights, finds hotels, builds a day-by-day itinerary, and returns a complete downloadable travel plan.

## Limitations

- Runs on free-tier API plans, so request limits and result depth are restricted.
- Flight data comes from AviationStack, which provides schedule and status information rather than live ticket prices. Treat costs in the itinerary as estimates.
- Sequential pipeline: each agent waits for the previous one, so a full plan takes a few seconds to generate.

## Results

Measurements in progress.

---

Built by [Bindu Keshav](https://github.com/Bindukeshav)
