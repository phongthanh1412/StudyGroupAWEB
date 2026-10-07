# AI-LOG.md — CSC13114 · StudyGroup Team

**Repository:** `https://github.com/phongthanh1412/StudyGroupAWEB`
**Team:** Phong · An · Toàn

> One entry per task where an assistant did part of the work.
> Heavy use is declared honestly. Anything we cannot explain at the oral scores zero.

---


## 2026-10-07 — PA#1 · LLM feature and cost-of-being-wrong analysis (§1)

**Tool:** Antigravity IDE Assistant  
**Asked for:** Describe the Meeting-to-Task Extractor feature and quantify what goes wrong when the LLM makes a mistake.  
**Kept:** The three output categories (To-do / Owner / Deadline) and the 48-hour draft expiry mechanism.  
**Changed:** The AI initially said "the group loses marks" in general terms. Rewrote the cost section to be specific: mis-assigned task → person receives no reminder → blank section at submission → shared milestone loss of up to 20% of course grade, irreversible after deadline.  
**Rejected:** A version that described the confirmation gate as optional. The gate must be mandatory and structurally enforced (data never written to DB without it), not just a UX suggestion.  
**By hand:** The core safety mechanism concept (a hard confirmation gate before any DB write or push notification), and the framing that the LLM is a "time-saving first draft, not an autonomous decision-maker."

---

## 2026-10-07 — PA#1 · Scope list, plan table, and risks (§2-4)

**Tool:** Antigravity IDE Assistant  
**Asked for:** Generate the in/out-of-scope lists, the 6-checkpoint plan table, and two concrete risks with mitigations.  
**Kept:** The overall structure of the plan table (Checkpoint / Deliverable / Owner / Date) and the two risk categories (LLM mis-assignment; no schedule overlap found).  
**Changed:**
- Moved all internal deadlines from the AI's original Thursday dates to Mondays (Oct 19, Nov 2, Nov 16, Nov 30, Dec 14, Jan 4) to leave a buffer before the weekly class.
- Updated all owner names from the AI's defaults (Minh / Thành / Linh) to our actual team (Phong / An / Toàn) and corrected role assignments.
- Raised the N−1 fallback threshold idea (already in our notes) into the formal Risk 2 mitigation.  

**Rejected:** A version of the risk list that included "time pressure" and "scope creep" — these are too generic. Replaced with two project-specific failure modes that a reader could actually act on this week.  
**By hand:** The out-of-scope decisions (no Google Calendar sync, no video calls, no contribution scoring) — these came directly from the course brief and our own team agreement.

---

## 2026-10-07 — PA#1 · Technology choices (§5)

**Tool:** Antigravity IDE Assistant  
**Asked for:** Recommend a technology stack with per-choice justification for this specific project.  
**Kept:** React + Vite (frontend), Node.js + Express (backend), Socket.io (real-time), OpenAI GPT-4o-mini (LLM) with cost estimate.  
**Changed:**
- Swapped **Supabase → Neon**: After the AI explained both options, we decided Neon's database branching is more valuable for parallel development across 3 team members than Supabase's all-in-one approach (which would conflict with our own Express layer).
- Swapped **Cloudinary → Cloudflare R2**: Zero egress fees and S3-compatible API fit our backend-controlled upload flow better than Cloudinary's media-focused SDK.
- Added **Redis** (for Interval Intersection caching and Socket.io Pub/Sub adapter) and **RabbitMQ** (for async LLM processing queue) after evaluating which features would suffer without them.  

**Rejected:** Supabase as primary database — the AI's first suggestion — because using Supabase's BaaS features alongside a full Express backend would duplicate Auth and Storage logic and add unnecessary complexity.  
**By hand:** The decision to include Redis and RabbitMQ (prompted by our own architectural thinking about the LLM timeout problem), and the final evaluation of Neon vs Supabase trade-offs.

---

## 2026-10-07 — PA#1 · SELF_ASSESSMENT_REPORT.md

**Tool:** Antigravity IDE Assistant  
**Asked for:** Generate the self-assessment table with evidence lines for each rubric criterion.  
**Kept:** The table structure and the criterion-by-criterion scoring rationale.  
**Changed:** Updated all evidence lines to reference the final section numbers and actual content of the revised proposal (e.g., updated names from Minh → Phong, updated date range Oct 19–Jan 4, updated tech count from 7 → 9).  
**Rejected:** An initial version that claimed 20/20 on Problem and Users — after re-reading the rubric, we self-assessed 19/20 because we do not have primary user research data (survey or interview), only a representative persona.  
**By hand:** The "What I Did Not Manage" section — written to honestly reflect the missing Gantt chart and missing early LLM prompt draft.

---

*Last updated: 2026-10-07. Next entry expected at IA#1 (Oct 19).*
