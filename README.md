# Product Health Monitor — Multi-Agent Voice System

A conversational AI voice agent built with ElevenLabs that lets PMs query product health data through natural speech. The system uses a greeter router with three specialist subagents, each scoped to its own knowledge base to prevent data leakage between domains.

## Architecture

```
Voice Input
  │
  ▼
Greeter (intent classification + routing)
  ├──────────────────┼──────────────────┐
  ▼                  ▼                  ▼
User Feedback    Support Ticket    Product Metrics
  Analyst           Analyst           Analyst
  (KB: Feedback)    (KB: Tickets)     (KB: Metrics)
  │    ▲    ▲       │    ▲    ▲       │    ▲    ▲
  │    │    └───────►│    │    └──────►│    │    │
  │    └─────────────┘    └───────────►│    │    │
  └──────────────────────────────────────────┘    │
  ◄───────────────────────────────────────────────┘
  │                  │                  │
  ▼                  ▼                  ▼
                  End Call
```

**5 nodes, 12 LLM-driven edges:**
- 3 forward edges (Greeter → each specialist)
- 3 end edges (each specialist → End Call)
- 6 cross-routing edges (between specialists)

## Key Design Patterns

### Scoped Knowledge Bases
Each specialist gets **only one** KB file. The User Feedback Analyst cannot reference support ticket churn risk data when discussing NPS scores. The Support Ticket Analyst cannot quote product engagement metrics when discussing ticket volume. This strict scoping forces information boundaries — a critical pattern for production AI systems.

### Cross-Routing Between Specialists
Six bidirectional edges enable seamless topic changes mid-conversation. A user can start with "What's our NPS?" (routes to Feedback Analyst), then ask "How many support tickets are about search?" (cross-routes to Support Ticket Analyst) without restarting.

### Boundary Enforcement
Each specialist is explicitly instructed to decline out-of-scope questions and offer to redirect. This prevents hallucination from one domain bleeding into another.

### Data-First Tone
The agent uses a "sharp, analytical" tone — no pleasantries like "Great question!" Every metric includes its benchmark and target status. "Dashboard p50 is 3.2 seconds against a target of 2 seconds — 60% above target, flagged as critical."

## Repo Structure

```
├── README.md
├── agents/
│   ├── greeter/
│   │   └── system-prompt.md           ← Intent classifier + routing logic
│   ├── user-feedback-analyst/
│   │   └── system-prompt.md           ← NPS, reviews, interviews, feature requests
│   ├── support-ticket-analyst/
│   │   └── system-prompt.md           ← Tickets, CSAT, churn risk, capacity
│   └── product-metrics-analyst/
│       └── system-prompt.md           ← DAU, conversion, retention, MRR, perf
├── config/
│   ├── global-system-prompt.md        ← Shared rules for all agents
│   └── edge-conditions.md            ← All 12 LLM edge conditions
├── knowledge-base/
│   ├── KB_User_Feedback.txt           ← NPS, app reviews, interviews, in-app feedback
│   ├── KB_Support_Tickets.txt         ← Volume, categories, CSAT, churn risk
│   └── KB_Product_Metrics.txt         ← Revenue, engagement, conversion, retention, perf
└── docs/
    └── testing-scenarios.md           ← 5 test cases with expected responses
```

## How to Recreate

1. Create a new Conversational AI agent in [ElevenLabs](https://elevenlabs.io/)
2. Set agent name to **TaskFlow Product Health Monitor**
3. Paste the global system prompt from `config/global-system-prompt.md`
4. Create 5 nodes: Greeter, User Feedback Analyst, Support Ticket Analyst, Product Metrics Analyst, End Call
5. Paste each agent's system prompt from `agents/<name>/system-prompt.md`
6. Upload the corresponding KB file to each specialist node (one file per agent)
7. Add all 12 edge conditions from `config/edge-conditions.md`
8. Test with the scenarios in `docs/testing-scenarios.md`

## Testing Scenarios

| # | Say This | Expected Routing | Key Data Points |
|---|----------|-----------------|-----------------|
| 1 | "What's our NPS looking like?" | User Feedback Analyst | NPS 38 (down from 45), 1,842 responses, dashboard loading top detractor (34%) |
| 2 | "Which enterprise accounts are we at risk of losing?" | Support Ticket Analyst | Top 5: Acme Corp ($120K), GlobalTech ($95K), Pinnacle ($88K), DataDrive ($78K), Brightpath ($67K) |
| 3 | "How's our conversion funnel doing?" | Product Metrics Analyst | 8.4% free-to-paid (down from 11.2%), 38% drop from performance issues |
| 4 | Start with feedback, then ask about tickets | Cross-route: Feedback → Support | Seamless transition with domain-specific answers |
| 5 | Ask Support Ticket Analyst about MRR | Boundary test | Should decline and offer to redirect to Product Metrics |

## Live Demo

**Try the agent:** [Product Health Monitor on ElevenLabs](https://elevenlabs.io/app/talk-to?agent_id=agent_8701kkdsak5vfyyrxj4qnxqjnx0d&branch_id=agtbrch_1001kkdsana4f6mb5d8qrsq3dk39)

## Built With

- [ElevenLabs Conversational AI](https://elevenlabs.io/) — multi-agent voice orchestration
- LLM-driven intent routing — 12 edge conditions for classification and cross-routing
- Scoped knowledge bases — strict information boundaries per specialist
