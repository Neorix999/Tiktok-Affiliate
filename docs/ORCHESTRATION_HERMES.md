# Orchestration — Hermes runtime contract

Hermes is the **orchestrator/runtime** that runs the agent stack. It doesn't do
the creative work — it decides *what runs when*, drives the Airtable state
machine, stops at the four human gates, and routes notifications.

Think of it as the conductor; the six agents are the players; Airtable is the
score.

## What Hermes owns
1. **Triggering** — starts each agent when its trigger condition is met
   (`schedule`, `after: <agent>`, or `status: <value>`). See
   [`../orchestration/stack.yaml`](../orchestration/stack.yaml).
2. **State transitions** — after an agent finishes, Hermes moves the record to
   the next status (or into a gate's inbox status).
3. **Gates** — at a gate, Hermes **stops the record** and notifies the operator.
   It resumes only when the human advances the status.
4. **Notifications** — pings the operator at each gate + on error (Slack / email /
   Airtable view).
5. **Multi-tenant fan-out** — runs the same pipeline per `Tenant/Client`, keeping
   state isolated (see [`MULTI_TENANT.md`](MULTI_TENANT.md)).
6. **Retries / errors** — reruns failed agent steps, logs to `Run Log`.

## The runtime contract (what an "agent" is to Hermes)
Each agent is a unit Hermes can invoke with:
- **Input:** the Airtable records in its `reads.status` (+ its spec file for behavior).
- **Work:** call its declared MCP tools.
- **Output:** write fields + set `writes.status`.
- **Return:** `ok` | `needs-human` | `error` (logged to `Run Log`).

Hermes never lets an agent write past a gate — the agent's `writes.status` for a
gated step is the gate's `inbox_status`, and only a human sets `advance_to`.

## Two ways to run it (pick per environment)
- **Hermes-native:** register `stack.yaml` with Hermes; it schedules Researcher,
  reacts to status changes, and manages gates automatically.
- **Chat-driven (no runtime):** I act as the orchestrator on request — "run
  Scout", "build the clips" — stopping at each gate. Good for day-one before
  Hermes is fully wired. Same state machine, same gates.

## Mapping stack.yaml → Hermes
| stack.yaml | Hermes concept |
|------------|----------------|
| `agents[].trigger` | job trigger (cron / event / status-watch) |
| `agents[].reads/writes` | input query + output transition |
| `agents[].tools` | tool permissions granted to that job |
| `gates` | approval steps (pause + notify + wait) |
| `notifications` | operator alert channels |

> ⚠️ Because Hermes isn't visible as a connected MCP server in this session, this
> file is the **integration spec**: it defines exactly what Hermes must trigger,
> read, write, and gate. Wiring it to the actual Hermes instance is the one step
> that needs your environment. Until then, run chat-driven — behavior is identical.
