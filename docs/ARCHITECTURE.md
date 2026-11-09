# Sanchaya (सञ्चय) System Architecture

## 1. End-to-End Data Flow
1. **Connect (Stage 1)**: Authorised ingestion of GitHub repository data (commits, pull requests, issues), meeting notes (PDF, DOCX, TXT), and task records (CSV) via Node.js Express API gateway[cite: 4, 6]. Each input is normalised into a timestamped source record (`source_id`)[cite: 4].
2. **Security & Redaction**: Deterministic regex secret and PII scanning masks API keys, tokens, passwords, and sensitive emails before any content reaches the AI model[cite: 5, 6, 9].
3. **Reconstruct (Stage 2)**: Python FastAPI backend uses strict JSON schema LLM prompts to extract chronological decisions, task statuses, owners, dependencies, and blockers. In parallel, rule-based Git history analysis computes the Contribution Concentration Risk Map.
4. **Verify (Stage 3)**: Deterministic evidence validator cross-checks every AI-generated source reference and verbatim excerpt against MongoDB, checks freshness timestamps, and assigns one of four states: **Supported**, **Needs review**, **Conflicting**, or **Unknown**.
5. **Review & Handover (Stages 4 & 5)**: Transparent 7-point Handover Readiness Checklist evaluation, Gap-to-Question loop for outgoing members, role-scoped access control, and ReportLab PDF export[cite: 3, 5, 6, 7].

---

## 2. Module Breakdown & Ownership
| Module Path / Component | Core Responsibility | Owner |
| :--- | :--- | :--- |
| `frontend/` | React + Tailwind CSS v4 UI supporting role-scoped views (Team Leader, Outgoing Member, Incoming Member)[cite: 5, 7, 8]. | Lokesh Pawar (Leader)[cite: 11] |
| `backend-node/src/` | Express API gateway, JWT role auth middleware, GitHub REST connector, document/CSV normalization, and pre-AI secret scanner. | Ayush Nirmal[cite: 11] |
| `backend-python/app/` | FastAPI AI extraction engine, 4-state evidence validator with freshness monitoring, temporal conflict detection, and Gap-to-Question generation[cite: 7, 12]. | Varad Gaikwad[cite: 12] |
| `backend-python/app/` (Analytics) | Rule-based Git commit concentration risk analyzer, 7-point Readiness Checklist engine, version logging, and ReportLab PDF export[cite: 7, 13]. | Prakhar Raj[cite: 13] |