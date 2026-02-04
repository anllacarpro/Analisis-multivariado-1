# MicroSaaS Media Toolbox — Option A Kickoff (Next.js + NestJS + BullMQ)

This document turns **Option A** into a build-ready plan: repo structure, environment config, first sprint backlog, MVP spec, and wireframe descriptions.

## 1) Architecture (high level)

**Frontend**
- Next.js 14 (App Router)
- Tailwind CSS + Radix UI
- Uppy for drag-and-drop uploads + resumable chunking

**Backend**
- NestJS (API + workers)
- BullMQ + Redis for queues
- FFmpeg for audio/video processing
- Sharp/ImageMagick for images
- Poppler/PDFTK for PDFs
- AI providers via API (modular)

**Storage**
- S3-compatible for production (AWS/Backblaze/MinIO)
- Local filesystem for dev

**Database**
- PostgreSQL + Prisma

**Payments**
- Stripe Checkout + webhooks

**Observability**
- OpenTelemetry + Grafana/Tempo
- Sentry for frontend + backend

## 2) Repository Structure (monorepo)

```
/apps
  /web                 # Next.js app
  /api                 # NestJS REST API
  /worker              # NestJS workers (queue processors)
/packages
  /ui                  # shared UI components
  /config              # shared configs (eslint, tsconfig)
  /media-lib           # shared FFmpeg/Sharp helpers
  /types               # shared types & DTOs
  /sdk                 # optional client SDK
/infra
  /docker              # Dockerfiles
  /k8s                 # Helm charts / manifests
  /terraform           # infra provisioning
/docs
  /architecture        # diagrams, ADRs
  /product             # requirements + specs
```

## 3) Environment Configuration

### Common (.env.example)
```
# App
APP_ENV=development
APP_URL=http://localhost:3000

# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/microsaas

# Redis
REDIS_URL=redis://localhost:6379

# Storage
S3_ENDPOINT=http://localhost:9000
S3_BUCKET=microsaas-dev
S3_ACCESS_KEY=local
S3_SECRET_KEY=local
S3_REGION=us-east-1

# Stripe
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# Auth
NEXTAUTH_SECRET=...
NEXTAUTH_URL=http://localhost:3000

# AI Providers
AI_PROVIDER=openai
AI_API_KEY=...
```

## 4) MVP Spec (Phase 1)

### Must-have Features
- Authentication (email + OAuth)
- Upload center with drag & drop
- Image tools: resize, crop, convert format
- Video tools: convert format, trim
- Audio tools: convert format, normalize loudness
- PDF tools: merge, split
- Job history + download manager
- Basic free tier limits

### Non-goals (Phase 1)
- AI features (background removal, OCR, STT)
- Team collaboration
- API access

## 5) First Sprint Backlog (2 weeks)

**Frontend**
1. Marketing landing page + tool categories
2. Upload UI (Uppy)
3. Job status view (progress bars)
4. File output preview + download

**Backend**
1. NestJS API scaffolding
2. Auth (NextAuth + API integration)
3. Job model + CRUD
4. Queue workers for FFmpeg + Sharp
5. Storage adapter (local + S3)

**Infra**
1. Docker compose (postgres + redis + minio)
2. CI pipeline (lint + tests)

## 6) Wireframe Descriptions

### Home / Landing
- Hero: "All-in-one Media Toolbox"
- CTA: “Upload file”
- Grid: Image / Video / Audio / PDF
- Pricing teaser + FAQ

### Dashboard
- Drag & drop upload area
- Tool selector on the right
- Recent jobs list with status
- Usage summary (credits + storage)

### Tool Page
- Input files list
- Parameters panel
- Live progress + output preview
- Download button

### Billing
- Plan cards
- Usage/credit history
- Manage subscription (Stripe portal)

## 7) Next Steps

1. Confirm hosting provider (AWS, Render, or Fly.io)
2. Pick AI providers for Phase 2
3. Finalize MVP limits and pricing
4. Start repo scaffolding
