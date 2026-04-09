# LLM Edge Conditions

All 12 edges that control routing between nodes. Paste each condition into the corresponding edge in ElevenLabs.

## Forward Edges from Greeter (3 edges)

### Greeter → User Feedback Analyst
```
The user is asking about NPS, net promoter score, user feedback, app reviews, app store
ratings, user interviews, user sentiment, feature requests, in-app feedback, or what
users are saying.
```

### Greeter → Support Ticket Analyst
```
The user is asking about support tickets, ticket volume, ticket categories, resolution
times, CSAT, customer satisfaction scores, escalations, churn risk, at-risk accounts,
or support team capacity.
```

### Greeter → Product Metrics Analyst
```
The user is asking about product metrics, DAU, WAU, engagement, feature usage, conversion
rates, free-to-paid conversion, retention, cohort analysis, MRR, revenue, page load times,
performance, error rates, uptime, or competitive win/loss data.
```

## End Call Edges (3 edges — same condition for all)

### User Feedback Analyst → End Call
### Support Ticket Analyst → End Call
### Product Metrics Analyst → End Call

```
The user has indicated they are done, said goodbye, thanked the agent, or has no more questions.
```

## Cross-Routing Edges (6 edges)

### User Feedback → Support Ticket Analyst
```
The user is now asking about support tickets, ticket volume, ticket categories, resolution
times, CSAT, churn risk, at-risk accounts, or support team capacity instead of user
feedback topics.
```

### User Feedback → Product Metrics Analyst
```
The user is now asking about product metrics, DAU, WAU, engagement, conversion rates,
retention, MRR, revenue, page load times, performance, error rates, or competitive data
instead of user feedback topics.
```

### Support Ticket → User Feedback Analyst
```
The user is now asking about NPS, user feedback, app reviews, user interviews, user
sentiment, feature requests, or in-app feedback instead of support ticket topics.
```

### Support Ticket → Product Metrics Analyst
```
The user is now asking about product metrics, DAU, WAU, engagement, conversion rates,
retention, MRR, revenue, page load times, performance, error rates, or competitive data
instead of support ticket topics.
```

### Product Metrics → User Feedback Analyst
```
The user is now asking about NPS, user feedback, app reviews, user interviews, user
sentiment, feature requests, or in-app feedback instead of product metrics topics.
```

### Product Metrics → Support Ticket Analyst
```
The user is now asking about support tickets, ticket volume, ticket categories, resolution
times, CSAT, churn risk, at-risk accounts, or support team capacity instead of product
metrics topics.
```
