# Testing Scenarios

Five scenarios to validate routing, data accuracy, cross-routing, and boundary enforcement.

## Test 1: User Feedback Routing

**Say:** "What's our NPS looking like?"

**Expected:** Routes to User Feedback Analyst. Should report:
- NPS is 38, down from 45 in January, based on 1,842 responses
- Top detractor reason: dashboard loading slow (34%)
- Top promoter reason: Kanban view (41%)

## Test 2: Support Ticket Routing

**Say:** "Which enterprise accounts are we at risk of losing?"

**Expected:** Routes to Support Ticket Analyst. Should name the top 5 at-risk accounts:
1. Acme Corp ($120K ARR) — 14 tickets, dashboard performance, champion gone silent
2. GlobalTech ($95K ARR) — formal complaint, evaluating Monday.com
3. Pinnacle Systems ($88K ARR) — downgraded to monthly billing
4. DataDrive ($78K ARR) — escalating tone, "if not fixed by March 15, we are leaving"
5. Brightpath ($67K ARR) — reduced seats 150 → 80, parallel testing alternatives

## Test 3: Product Metrics Routing

**Say:** "How's our conversion funnel doing?"

**Expected:** Routes to Product Metrics Analyst. Should walk through:
- 62% activation → 34% aha moment → 24.7% conversion from aha to paid
- Overall 8.4% free-to-paid (down from 11.2%)
- Top drop-off reasons: 38% performance issues during trial, 24% mobile bugs

## Test 4: Cross-Routing

**Say:** Start with "What are users saying about search?" (routes to Feedback), then ask "How many support tickets are about search?"

**Expected:** Cross-routes from User Feedback Analyst to Support Ticket Analyst. Should answer:
- 534 search tickets (13% of total)
- Breakdown by type
- Engineering assessment: Elasticsearch migration proposed, 6-8 weeks, in backlog

## Test 5: Boundary Test

**Say:** Get routed to Support Ticket Analyst, then ask "What's our MRR?"

**Expected:** Support Ticket Analyst should decline: "That is in the product metrics track. Want me to redirect you?" — NOT answer with revenue data.
