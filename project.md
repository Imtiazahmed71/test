# StudyMind — AI-Powered Personal Study Assistant

> An intelligent assistant that helps students plan study sessions, summarize learning material, quiz them adaptively, and track progress over time — all through a conversational chat interface.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | React + TypeScript |
| AI | Claude API |
| Backend | Node.js / Express |
| Database | PostgreSQL |
| Vector Store | Pinecone (RAG) |
| File Parsing | PDF parsing library |
| File Storage | S3 or equivalent |

---

## Requirements Analysis

### Functional Requirements

- Upload study material (PDF, text, URLs)
- Auto-generate summaries & key concepts
- Adaptive quiz engine (MCQ, fill-in, short answer)
- AI chat for Q&A on uploaded content
- Study session planner with time blocks
- Spaced repetition reminders
- Progress dashboard per subject

### Non-Functional Requirements

- Response latency < 2s (streaming preferred)
- Supports documents up to 50MB
- 99.5% uptime SLA
- FERPA / data privacy compliance
- Mobile-responsive UI
- Multi-language support *(Phase 2)*
- Offline mode for saved notes *(Phase 2)*

### Technical Requirements

- RAG pipeline for document Q&A
- Persistent conversation memory per user
- Auth (OAuth2 / email-password)
- File storage (S3 or equivalent)
- Background jobs for PDF parsing
- Rate limiting per user tier
- Token usage tracking per session

---

## Priority Analysis (MoSCoW)

### 🔴 Must Have

| Feature | Effort | Rationale |
|---|---|---|
| AI Chat (Q&A on material) | Medium | Core value proposition; drives all other features |
| Document upload & parsing | High | Needed to feed content into the AI pipeline |
| Auto-summarization | Low | High ROI — easy win once ingestion works |

### 🟠 Should Have

| Feature | Effort | Rationale |
|---|---|---|
| Adaptive quiz engine | High | Differentiator; requires scoring + feedback loop logic |
| Progress dashboard | Medium | Improves retention; needs data from quizzes first |
| Study session planner | Low | Adds habit-building; relatively simple to implement |

### 🔵 Could Have

| Feature | Effort | Rationale |
|---|---|---|
| Spaced repetition alerts | Medium | Nice UX boost; depends on quiz history data |

### ⚪ Won't Have (v1)

| Feature | Effort | Rationale |
|---|---|---|
| Multi-language support | Very High | Scope risk; defer to post-launch iteration |
| Offline mode | Very High | Complex PWA + sync; not core to v1 value |

---

## Build Phases

### Phase 1 — MVP (Weeks 1–4)

- User authentication (OAuth2 / email-password)
- Document upload & storage
- PDF parsing pipeline
- Auto-summarization
- Basic AI chat interface

**Goal:** A working product where a user can upload a document and have a conversation about it.

---

### Phase 2 — Core UX (Weeks 5–8)

- Adaptive quiz engine (MCQ, fill-in, short answer)
- Quiz scoring & feedback loop
- Persistent conversation memory per user
- Basic progress dashboard

**Goal:** Turn passive reading into active learning with measurable outcomes.

---

### Phase 3 — Engagement (Weeks 9–12)

- Study session planner with time blocks
- Spaced repetition reminder system
- Advanced progress analytics
- UI polish & performance optimization

**Goal:** Drive long-term habit formation and retention.

---

## Architecture Overview

```
User
 │
 ▼
React Frontend
 │
 ▼
Node.js / Express API
 ├── Auth Service
 ├── File Upload → S3
 ├── Background Job (PDF Parser)
 │        │
 │        ▼
 │   Vector DB (Pinecone)
 │        │
 ├── RAG Pipeline ◄──────────┐
 │        │                  │
 │        ▼                  │
 └── Claude API          PostgreSQL
      (Chat / Quiz /     (Users, Sessions,
       Summaries)         Progress, Notes)
```

---

## Key Design Decisions

1. **RAG over fine-tuning** — Document Q&A is handled via retrieval-augmented generation, keeping costs low and content always up-to-date without retraining.
2. **Streaming responses** — Claude API streaming is used for chat to keep perceived latency under 2 seconds even for long answers.
3. **Adaptive quizzes** — Quiz difficulty adjusts based on past performance stored in PostgreSQL, not just random generation.
4. **Phase-gated scope** — Each phase depends on the previous one being stable. No phase 2 features are started until phase 1 is complete and tested.

---

## Risk Register

| Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|
| PDF parsing edge cases | High | Medium | Use battle-tested library; add manual fallback |
| RAG retrieval quality | Medium | High | Tune chunk size & overlap; add re-ranking step |
| API cost overrun | Medium | High | Token tracking + per-user rate limiting from day 1 |
| Scope creep | High | Medium | Strict MoSCoW enforcement; defer to backlog |

---