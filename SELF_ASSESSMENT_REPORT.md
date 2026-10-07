# SELF_ASSESSMENT_REPORT.md

**Project:** StudyGroup — Coordinate group schedules and share learning resources  
**Course:** CSC13114  
**Team:** `<23120017>-<23120096>-<23120154>`   

---

## Self-Assessment Table

| # | Criterion | Max | Claimed | Evidence |
|---|---|---|---|---|
| 1 | Problem and users | 20 | **19** | `PA1_Proposal_StudyGroup.md` §1 — named user (Phong, CS student at HCMUS), named situation (capstone group coordinator), problem in one sentence, plus what-they-do-today description. |
| 2 | LLM feature and cost of being wrong | 25 | **23** | `PA1_Proposal_StudyGroup.md` §2 — one feature (Meeting-to-Task Extractor), specific wrong-answer cost (mis-assigned task → blank submission → shared grade loss, irreversible), reversibility analysis, and product-level mitigation (mandatory confirmation gate; draft expires in 48 h if not confirmed). |
| 3 | Scope: in and out | 15 | **14** | `PA1_Proposal_StudyGroup.md` §3 — 5 explicit in-scope items, 5 explicit out-of-scope items; out-scope list is consistent with §4 plan (no Google Calendar checkpoint appears). |
| 4 | Plan and ownership | 20 | **18** | `PA1_Proposal_StudyGroup.md` §4 — table with 6 checkpoints (IA#1 through Final), each with deliverable, named owner, and date; internal dates span Oct 19 – Jan 4, leaving a safe buffer before Thursday classes. |
| 5 | Risks | 10 | **9** | `PA1_Proposal_StudyGroup.md` §5 — 2 project-sinking risks (LLM mis-assignment; no schedule overlap found), each with an actionable mitigation startable this week (confirmation gate UI first; fallback relaxation + fixture testing). |
| 6 | Technology choices | 10 | **9** | `PA1_Proposal_StudyGroup.md` §6 — 9 choices with per-choice justification tied to project; includes LLM provider (OpenAI GPT-4o-mini) with concrete cost estimate (< $0.001 per transcript). |
| | **Total** | **100** | **92** | |

---

## What I Did Not Manage

- The plan lists milestone dates but does not break each checkpoint into sub-tasks with hour estimates; a Gantt chart would make ownership more granular.
- The LLM system prompt / few-shot examples are not drafted in this proposal — deferred to IA#5. An early draft prompt would strengthen §2.
- No primary user research data (interviews or surveys) was included; the proposal relies on a representative persona rather than measured evidence.

---

