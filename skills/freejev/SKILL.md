---
name: freejev
description: Use Jev through FreeJev to classify text, answer probabilistic yes/no questions, or score input against an ordered rubric. Use for structured decisions in scripts and Agent workflows, not free-form writing or reasoning explanations.
---

# FreeJev

Use the configured `jev_decide` MCP tool, or the bundled CLI (`freejev --help`). Both use the same API and account credits. Installation does not authenticate. Prefer remote MCP at https://freejev.org/mcp: the operator signs in and authorizes the connection. For CLI or legacy stdio, the operator supplies `FREEJEV_API_KEY` through their secret store. Never print or embed a key in a request file. `FREEJEV_BASE_URL` selects an explicitly requested development server; default is https://freejev.org.

Before submitting, establish the user's input, judgment question, and allowed answers. Send only the data the user intends to evaluate; avoid secrets and unrelated context. Calls consume credits, so keep the submitted scope bounded and use the existing authorization for that task.

Choose and save one stable `request_id` for this intended paid call before sending it; reuse it after a lost response. Missing IDs are rejected. Create a request JSON file:

```json
{
  "request_id": "ticket-evaluation-001",
  "state": "A customer cannot sign in.",
  "questions": {
    "team": {
      "type": "choice",
      "instructions": "Which team should handle this ticket?",
      "criteria": {"support": "Account or technical problems", "sales": "Purchasing questions"}
    }
  }
}
```

- `choice`: criteria object maps labels to definitions.
- `noul`: returns `noul`, the probability of Yes (0–1). Optional criteria keys are `true` and `false`.
- `score`: criteria is an ordered array of 2–10 level descriptions. Result is a zero-based probability-weighted score, possibly fractional.
- Reuse `state` across up to 16 questions per request; total JSON must fit 128 KB. Avoid sending overlapping requests on the same account.

Run `freejev decide --request request.json`, or use exported questions with `freejev decide --config judgment.json --file input.txt --request-id ticket-evaluation-001`. A pipe can replace `--file`. Output is JSON. `freejev usage` / `freejev_usage` reads remaining credits without model inference.

Report the returned answer and relevant probabilities, plus usage when useful. A probability is not a correctness guarantee or permission to execute an action. Jev does not produce reasoning; do not invent an explanation attributed to it.

On 401/403, resolve credentials or verified-account access. On 402, stop and report insufficient credits; do not purchase automatically. On 429 or an in-progress conflict, wait for the running request or rate window. After connection loss, check usage by request ID before proposing a new request. The API records metadata, not response history: repeating an ID returns a conflict without another charge; using a new ID may incur a new charge. Do not silently retry a billable call.

Manage or revoke remote connections at https://freejev.org/connections. Revocation stops future calls but does not undo a call already started. Full installation help: https://freejev.org/docs/agents.
