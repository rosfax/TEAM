# TEAMS Slack Interaction Blueprint v1

## 🧭 Overview
Defines how Slack operates as the user interface for TEAMS PDCA cycles. Each channel corresponds to a PDCA phase, and agents communicate with initiators using dual-layer messaging and HITL confirmation before public updates.

---

## 🧱 PDCA Channel Structure

| PDCA Stage | Slack Channel | Purpose |
|-------------|----------------|----------|
| Think | `#think` | Idea generation, research, feasibility analysis. |
| Plan | `#plan` | Task decomposition, prioritization, execution planning. |
| Build | `#build` | Development, coding, implementation, PR tracking. |
| Audit | `#audit` | Review, validation, HITL checks, approval gates. |
| Ops | `#ops` | Deployment logs, monitoring, post-mortems. |

Additional service channels:
- `#announcements` — Project updates, releases, public news.
- `#rfc` — Proposals and process evolution.
- `#random` — Non-work communication.

---

## 🤖 Agent Communication Model

### Dual-Layer Answering System
Agents use a two-layer response format:

**1️⃣ Executive Summary (public):**
- Posted directly in the thread where the question originated (e.g. `#think`).
- Concise, 3–5 points.
- Accessible to all participants.

**2️⃣ Detailed Explanation (private):**
- Sent in a direct message (DM) between initiator and agent.
- Contains reasoning, data, alternatives, risks, and attachments.
- Logged in TEAMS trace as `private_interaction`.

---

## 💬 Private Clarification Flow

All discussions, disagreements, or detailed refinements occur **privately** in DM between the initiator and the responsible agent.

- The public thread remains clean and contains only validated conclusions.
- DM interactions are automatically attached to task trace for auditability.
- When consensus is reached, the initiator simply confirms with a short reply such as `ok`, `👍`, or `✅`.

---

## ⚙️ Push-Confirm Mechanism

### Sequence
1. Agent completes analysis → asks initiator in DM: *“Ready to push the update to thread?”*
2. Initiator replies with any positive confirmation (`ok`, `👍`, `✅`, `да`, `пушь`).
3. TEAMS Bot detects confirmation → triggers event `push_approved`.
4. Bot posts updated **Executive Summary** to the corresponding thread in Slack.
5. Bot adds trace entry and notifies the project channel participants.

### Example
**DM:**
> Agent: All clarifications applied, ready to push?
>
> You: 👍

**Then in `#think`:**
> 🧩 *Update v1.4 (approved by @User)*  
> — Exchange X → risk class B  
> — Exchange Y → reserved for v2  
> — Ready for planning phase  
> _trace: think_2026_004_v1.4_

---

## 🧠 Trace Example
```yaml
task_id: think_2026_004
stage: think
summary_version: 1.4
private_interactions:
  participants: [@User, @Olya]
  messages: 28
  duration: 2d
agreement:
  confirmed_by: [@User]
  date: 2026-01-07T15:30:00Z
  outcome: ready_for_plan
