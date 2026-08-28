# Examples: triggering plan-validation-loop

Example event payload (JSON) for triggering a validation run via webhook/event bus:

```
POST /events/validation.request
{
  "requestId": "req-20260828-001",
  "repo": "Felix-lxd/your-repo",
  "branch": "fix/bug-123",
  "commits": ["abc123"],
  "planPath": ".qoder/plans/plan-20260828-001.md",
  "initiator": "alice",
  "mode": "review-only"
}
```

Expected response: a validation run artifact URL and round report summary.
