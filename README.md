# Multi-Agent Customer Support Automation

> **Built with:** CrewAI · OpenAI GPT · Python  
> **Author:** Pradeep Kumar

---

## What This Does

A **two-agent customer support system** built with CrewAI. Instead of a single LLM handling support end-to-end, two specialised agents collaborate:

- **Support Agent** — researches the inquiry by scraping official documentation and drafts a thorough response
- **QA Agent** — reviews the draft for accuracy, completeness, and tone before delivery

The pattern mirrors how real support teams operate: a first responder handles the inquiry, a senior reviewer ensures quality before the response reaches the customer.

---

## System Architecture

```
Customer Inquiry
      │
      ▼
┌─────────────────────────┐
│   Support Agent          │  role: Senior Support Representative
│   ─────────────────────  │  tools: ScrapeWebsiteTool (docs)
│   • Reads inquiry        │  allow_delegation: False
│   • Scrapes docs         │  memory: shared via Crew
│   • Drafts response      │
└───────────┬─────────────┘
            │  passes draft
            ▼
┌─────────────────────────┐
│   QA Agent               │  role: Support QA Specialist
│   ─────────────────────  │  tools: none (review only)
│   • Reviews draft        │  allow_delegation: True
│   • Checks accuracy      │  memory: shared via Crew
│   • Finalises tone       │
└───────────┬─────────────┘
            │
            ▼
    Final Customer Response
```

---

## Key Concepts Demonstrated

| Concept | Implementation |
|---|---|
| **Role Playing** | Each agent has a distinct `role`, `goal`, and `backstory` |
| **Focus** | Agents are prompted to stay in character and avoid assumptions |
| **Tool Use** | Support Agent uses `ScrapeWebsiteTool` to ground answers in official docs |
| **Cooperation** | QA Agent can delegate tasks back to the Support Agent if needed |
| **Guardrails** | Task `expected_output` constrains response scope and format |
| **Memory** | `memory=True` on the Crew enables agents to share context across tasks |

---

## Tech Stack

- [CrewAI](https://github.com/joaomdmoura/crewAI) — multi-agent orchestration
- [crewai-tools](https://github.com/joaomdmoura/crewAI-tools) — `ScrapeWebsiteTool`
- [OpenAI GPT-3.5-turbo](https://platform.openai.com/docs) — LLM backbone
- [python-dotenv](https://pypi.org/project/python-dotenv/) — secure API key loading

---

## How to Run

### 1. Clone the repo
```bash
git clone https://github.com/Pradeep-Kumar25th/multi-agent-customer-support.git
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Set up your API key
```bash
cp .env.example .env
# Edit .env and add your OPENAI_API_KEY
```

### 4. Launch the notebook
```bash
jupyter notebook multi_agent_customer_support.ipynb
```

Run all cells. The crew will execute and print the final QA-reviewed response.

---

## Customising the Inquiry

The crew accepts dynamic inputs — change `customer`, `person`, and `inquiry` in the inputs dict at the bottom of the notebook to test different scenarios:

```python
inputs = {
    "customer": "Acme Corp",
    "person": "Jane Smith",
    "inquiry": "How do I configure agent memory with a custom embedding model?"
}
```

---

## Project Structure

```
multi-agent-customer-support/
├── multi_agent_customer_support.ipynb   # Main notebook
├── requirements.txt                      # Dependencies
├── .env.example                          # API key template
├── .gitignore                            # Keeps secrets out of git
└── README.md                             # This file
```

---

## License

MIT — feel free to use and adapt.
