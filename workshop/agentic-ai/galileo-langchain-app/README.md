# Galileo-instrumented travel planner

The workshop 18 multi-agent travel planner (`../base-app`) with **Galileo
(Splunk Agent Observability)** tracing added. This is the runnable companion to
workshop 19.

Two changes vs. the base app:

1. `galileo_context.init(...)` selects the project / log stream traces land in.
2. A single `GalileoCallback` is attached to the LangGraph run config, so every
   agent node's LLM call (coordinator, flight, hotel, activity, synthesizer) is
   captured in one trace per request.

## Run locally

Requires **Python 3.10+**.

```bash
cd workshop/agentic-ai/galileo-langchain-app

python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

cp .env.example .env   # then edit .env and fill in your keys

python3 main.py
```

In a second terminal:

```bash
curl http://localhost:8080/travel/plan \
  -H "Content-Type: application/json" \
  -d '{
    "origin": "Seattle",
    "destination": "Tokyo",
    "user_request": "Planning a week-long trip from Seattle to Tokyo. Looking for a boutique hotel, business-class flights and unique experiences.",
    "travelers": 2
  }'
```

Then open the Galileo console, select project `Workshop19Galileo` and log stream
`TravelPlanner`, and inspect the latest trace.
