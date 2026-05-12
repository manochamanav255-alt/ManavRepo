# Agentic AI Labs

Hands-on implementations from an agentic AI curriculum covering LangGraph, 
OpenAI Agents SDK, CrewAI, and production evaluation patterns.


| Topic | Frameworks Used |
|-------|-----------------|
| Agent Evaluation Suite | LangGraph, LangChain, LLM-as-Judge |

##  Agent Eval Suite

Built a 3-layer evaluation system for an AI customer support agent:

- **Routing accuracy** — deterministic check for correct specialist routing
- **Response quality** — LLM-as-Judge scoring on 4 dimensions
- **Trajectory correctness** — keyword verification for data usage
- **Consistency testing** — pass@k metric across multiple trials

**Results:** 100% routing accuracy, 100% pass@3 consistency on 20-case test set.

## Skills Demonstrated

- Production agent evaluation patterns
- LangGraph state machines with conditional routing
- LLM-as-Judge implementation
- Per-category metric analysis
- Eval-driven improvement workflows

## Tech Stack

- Python 3.12
- LangGraph, LangChain
- OpenAI (gpt-4.1-mini, gpt-5-mini)
- Matplotlib for visualization

## Author

Manav Manocha  
Senior Multi Cloud  AI Architect 
LinkedIn: [your-linkedin-url]# ManavRepo
Manav's AI Repository
