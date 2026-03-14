# 🎓 EduFlow AI
### *The Autonomous Learning Agent That Teaches, Tests & Adapts*

> "An autonomous AI agent that builds your personal learning path, teaches you, quizzes you, and adapts in real-time — like having a world-class tutor available 24/7."

Built for: Education × AI/ML Hackathon | Stack: Gemini API + Firebase + FAISS

---

## 🎯 The Problem

In a classroom of 40+ students, personalized teaching is impossible. Every student learns differently, struggles with different concepts, and needs different pacing — yet they all get the same curriculum delivered at the same speed.

EduFlow AI solves this It gives every student their own autonomous AI tutor that knows exactly where they struggle, adapts in real-time, and never stops learning about the learner.

| Metric | Without EduFlow | With EduFlow |
|---|---|---|
| Personalization | One-size-fits-all | 100% adaptive per student |
| Feedback speed | Days (manual grading) | Instant, after every answer |
| Availability | 8 hrs/day, weekdays | 24/7, any device |
| Progress tracking | Quarterly, manual | Real-time, per concept |

---

## 🏗️ Architecture

EduFlow uses a **5-layer agentic architecture** — not a chatbot, a fully autonomous agent:

```
Student Goal Input (React UI)
        ↓
FastAPI Backend → Firebase (session created)
        ↓
PLANNER AGENT (Gemini) → JSON Curriculum DAG
        ↓
        ORCHESTRATOR LOOP
        ├─ TEACHING AGENT (Gemini)   → Explain concept
        ├─ RESOURCE AGENT (Tavily)   → Fetch videos/docs
        ├─ QUIZ AGENT (Gemini)       → Generate questions
        └─ MEMORY MANAGER (FAISS)    → Store results
        ↓
PERFORMANCE ANALYZER (Gemini) → Score + identify gaps
        ↓
RE-PLANNER (Gemini) → Update curriculum if score < 70%
        ↓
React Dashboard → Progress, scores, next steps
```

### What Makes This *Agentic*

A chatbot answers one question. EduFlow autonomously:
- **Decomposes** a learning goal into 10+ ordered sub-tasks (curriculum DAG)
- **Selects tools** — teaching, search, quiz, or re-planning — based on context
- **Maintains persistent memory** across an entire session using a 4-layer memory system
- **Re-plans autonomously** when a student fails — zero human intervention required

---

## 🧠 Memory System

| Memory Type | Technology | What's Stored | When Accessed |
|---|---|---|---|
| Short-term | Gemini's context window | Current module, recent answers | Every Gemini call |
| Long-term | FAISS Vector Store | All past quiz results + weak concepts | Before each teaching step |
| Episodic | Firebase Firestore | Full session history, scores | Dashboard & re-planning |
| Working | Python dict (in-memory) | Current task state, active module | During orchestrator loop |

---

## ⚙️ How It Works

**Phase 1 — Onboarding:** Student enters topic, skill level, and goal. Gemini generates a baseline assessment quiz to determine their exact starting point.

**Phase 2 — Curriculum Planning:** Gemini's Planner Agent creates a Directed Acyclic Graph (DAG) of learning modules — ordered by prerequisites, with parallel modules where possible.

**Phase 3 — Teaching Loop:** For each module, the agent retrieves the student's weak areas from FAISS, explains the concept, fetches real learning resources via Tavily Search, then generates an adaptive quiz. If the student scores below 70%, a remedial module is automatically inserted.

**Phase 4 — Memory & Adaptation:** Quiz results are embedded into FAISS vector memory, enabling increasingly personalized teaching as the session progresses.

**Phase 5 — Final Report:** Gemini synthesizes the full session into a personalized progress report with mastered concepts, weak areas, time spent, and recommended next steps.

---

## 🛠️ Tech Stack

| Technology | Role |
|---|---|
| **Gemini API (Google)** | Planner, Teacher, Quiz Generator, Re-Planner, Synthesizer |
| **Google Firebase** | Firestore (session DB), Realtime DB (live updates), Auth, Storage |
| **FAISS (Facebook AI)** | Long-term vector memory for student weaknesses |
| **sentence-transformers** | Embedding quiz results and concepts into vectors |
| **Tavily Search API** | Finding the best learning resources per concept |
| **FastAPI + Uvicorn** | REST API server for agent orchestration |
| **React + TailwindCSS** | Student dashboard, quiz UI, progress tracker |

---

## 🚀 Getting Started

### Prerequisites
```bash
pip install google-generativeai firebase-admin faiss-cpu sentence-transformers fastapi uvicorn tavily-python
```

### Environment Variables
```
GEMINI_API_KEY=AIza...
FIREBASE_SERVICE_ACCOUNT=serviceAccount.json
TAVILY_API_KEY=tvly-...
```

### Run the Backend
```bash
uvicorn main:app --reload --port 8000
```

### API
```
POST /start-session   → { topic, skill_level, hours_available }
GET  /session/{id}    → Full session history and progress
```

---

## 📁 Project Structure

```
eduflow-ai/
├── main.py                  # FastAPI app + session endpoints
├── planner.py               # Curriculum DAG generation (Gemini)
├── orchestrator.py          # Main agent loop
├── memory_manager.py        # FAISS store + retrieve
├── re_planner.py            # Adaptive re-planning logic
├── tools/
│   ├── teaching_agent.py    # Concept explanation (Gemini)
│   ├── quiz_agent.py        # Adaptive quiz generation (Gemini + function calling)
│   ├── resource_finder.py   # Learning resource search (Tavily)
│   └── performance_analyzer.py  # Score analysis + gap detection
├── firebase_utils.py        # Firebase read/write helpers
└── frontend/                # React dashboard
```

---

 Key Agent Behaviors

Function Calling for Structured Output — The Quiz Agent uses Gemini's function calling to guarantee structured JSON quiz output with questions, options, correct answers, and concept tags.

RAG-Powered Teaching — Before every lesson, the agent retrieves semantically similar past mistakes from FAISS, so Gemini's explanation directly addresses what this specific student struggles with.

Autonomous Re-Planning — The Re-Planner is triggered automatically when a quiz score falls below 70%, inserting a remedial module into the DAG without any human intervention.

Hallucination Mitigation — Every teaching step is grounded in retrieved student context. Resource Agent fetches real URLs. Quiz scoring uses exact-match. All JSON outputs are schema-validated.

---

## 👥 Team

Built in 24 hours at NeuraX 2.0 | Domain: AI & ML

---
