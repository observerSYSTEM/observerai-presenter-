# Architecture Overview

ObserverAI Presenter follows an **open-core + managed cloud** architecture.

This repository exposes the **reference design and orchestration logic** for an
AI presenter pipeline, while production execution, scaling, and commercial
features are handled by **ObserverAI Presenter Cloud**.

This separation is intentional.

---

## 🧠 Architectural Principles

The system is designed around the following principles:

- **Open-core transparency**  
  Architecture and design are public; execution is managed.

- **Consent-first identity handling**  
  Identity assets are owned by the user and handled explicitly.

- **Service-oriented execution**  
  Rendering is a managed service, not a local binary.

- **Scalable by design**  
  Components can be swapped or scaled independently.

- **Business-safe defaults**  
  No impersonation, no hidden automation, no deceptive flows.

---

## 🧩 High-Level System Diagram



┌─────────────┐
│ Client │
│ (Browser) │
└─────┬───────┘
│
▼
┌─────────────────────┐
│ Web Application │
│ (Next.js) │
│ │
│ - Upload identity │
│ - Enter script │
│ - Choose accent │
│ - Select mode │
└─────┬───────────────┘
│ HTTPS
▼
┌─────────────────────┐
│ Backend API │
│ (FastAPI) │
│ │
│ - Auth │
│ - Job creation │
│ - Quotas & limits │
│ - Metadata storage │
└─────┬───────────────┘
│ Job enqueue
▼
┌─────────────────────┐
│ Job Queue │
│ (Redis / RQ) │
└─────┬───────────────┘
│
▼
┌─────────────────────┐
│ Render Worker │
│ (Managed) │
│ │
│ - Voice synthesis │
│ - Accent routing │
│ - Avatar animation │
│ - Video composition │
└─────┬───────────────┘
│
▼
┌─────────────────────┐
│ Storage │
│ (S3 / R2 / MinIO) │
│ │
│ - Rendered videos │
│ - Temporary assets │
└─────┬───────────────┘
│
▼
┌─────────────────────┐
│ User Dashboard │
│ │
│ - Download output │
│ - Share link │
│ - Manage projects │
└─────────────────────┘


---

## 🔓 Open-Core vs Cloud Responsibilities

### What lives in this repository (Open-Core)

- Pipeline orchestration logic
- Accent routing abstraction
- Reference audio/video composition
- API skeleton & schemas
- Documentation & policies
- Local demo (non-commercial)

### What lives in ObserverAI Presenter Cloud

- GPU acceleration
- Production avatar models
- Professional voice packs
- Live virtual camera output
- Billing & quotas
- User dashboard
- Audit logging
- Commercial licensing

This boundary protects:
- performance
- IP
- ethical controls
- business sustainability

---

## 🎬 Rendering Flow (Detailed)

### Step 1 — Input
User provides:
- Script text or audio
- Selected accent & voice
- Presentation mode (video or live)

### Step 2 — Job Creation
Backend API:
- Validates input
- Checks quota & plan
- Creates a render job
- Enqueues job

### Step 3 — Rendering
Worker performs:
1. Text → speech (TTS)
2. Audio → lip-sync
3. Avatar animation
4. Frame composition
5. Watermark / branding
6. Video export

### Step 4 — Delivery
- Output stored securely
- User notified
- Download / share enabled

---

## 🎥 Live Virtual Camera Mode (Cloud Only)

Live mode follows a similar flow but replaces file rendering with
**real-time frame streaming**.



Avatar Engine → Frame Stream → Virtual Camera Driver → Video App


Supported targets:
- Zoom
- Google Meet
- Microsoft Teams
- OBS / screen recording tools

Mobile platforms are not guaranteed.

---

## 🔐 Identity & Consent Handling

Identity assets:
- Are provided by the user
- Are not shared between users
- Are processed only for the requested job
- Are retained for a limited time (configurable)

Consent is enforced via:
- Explicit user confirmation
- Terms of Service
- Audit logs (cloud)

See:
- `ethical-use.md`
- `consent-policy.md`

---

## 📈 Scalability Model

The system scales horizontally:

- API servers scale independently
- Workers scale independently
- Storage is externalised
- Rendering providers can be swapped

This allows:
- Cost control
- Regional expansion
- Enterprise isolation
- Future self-hosted GPU migration

---

## 🔄 Future Evolution (Non-Binding)

Planned evolution paths include:
- Self-hosted GPU workers
- Enterprise private deployments
- Real-time API integrations
- Expanded voice & language support

Details are intentionally omitted from the public repository.

---

## 🧭 Summary

ObserverAI Presenter is designed as **presentation infrastructure**, not a novelty tool.

The architecture prioritises:
- professionalism
- ethical use
- scalability
- commercial viability

This document describes the **reference architecture** only.
Production execution is handled by **ObserverAI Presenter Cloud**.

---

End of document.
