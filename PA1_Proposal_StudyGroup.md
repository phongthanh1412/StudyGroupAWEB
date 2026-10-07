# PA#1 — Proposal: StudyGroup Scheduler & Resource Hub

## 1. Problem and Users

**User:** Phong, a final-year Software Engineer student at HCMUS, is the leader of a 5-person project group for their Software Engineering capstone. Every semester, his group relies on a Zalo group chat to coordinate: they spam availability polls, share Google Drive links, and paste meeting notes — all in the same thread. Within days the chat is a wall of noise. Documents scroll out of reach, no one agrees on a meeting time, and assignment ownership is never written down clearly.

**Problem in one sentence:** Study groups cannot find a shared free slot to meet, and every file and task decision gets buried in chat history before anyone acts on it.

**What they do today:** Group leader manually DMs each member, collects free times via a screenshot of a hand-drawn table, picks the "best" slot by eye, announces it in chat (which half the group misses), and re-sends shared files every time someone asks "where's the doc?".

---

## 2. The LLM Feature — and the Cost of Being Wrong

**Feature:** *Meeting-to-Task Extractor.* After a group meeting, the leader pastes the raw meeting transcript (exported from Zalo, Google Docs notes, or typed minutes) into the app. An LLM (GPT-4o-mini via OpenAI API) reads the transcript and returns a structured summary with three sections:

1. **To-do list** — concrete action items extracted from the discussion.
2. **Owner** — which member's name appears closest to each action item.
3. **Deadline** — the date or relative phrase mentioned ("by Friday", "before Week 10").

**Cost of being wrong — concretely:**
- *Who is harmed:* Every member of the group, but especially the person incorrectly assigned a task.
- *How much:* If the LLM assigns "write the report introduction" to Lan instead of Minh (who actually volunteered), Minh never sees a reminder, does nothing, and the introduction is blank on submission day. The group loses points on a graded milestone worth up to 20% of their semester grade — a shared, irreversible consequence.
- *Can it be undone:* Only if caught before the deadline. After submission, no.

**Mitigation baked into the product:** The LLM output is shown in a *draft confirmation screen*. The group leader must review every row and press **"Confirm & Notify"** before any task is written to the database or any push notification is sent to members. Unconfirmed drafts expire after 48 hours and are never stored in the official task table. This turns the LLM into a time-saving first draft, not an autonomous decision-maker.

---

## 3. Scope — In and Out

**In scope this semester:**

- Internal scheduling: members submit weekly availability blocks; the app runs an **Interval Intersection** algorithm across all N members and surfaces the top-3 overlapping slots ranked by duration and number of attendees.
- Meeting creation with push notifications via **WebSockets** (Socket.io) so all members see new meetings instantly.
- File repository: upload documents (PDF, DOCX, images) to **Cloudflare R2** via S3-compatible API; files are organized in a two-level folder tree (Group → Topic).
- LLM Meeting-to-Task Extractor with mandatory leader confirmation gate (described above).
- Mobile-responsive web app; no native mobile app.

**Explicitly out of scope:**

- Google Calendar / Apple Calendar sync — internal calendar only.
- Video or audio calls inside the app.
- Automated grading or contribution scoring per member.
- Real-time collaborative document editing (Google Docs-style).
- Payment or subscription features.

---

## 4. Plan and Ownership

| Checkpoint | Deliverable | Owner | Date |
|---|---|---|---|
| **IA#1** | Architecture diagram, DB schema, API contract, wireframes | Phong (BE) | Oct 19 |
| **IA#2** | Auth + group management + availability input UI | An (FE) + An (BE) | Nov 2 |
| **IA#3** | Interval Intersection engine + meeting creation + WebSocket notifications | Phong (BE) | Nov 16 |
| **IA#4** | File upload (Cloudflare R2) + folder hierarchy + file listing UI | Toàn (FE) | Nov 30 |
| **IA#5** | LLM extractor + confirmation gate + task board display | Toàn (AI/FE) | Dec 14 |
| **Final** | Integration testing, bug fixes, demo recording, final report | All | Jan 4 |

*Team:* An (frontend/backend), Phong (backend/infra), Toàn (LLM integration + frontend support). Each member owns their checkpoint; cross-review happens in a weekly 1-hour call every Sunday.

---

## 5. Risks

**Risk 1 — LLM extracts wrong owner or wrong deadline from ambiguous transcript.**
*Impact:* Tasks mis-assigned; missed deadlines affect the whole group's grade.
*Mitigation (start this week):* Design the confirmation gate UI first, before writing any LLM code, so it is structurally impossible to skip. Also write 10 representative transcript fixtures (clear, ambiguous, mixed-language) and run them against the prompt before IA#5; iterate on the system prompt until precision on owner extraction exceeds 80% on the fixture set.

**Risk 2 — Interval Intersection returns no overlap when the group has highly fragmented schedules.**
*Impact:* The core scheduling value proposition fails; users see an empty result and lose trust in the app.
*Mitigation (start this week):* Implement a fallback that relaxes the constraint from "all N members free" to "at least N−1 members free" and shows which member is excluded. Prototype this logic in a standalone script by IA#2 using synthetic schedule data.

---

## 6. Technology Choices

| Layer | Choice | Justification |
|---|---|---|
| **Frontend** | React + Vite | Fast HMR for iterative UI work; both An and Toàn already know React, saving ramp-up time. |
| **Backend** | Node.js + Express | Lightweight, same language as frontend, easy WebSocket integration with Socket.io. |
| **Database** | PostgreSQL (Neon free tier) | Relational model fits availability blocks (time ranges) and task assignment tables; Neon's database branching feature allows safe parallel development across the 3 team members. |
| **Cache/State**| Redis | Caches the computationally heavy "Interval Intersection" outputs; serves as Pub/Sub adapter for scaling WebSockets across backend nodes. |
| **Msg Broker** | RabbitMQ | Handles asynchronous LLM processing queues (to prevent HTTP timeouts when waiting 10-30s for the OpenAI API) and scheduled reminders. |
| **Real-time** | Socket.io | Mature library; handles reconnection and rooms — maps directly to "group" entities in the app. |
| **File storage** | Cloudflare R2 (free tier) | Zero egress fees and an S3-compatible API, making it easy to store and retrieve team documents cost-effectively. |
| **LLM** | OpenAI GPT-4o-mini | ~$0.15 / 1M input tokens — a 2,000-word meeting transcript costs < $0.001 per call, affordable for demo scale. Structured output (JSON mode) reduces post-processing errors. |
| **Deployment** | Vercel (FE) + Render (BE free tier) | Zero-config deployment; both have generous free tiers sufficient for course demo load. |

---

*Repository:* `https://github.com/phongthanh1412/StudyGroupAWEB`

