# Shortlist

**AI-powered recruitment automation.** Shortlist analyzes applications, qualifies candidate profiles, and tracks every hiring process from first CV to final decision.

---

## Core purpose

Recruiters spend most of their time reading CVs, comparing them to job requirements, and updating spreadsheets. Shortlist automates the repetitive part so humans can focus on interviews and decisions.

1. **Ingest** applications (CV upload or API).
2. **Parse** each CV into structured data (skills, experience, education, languages).
3. **Score** each candidate against a job's requirements with an explainable result.
4. **Track** candidates through a configurable hiring pipeline.

## Key features

### Application analysis
- CV upload (PDF, DOCX) and text extraction
- Structured profile extraction: contact, skills, experience, education, languages
- Duplicate candidate detection

### Candidate qualification
- Job-to-candidate matching with a score from 0 to 100
- Explainable results: matched skills, missing skills, experience gap
- Configurable criteria per job (must-have vs nice-to-have)
- Automatic ranking and shortlist generation

### Pipeline tracking
- Kanban pipeline: New, Screened, Interview, Offer, Hired, Rejected
- Status history and notes for every candidate
- Dashboard with funnel metrics (volume, conversion, time per stage)

### Job management
- Create and edit job postings with required skills and weights
- Per-job candidate list and shortlist view

## How scoring works

```
CV file -> text extraction -> structured profile -> matching vs job criteria -> score + explanation
```

- `src/lib/parsers/cv-parser.ts` turns raw files into a structured profile.
- `src/lib/ai/matching.ts` compares the profile to the job requirements.
- `src/lib/ai/scoring.ts` computes the final weighted score and the explanation.

The AI assists the recruiter; it does not decide. Every score shows its reasoning, and final decisions stay with a human.

## Tech stack

| Layer | Choice |
|---|---|
| Framework | Next.js (App Router) + TypeScript |
| Styling | Tailwind CSS |
| Database | PostgreSQL via Prisma |
| AI | LLM API for parsing and matching |
| Storage | Local or S3-compatible file storage for CVs |

## Project structure

```
src/
  app/
    dashboard/        Funnel metrics and overview
    jobs/[id]/        Job list and job detail
    candidates/[id]/  Candidate list and profile
    pipeline/         Kanban pipeline
    api/              candidates, jobs, screening, upload
  components/         ui, candidates, jobs, pipeline
  lib/
    ai/               scoring.ts, matching.ts
    parsers/          cv-parser.ts
    db/               client.ts
  hooks/ types/ services/ utils/
prisma/schema.prisma  Data model
```

## Data model (core entities)

- **Job**: title, description, required skills, weights, status
- **Candidate**: contact info, parsed profile, source, CV file
- **Application**: links a candidate to a job, with score, explanation and stage
- **StageEvent**: history of pipeline movements and notes

## API overview

| Route | Purpose |
|---|---|
| `POST /api/upload` | Upload a CV |
| `GET/POST /api/candidates` | List and create candidates |
| `GET/POST /api/jobs` | List and create jobs |
| `POST /api/screening` | Run parsing and scoring for an application |

## Getting started

```bash
cp .env.example .env.local
# fill in DATABASE_URL and your AI provider key

npm install
npx prisma migrate dev
npm run dev
```

Open http://localhost:3000.

## Environment variables

```
DATABASE_URL=
AI_API_KEY=
STORAGE_PATH=./uploads
```

## Responsible use

Automated screening can introduce bias. Shortlist is designed to:
- show the reasons behind every score,
- avoid using protected attributes (age, gender, origin, photo) in scoring,
- keep a human in the loop for every accept or reject decision,
- handle candidate data in line with GDPR (consent, retention, deletion).

## Roadmap

- [ ] Email integration for automatic application intake
- [ ] Interview scheduling
- [ ] Custom pipeline stages
- [ ] Bulk import and export
- [ ] Multilingual CV support

## License

Private project. All rights reserved.
