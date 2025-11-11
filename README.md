
#LexiLight
LexiLight turns dense legal PDFs into plain-language guidance with a local retrieval pipeline. Upload contracts or terms of service, extract embeddings, and review actionable summaries with grounded citations before chatting through open questions.

Prerequisites
Node.js 20+
pnpm 8+
Python 3.11+
uv (recommended) or pip
OpenAI API key with access to the configured chat/embedding models
Quickstart
cp .env.example .env
# add your OpenAI credentials

pnpm install
pnpm --filter shared build  # optional: compile shared package for reuse

# Terminal 1 – frontend
pnpm --filter web dev

# Terminal 2 – backend
uvicorn api.main:app --reload
The app runs at http://localhost:3000 and calls the FastAPI backend at http://localhost:8000.

Environment files
Root .env controls API credentials (OPENAI_API_KEY, EMBED_MODEL, CHAT_MODEL).
web/.env.local offers NEXT_PUBLIC_API_URL for overriding the backend URL.
api/.env mirrors model settings for the FastAPI service.
Seed data
./scripts/seed.sh
Copies the sample public-domain PDFs into web/public/seed_docs for quick demos.

Available scripts
pnpm dev:web – run Next.js in dev mode (proxy commands in root Makefile as make web).
pnpm dev:api – convenience alias for uvicorn api.main:app --reload.
pnpm test – runs Playwright smoke tests for the frontend UI.
make api / make web / make dev – shortcuts for local workflows.
Testing & Quality
Frontend: Playwright smoke flows (pnpm --filter web test). ESLint (Next config) and Prettier.
Backend: Pytest unit tests (pytest within api). Ruff + Black configs defined in api/pyproject.toml.
Shared types: Zod schemas mirrored in FastAPI models to guarantee parity between JSON responses and React UI.
Project Layout
lexilight/
├── web/           # Next.js 14 app (Tailwind, shadcn/ui components, react-query)
├── api/           # FastAPI service with LangChain RAG pipeline
├── shared/        # Zod schemas shared across frontend/backend
├── data/          # Sample PDFs and vector_store persistence
├── docs/          # Execution plan and architecture notes
└── scripts/       # Helper scripts (e.g., seed PDFs into the frontend)
Safety Notes
Every response includes the disclaimer “This is informational assistance, not legal advice.”
Vector stores persist locally in data/vector_store/{ingest_id} for privacy and repeat use.
Q&A refuses to answer outside retrieved context and always cites file + page numbers.
