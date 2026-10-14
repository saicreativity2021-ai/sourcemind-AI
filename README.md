# SourceMind AI — Hackathon Prototype

A responsive frontend prototype for **SourceMind AI**, a RAG-powered source-grounded tutor and knowledge-tracing concept.

## What's included

- Overview dashboard with learning metrics and recommended topics
- AI Tutor chat with sample answers, visible source-card UI, and clear demo disclaimers
- Source library UI with local file selection / drag-and-drop listing and search
- Sample quiz with scoring and explanations
- Knowledge map with topic mastery estimates
- Responsive sidebar for mobile
- Backend integration scaffold in `backend/`

## Important: what is real vs simulated?

This is a **frontend prototype**. Chat answers come from a small, hard-coded sample knowledge library. Uploading a file only adds its name to the browser UI; it does not parse/index the document or send it to an AI model. The displayed citations are demonstration UI, not citations verified against uploaded files. The mastery numbers are sample values.

To make it a real RAG application, connect the frontend to a backend that:
1. extracts text from uploaded PDFs and documents,
2. chunks text and stores metadata (document name, page, chunk ID),
3. creates embeddings and indexes chunks in a vector store,
4. retrieves relevant chunks for each question,
5. asks an LLM to answer using only retrieved context,
6. returns passage-level citations, and
7. stores quiz attempts / mastery updates per learner.

## Run locally

You can open `index.html` directly in a modern browser. For a local static server, from this folder run:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`.

## Publish on GitHub Pages

1. Create a **public** GitHub repository named `SourceMind-AI`.
2. Upload `index.html`, `styles.css`, `app.js`, and `README.md` to the repository root. You can also upload the `backend/` folder.
3. Open **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Select branch `main` and folder `/ (root)`, then Save.
6. Wait for the published URL shown in the Pages section.

The static frontend can be hosted on GitHub Pages. The Python backend cannot run on GitHub Pages; deploy it separately to a backend host and set the API base URL in the frontend when integrating it.

## Suggested hackathon architecture

```text
Browser (GitHub Pages)
  ├── Dashboard / Tutor / Sources / Quiz / Knowledge Map
  └── HTTPS API calls
         ↓
FastAPI backend (deploy separately)
  ├── POST /api/upload  → extract text + chunk + index
  ├── POST /api/chat    → retrieve passages + generate cited answer
  ├── GET  /api/sources → list indexed documents
  └── POST /api/quiz/attempt → save answer + update mastery
         ↓
Vector index + document metadata + learner progress store
```

## Backend scaffold

`backend/main.py` contains API endpoint shapes and TODOs. It deliberately does not pretend to perform actual retrieval or model generation. Add a chosen LLM provider, PDF parser, embedding model, vector database, authentication, and persistent storage before using real student documents.
