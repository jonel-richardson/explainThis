# explainThis

> AI-powered jargon translator that explains any technical term at five complexity levels — from "explain it like I'm five" to deep technical precision.

**Live:** [explainThis on Netlify](https://explainthis.netlify.app)

---

## The Problem

Jargon is the single biggest barrier to entry in tech. Career changers, bootcamp students, and non-technical stakeholders hit a wall when every "beginner" explanation uses five more terms they don't know. Existing resources like Stack Overflow and MDN assume prior knowledge, creating a cycle where understanding jargon requires... more jargon. This locks out the very people trying to break into the industry.

## What It Does

Paste any technical term or confusing snippet — "What is a RESTful API?" or "middleware" — and get a clear, structured explanation tailored to your current understanding. Choose from five complexity levels ranging from everyday analogies (Beginner) to architectural trade-offs (Expert), so the same concept grows with you as you learn.

## Key Features

| Feature | What It Does |
|---------|-------------|
| 5-Level Explanations | Choose from Beginner, Elementary, Intermediate, Advanced, or Expert — each with distinct vocabulary, depth, and examples |
| Structured Responses | Every explanation includes a plain-language summary, real-world analogy, code example, and "why it matters" context |
| Explore Library | Browse 48 curated concepts across 6 categories (Frontend, Backend, Databases, DevOps, CS Fundamentals, Web Concepts) |
| Save & Search | Bookmark explanations to a personal library with full-text search, sorting, and bulk management |
| Progress Tracking | Visual dashboard showing concepts explored, favorite levels, and learning patterns over time |
| Related Concepts | Each explanation surfaces 3-5 related terms as clickable pills, turning one lookup into a learning path |
| Dark Mode | Full dark/light theme with OS preference detection and persistence |
| Privacy-First | All data stays in your browser via localStorage — no accounts, no tracking, no server-side storage |

## Tech Stack

| Layer | Technology | Why |
|-------|-----------|-----|
| Frontend | React 19 | Component model maps cleanly to the multi-section explanation cards; hooks handle API state without a state library |
| Build Tool | Vite 7 | Sub-second HMR during development and optimized production builds — no Webpack configuration overhead |
| Styling | Tailwind CSS 3 | Utility classes enable rapid prototyping of the level-colored UI system without writing custom CSS for every variant |
| AI | Google Gemini 2.0 Flash | Structured prompt engineering produces consistent 5-section responses; Gemini's speed and free tier make it ideal for a learning tool with frequent API calls |
| Icons | Lucide React | Tree-shakeable SVG icons that match the minimal UI aesthetic |
| Deployment | Netlify | Zero-config deploys from Git with environment variable management for the API key |

## Architecture

```
┌──────────┐     ┌─────────────────────────────────┐     ┌─────────────┐
│          │     │           Frontend (React)        │     │             │
│   User   │────>│                                   │────>│  Gemini API │
│          │     │  TextInput ─> LevelSelector       │     │             │
└──────────┘     │       ─> ExplainButton            │     │  Generates  │
                 │                                   │<────│  structured │
                 │  ExplanationCard (5 sections)     │     │  5-section  │
                 │  Library / Explore / Progress     │     │  response   │
                 │                                   │     └─────────────┘
                 │        localStorage               │
                 │  (saved explanations, prefs)       │
                 └─────────────────────────────────┘
```

```
User Input ──> Extract Technical Term ──> Generate Level-Specific Prompt
                                                    │
                                                    v
                                              Gemini API
                                                    │
                                                    v
                                          Parse Structured Response
                                                    │
                                                    v
                                    ┌───────────────────────────────┐
                                    │  Simple Explanation           │
                                    │  Analogy                      │
                                    │  Real-World Example           │
                                    │  Why It Matters               │
                                    │  Related Concepts (clickable) │
                                    └───────────────────────────────┘
```

## Technical Decisions

**Client-side only architecture:** No backend server, API calls go directly from the browser
**Why:** A backend would add deployment complexity, cost, and latency for a tool that has no user accounts or shared data. localStorage handles persistence, and the API key is scoped to this project. For a portfolio-scale app, the simplicity tradeoff is worth it — every piece of infrastructure you don't run is a piece that can't break.

**Structured prompt engineering over free-form responses:** Every prompt enforces a 5-section markdown format (`## SIMPLE EXPLANATION`, `## ANALOGY`, etc.)
**Why:** Free-form AI responses vary wildly in structure, making them impossible to render in a consistent UI. By constraining the output format in the prompt, the app can regex-parse sections into discrete React components. This trades some response flexibility for a predictable, scannable layout that users can rely on.

**localStorage over a database:** All saved explanations and preferences persist in the browser
**Why:** The target users are learners exploring concepts — they don't need cross-device sync or collaboration. localStorage is instant, requires zero authentication, and reinforces the privacy promise (no data leaves the browser). The tradeoff is that clearing browser data erases the library, but for a learning tool this is acceptable.

## Getting Started

### Prerequisites
- Node.js 18+
- A Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey)

### Installation
```bash
git clone https://github.com/jonelrichardson-spec/explainThis.git
cd explainThis/explainThis
npm install
```

### Environment Variables
Create a `.env` file in the project root:
```
VITE_GEMINI_API_KEY=your_gemini_api_key_here
```

### Run Locally
```bash
npm run dev
# Open http://localhost:5173
```

### Build for Production
```bash
npm run build
npm run preview
```

## What I'd Build Next

- **Conversation mode** — let users ask follow-up questions about an explanation without re-entering the concept, turning a single lookup into a back-and-forth tutoring session
- **Spaced repetition** — surface previously saved concepts at increasing intervals, using the progress data already being tracked to build a lightweight review system
- **Share as image** — export a single explanation card as a styled PNG for sharing on social media or in Slack, turning users into distribution

## About This Project

Built as a solo project for the **Pursuit Fellowship** (Full Stack Web Development), showcased at **Demo Day in October 2025**. The idea came from my own experience — after 8 years teaching preschool in Japan, I transitioned into tech and found that every "beginner-friendly" explanation assumed knowledge I didn't have yet. explainThis is the tool I wished existed on day one.

**My role:** Solo developer — designed the UI, engineered the prompts, built all components, and deployed to production.

---

Built by [Jonel Richardson](https://linkedin.com/in/jonel-richardson-09a399382)
