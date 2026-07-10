# 🧑‍💻 Yash Agarwal - Master Profile

> **Last Updated:** 2026-06-26
> This is a living document. Update it whenever you gain new skills, complete projects, or change roles. Use it to generate tailored resumes for specific JDs.

---

## 📌 Personal Info

| Field | Value |
|---|---|
| **Full Name** | Yash Agarwal |
| **Handle** | XploY04 |
| **Phone** | +91 8140724216 |
| **Email** | 2004agarwalyash@gmail.com |
| **LinkedIn** | [linkedin.com/in/XploY04](https://www.linkedin.com/in/XploY04) |
| **GitHub** | [github.com/XploY04](https://github.com/XploY04) |
| **Linktree** | [linktr.ee/XploY04](https://linktr.ee/XploY04) |
| **Location** | Bengaluru, Karnataka, India |

**Tagline:** Software Engineer @ RemoteStar & Final Round AI | LFX'25 @ CloudNativePG | Kubeflow Contributor | Reliance Foundation Scholar

**Summary:** I love to explore tech and solve engineering problems. Currently working on resume, jobs, interview, billing, and AI platform systems at RemoteStar and Final Round AI, with a strong backend and infrastructure focus plus CNCF open-source contributions across CloudNativePG, Kubeflow, and LitmusChaos.

---

## 🎓 Education

### Dayananda Sagar College of Engineering, Bengaluru
- **Degree:** B.E. in Information Science and Engineering
- **Duration:** September 2023 – June 2027 (Expected)
- **CGPA:** 8.9

### Scholar English Academy
- **Level:** High School (Science stream)
- **Duration:** June 2020 – May 2022

### Agarwal Vidya Vihar
- **Level:** Secondary School
- **Year:** 2020

---

## 🛠 Technical Skills

### Languages & Frameworks
- Go
- Python
- TypeScript
- Node.js
- SQL
- REST APIs
- WebSockets

### Infrastructure & DevOps
- Docker / Docker Compose
- Kubernetes / Kubernetes CRDs
- GCP (Cloud Run, etc.)
- AWS (S3, etc.)
- CI/CD Pipelines
- Firebase (Auth, Hosting, Security Rules)

### Databases
- PostgreSQL (full-text search, tsvector, GIN indexing)
- MongoDB (aggregation pipelines)
- Redis

### Monitoring & Observability
- Prometheus
- Grafana
- Datadog (custom metrics)

### Messaging & Storage
- RabbitMQ
- Google Cloud Storage (GCS)

### AI / ML Tools
- Gemini API
- OpenAI Structured Outputs
- OpenCV

### Other
- Litmus Chaos
- Jepsen (consistency verification)
- Megatron-Core (Tensor Parallelism)
- PyTorch
- Next.js 14
- FastAPI
- APScheduler
- Nodemailer / SMTP
- Cloudinary
- JWT / RBAC
- Stripe
- SQS
- Gotenberg

---

## 💼 Work Experience

### 1. RemoteStar - Software Engineer
- **Duration:** March 2026 – Present
- **Type:** Current Role – Remote
- **Stack:** Go, TypeScript, MongoDB, Redis, OpenAI Structured Outputs, Firebase Auth, SQS, Gotenberg, GitOps

**Key Contributions:**
- Built saved-jobs functionality across backend and frontend, adding idempotent save/unsave APIs, a `saved_jobs` collection with indexes, hydrated listings, UI save toggles, and a Saved tab.
- Added on-demand per-job resume analysis with 7-day cache hits, Redis per-user/job locks, credit ledger charging/refunds, and structured-output scoring for candidate-job fit.
- Reworked resume parsing from legacy JSON mode to OpenAI Structured Outputs using a strict `ParsedResume` schema, validator, and cleaner prompt path.
- Rebuilt the tailor-resume pipeline around Jake-style one-page output, passing raw Tika text plus parsed JSON to the LLM and adding tested normalization for section order, aliases, forbidden sections, and empty-section cleanup.
- Added admin job management with admin-only CRUD, soft archive/restore, public filtering for archived jobs, and verified-domain admin access.
- Added IP-geolocated display-currency support for billing surfaces while keeping Stripe checkout as the source of truth for conversion.
- Added a free daily cap for Compass coach/counsellor chat, returning a 429 cap response before streaming and surfacing the limit in the frontend.
- Added SQS-backed resume processing queue work and Gotenberg deployment support for HTML-to-PDF resume rendering.
- Reworked the signup flow to defer backend-user creation until email verification, extracting an idempotent `ensureBackendUser` helper so a mistyped/abandoned email leaves no Firebase or backend record behind and the corrected address can register cleanly.
- Built an achievements editor as a bullet list (matching projects) and preserved achievement plus profile-section bullets through render instead of collapsing them.
- Hardened saved jobs to stop silently dropping bookmarks on a transient lookup error, and allowed signup without a last name.
- Raised the nginx ingress `proxy-read-timeout` to 180s on the wildcard host for long-running requests.

**Impact Themes:**
- Candidate job discovery and saved jobs
- Resume parsing, tailoring, rendering, and per-job analysis
- Admin job operations, billing UX, chat quota control, and deployment support
- Signup/auth correctness (email-verification-gated user creation) and resume-editor fidelity

---

### 2. Final Round AI - Software Engineer
- **Duration:** April 2026 – Present
- **Type:** Current Role – Remote
- **Stack:** Python, async HTTP, Stripe, Clerk, React/TypeScript, resume parsing, Copilot context

**Key Contributions:**
- Replaced a blocking `requests.delete()` call inside an async payment-account delete path with `httpx.AsyncClient`, adding timeout-specific handling and unit coverage.
- Fixed a noisy concurrent Stripe idempotency race by downgrading the known recoverable path from error logs to warning logs while preserving error logging for real Stripe failures.
- Fixed `None`-safe company access in resume work-experience validation.
- Worked on Copilot resume context cache validation across resume upload, deletion, session context, workflow events, and tests.
- Improved Copilot resume/material setup by waiting for backend `usable_status=success`, filtering out processing/failed files, and tightening upload validation, empty states, and selected resume/material ID behavior.
- Built the **Report V2 backend** (`frai_js_server`): the Node `interview_reports` worker now generates and persists a `schema_version: 2` document with a per-question Stage 1 (canonical `answer_summary`/`strengths`/`gaps`/`evidence`/`next_action` fields plus stable `question_id`) and a new aggregate Stage 2 (overall headline, five dimension scores, ranked improvement areas, practice items, home insights). Added an evidence gate that blocks a report from reaching `completed` unless every rendered score carries non-empty evidence (regenerate once, then fail), plus structured lifecycle logs and Datadog status/duration metrics tagged by `report_type`/`schema_version`. Pre-V2 documents render unchanged.
- Fixed blank report emails (~40% of recently emailed reports) by tracing in prod that abnormally-ended/2-hour-timeout sessions finalized and emailed without publishing the RabbitMQ `end` event, so the website's worker never built the viewable report; made `_session_timeout_worker` publish the `end` event before scheduling report generation.
- Persisted full report input to GCS (`report-inputs/{userId}/{sessionId}/requests.v1.json`) so the worker can rebuild long-session inputs after Redis expiry instead of relying on the truncatable (1 MiB) Firestore session document, with a non-fatal fallback to the prior Firestore path.
- Rejected scanned/image-only PDFs before upload, fixed report-page routing and a null-deref crash, fixed the report email detail link, and skipped an archived goal in the last-used config so a deleted role isn't replayed on Launch Copilot.

**Impact Themes:**
- Payment reliability and cleaner production observability
- Structured, evidence-backed interview reports (Report V2) and report-delivery reliability
- Resume parsing and Copilot context correctness
- Frontend/backend alignment around upload status and selected resume/material IDs

---

### 3. [Seeqlo](https://seeqlo.com/) - SDE Intern
- **Duration:** July 2025 – February 2026 (8 months)
- **Type:** Internship – Remote
- **Stack:** Go, GCP, Firebase, CI/CD, RBAC

**Key Contributions:**
- Rewrote the **entire backend in Go** from scratch, building a REST API with clean service separation for student–teacher workflows.
- Set up **separate staging and production environments** with CI/CD pipelines; Go backend deployed via Docker Compose on GCP Cloud Run, frontend on Firebase Hosting, cutting manual deployment to zero.
- Wrote **Firebase Security Rules** enforcing multi-role RBAC across collections, locking down sensitive academic data to the right student, teacher, and admin roles.
- Developed the backend pipeline for an **AI presentation maker** using Gemini API for content and image generation, with real-time sync and auto-save to keep user state consistent across sessions.

**Impact Metrics:**
- Zero manual deployment after CI/CD setup
- Multi-role RBAC (student, teacher, admin)
- Real-time sync + auto-save for AI presentations

---

### 4. [LFX Mentee - CloudNativePG](https://github.com/cloudnative-pg/chaos-testing) (Linux Foundation / CNCF)
- **Duration:** September 2025 – December 2025 (4 months)
- **Type:** Mentorship – Remote
- **Stack:** Go, Bash, Kubernetes, PostgreSQL, CloudNativePG, LitmusChaos, Jepsen, Prometheus, Grafana, GitHub Actions
- **Mentors:** Gabriele Bartolini, Marco Nenciarini, Francesco Canovai, Jonathan Gonzalez
- **Showcase:** Presented at the Linux Foundation Mentorship Showcase ([poster](https://lnkd.in/d4CPXmVz), [project](https://lnkd.in/d8qDAGwX))

**Key Contributions:**
- Built the [CloudNativePG chaos-testing](https://github.com/cloudnative-pg/chaos-testing) harness for PostgreSQL HA validation, combining LitmusChaos pod deletion, Jepsen consistency checks, Prometheus probes, and GitHub Actions automation.
- Created a reproducible E2E flow that provisions a CNPG Playground Kind cluster, installs the CloudNativePG operator, deploys a 3-instance `pg-eu` cluster, installs LitmusChaos, runs Jepsen against PostgreSQL, injects primary pod deletion, and exports test artifacts.
- Added two ChaosEngine paths: a no-probe smoke test for validating Litmus wiring without Prometheus, and a probe-enabled Jepsen workflow that validates cluster health before and after chaos.
- Wrote `run-jepsen-chaos-test.sh` to orchestrate Jepsen deployment, chaos injection, result extraction from PVCs, Litmus `ChaosResult` export, operation statistics, latency/rate graphs, and cleanup.
- Wrote `monitor-cnpg-pods.sh` and `PodMonitor` configuration to watch CNPG primary/replica roles, Kubernetes events, ChaosEngine state, and Prometheus metrics during failover experiments.
- Solved a LitmusChaos targeting gap for operator-managed CNPG pods by using the `TARGETS` environment variable to target pods with `cnpg.io/instanceRole=primary` instead of relying on standard Deployment/StatefulSet `appkind` targeting.
- Shipped upstream [chaos-operator#510](https://github.com/litmuschaos/chaos-operator/pull/510), adding `labelMatchMode` to `ApplicationParams` and `Workload`, updating `getTargets()` to emit `kind:namespace:[labels]:labelMatchMode`, regenerating CRDs, and preserving backward-compatible `union` behavior.
- Authored related [litmus-go#777](https://github.com/litmuschaos/litmus-go/pull/777) to implement runtime label intersection selection, with `LabelMatchMode` parsing, intersection-based pod lookup, and unit tests. This PR is the runtime counterpart to the merged `chaos-operator` API/wire-format change.
- Participated in [litmus-go#774](https://github.com/litmuschaos/litmus-go/issues/774), the CloudNativePG/LitmusChaos design discussion that identified the need for precise pod targeting for CNPG primary pods.

**Repository Context:**
- `clusters/pg-eu-cluster.yaml` defines a 3-instance CloudNativePG cluster with one primary and two replicas.
- `experiments/cnpg-jepsen-chaos.yaml` runs primary pod deletion with Jepsen and Prometheus start/end probes.
- `experiments/cnpg-jepsen-chaos-noprobes.yaml` runs the same pod-delete experiment without Prometheus for first-pass validation.
- `litmus-rbac.yaml` grants the Litmus service account access to pods, events, secrets, jobs, and Litmus chaos resources.
- `.github/workflows/chaos-test-full.yml` runs the full workflow in CI: Kind, CNPG, Litmus, Prometheus, Jepsen, artifact upload, and final cluster status checks.

**Impact Metrics:**
- Sub-minute failover confirmed
- Zero data loss under failure conditions
- Merged upstream CloudNativePG PR: [chaos-testing#3](https://github.com/cloudnative-pg/chaos-testing/pull/3) (chaos-testing setup + experiment documentation)
- Merged upstream LitmusChaos PR: [chaos-operator#510](https://github.com/litmuschaos/chaos-operator/pull/510)
- Open runtime follow-up PR: [litmus-go#777](https://github.com/litmuschaos/litmus-go/pull/777)
- Closed design issue: [litmus-go#774](https://github.com/litmuschaos/litmus-go/issues/774)

---

### 5. AMX Innovations - Full-Stack Developer Intern
- **Duration:** February 2025 (1 month)
- **Type:** Internship – Remote
- **Stack:** AWS, OpenCV, REST APIs

**Key Contributions:**
- Built a client-side **Ground Control Point Detection model** using OpenCV achieving **95% accuracy**.
- Engineered a **batch upload pipeline** for 50,000+ drone images to AWS S3 with fault-tolerant error recovery.
- Built REST APIs for real-time progress tracking of large-scale image detection workloads.
- Optimized cloud resource handling and error recovery mechanisms for better fault tolerance.
- Collaborated with AI and infrastructure teams to deliver scalable, high-throughput backend services.

**Impact Metrics:**
- 95% accuracy on GCP detection
- 50,000+ images processed with fault tolerance

---

### 6. Embrays Technologies - Software Developer
- **Duration:** October 2024 – January 2025 (4 months)
- **Type:** *(details not yet documented, update when available)*

> ⚠️ **TODO:** Add detailed bullet points for Embrays Technologies role.

---

## 🌐 Open Source Contributions

### Kubeflow (CNCF)
- **Role:** Contributor across `trainer`, `sdk`, `website`, `pipelines`, and `notebooks`
- **Stack:** Go, Python, Kubernetes API, Kubernetes CRDs, Megatron-Core, PyTorch

**Kubeflow Trainer:**
- Shipped [trainer#3201](https://github.com/kubeflow/trainer/pull/3201), adding a **Megatron-Core GPT Tensor Parallelism** example notebook demonstrating multi-GPU distributed training on TrainJob.
- Shipped [trainer#3434](https://github.com/kubeflow/trainer/pull/3434), unblocking the Megatron TP notebook on the GPU E2E pipeline.
- Authored merged design proposal [trainer#3068](https://github.com/kubeflow/trainer/pull/3068) introducing **`TTLSecondsAfterFinished`** and **`ActiveDeadlineSeconds`** fields to the TrainJob CRD for job lifecycle control.
- Shipped the merged implementation [trainer#3258](https://github.com/kubeflow/trainer/pull/3258), adding `activeDeadlineSeconds` to the TrainJob CRD (Fixes [#2899](https://github.com/kubeflow/trainer/issues/2899), KEP [#3068](https://github.com/kubeflow/trainer/pull/3068)); the earlier [trainer#3065](https://github.com/kubeflow/trainer/pull/3065) was the closed first-pass.

**Kubeflow SDK:**
- Shipped [sdk#307](https://github.com/kubeflow/sdk/pull/307), debugging and fixing all failing E2E tests by identifying the root cause of timeout/lookup issues, handling Kubernetes API 404/403 errors gracefully, and installing the SDK locally in the workflow.
- Drove follow-up [sdk#415](https://github.com/kubeflow/sdk/pull/415) (in review) wiring `ActiveDeadlineSeconds` through the Python SDK's TrainJob configuration, plus issue [sdk#403](https://github.com/kubeflow/sdk/issues/403) scoping the work.

**Kubeflow Website:**
- Shipped [website#4360](https://github.com/kubeflow/website/pull/4360), authoring the **Megatron-Core Tensor Parallelism** user guide for the Trainer docs.
- Shipped [website#4348](https://github.com/kubeflow/website/pull/4348), authoring the **Configure TrainJob Lifecycle** user guide covering the new TTL/deadline fields.

**Kubeflow Pipelines:**
- Filed [pipelines#12728](https://github.com/kubeflow/pipelines/issues/12728) proposing official Helm charts for Kubeflow Pipelines, and authored the follow-up KEP [pipelines#12842](https://github.com/kubeflow/pipelines/pull/12842).

**Kubeflow Notebooks:**
- Authored [notebooks#835](https://github.com/kubeflow/notebooks/pull/835), expanding the `DEVELOPMENT_GUIDE.md` with component-specific local development workflows for the controller, backend, and frontend, plus instructions for working with Kubernetes webhooks locally (disable, self-signed certs, Tilt).

### LitmusChaos (CNCF Sandbox Project)
- **Role:** Contributor
- **Stack:** Go, Kubernetes, CRDs
- Worked on precise pod targeting for operator-managed systems like CloudNativePG, where the PostgreSQL primary pod is selected by labels instead of a standard Deployment/StatefulSet workload.
- Shipped upstream [chaos-operator#510](https://github.com/litmuschaos/chaos-operator/pull/510), adding `labelMatchMode` to `ApplicationParams` and `Workload`, updating `getTargets()` to emit `kind:namespace:[labels]:labelMatchMode`, and regenerating CRDs with backward-compatible `union` defaults.
- Authored [litmus-go#777](https://github.com/litmuschaos/litmus-go/pull/777), the runtime follow-up for [**issue #774**](https://github.com/litmuschaos/litmus-go/issues/774), adding label intersection parsing and pod selection logic for cases that need AND semantics across labels.
- Filed [litmus-go#778](https://github.com/litmuschaos/litmus-go/issues/778) proposing a `PreTargetSelection` probe mode for stateful workloads, addressing `TARGET_SELECTION_ERROR` failures when operator-managed pods (e.g. a promoted CloudNativePG primary) update their labels 60–120s after a chaos iteration begins.

---

## 📈 Recent GitHub Activity (Last 6 Months)

### Final Round AI
- **Period covered:** November 29, 2025 – June 27, 2026
- **Repos:** [`finalroundai/interviewai`](https://github.com/finalroundai/interviewai), [`finalroundai/frai-website`](https://github.com/finalroundai/frai-website), [`finalroundai/frai_js_server`](https://github.com/finalroundai/frai_js_server), [`finalroundai/finalround-desktop`](https://github.com/finalroundai/finalround-desktop)
- **Stack:** Python, Node.js, FastAPI-style services, async HTTP, Stripe, Clerk, React/TypeScript, RabbitMQ, GCS, Datadog, resume parsing, Copilot context

**Significant Work:**
- Shipped [interviewai#2401](https://github.com/finalroundai/interviewai/pull/2401), replacing a blocking `requests.delete()` call inside an async payment-account delete path with `httpx.AsyncClient`, adding timeout-specific handling, and keeping unit coverage green.
- Shipped [interviewai#2396](https://github.com/finalroundai/interviewai/pull/2396), downgrading a known concurrent Stripe idempotency race from error logs to warning logs while preserving error logging for real Stripe failures.
- Shipped [interviewai#2395](https://github.com/finalroundai/interviewai/pull/2395), fixing `None`-safe company access in resume work-experience validation.
- Opened [interviewai#2413](https://github.com/finalroundai/interviewai/pull/2413), validating Copilot resume context cache behavior across resume upload, deletion, session context, workflow events, and tests.
- Opened [frai-website#999](https://github.com/finalroundai/frai-website/pull/999), improving Copilot resume/material setup by waiting for backend `usable_status=success`, filtering out processing/failed files, and tightening upload validation and empty states.

**June work — Report V2 and report-delivery reliability:**
- Shipped [frai_js_server#744](https://github.com/finalroundai/frai_js_server/pull/744), the full Report V2 backend: `schema_version: 2` generation (per-question Stage 1 + aggregate Stage 2), an evidence gate before `completed`, GCS read-side input recovery, and Datadog lifecycle metrics; with the in-progress frontend in [frai-website#1080](https://github.com/finalroundai/frai-website/pull/1080), [#1056](https://github.com/finalroundai/frai-website/pull/1056), [#1055](https://github.com/finalroundai/frai-website/pull/1055), [#1053](https://github.com/finalroundai/frai-website/pull/1053).
- Opened [interviewai#2463](https://github.com/finalroundai/interviewai/pull/2463), persisting full report input to GCS at session end (the write side that pairs with the worker's GCS read).
- Shipped [interviewai#2452](https://github.com/finalroundai/interviewai/pull/2452), fixing blank report emails (~40% of recently emailed reports) by publishing the RabbitMQ `end` event on session timeout so the website's report worker runs.
- Shipped [frai_js_server#743](https://github.com/finalroundai/frai_js_server/pull/743) (skip archived goal in last-used config), [frai_js_server#740](https://github.com/finalroundai/frai_js_server/pull/740) (allow clearing `company_detail`/`job_description`), [interviewai#2440](https://github.com/finalroundai/interviewai/pull/2440) (fix report email detail link), and [frai-website#1046](https://github.com/finalroundai/frai-website/pull/1046) (remove duplicate upload error toast).
- Opened [finalround-desktop#111](https://github.com/finalroundai/finalround-desktop/pull/111), rejecting scanned/image-only PDFs before upload.

**Impact Themes:**
- Payment reliability and cleaner production observability
- Structured, evidence-backed interview reports (Report V2) and report-delivery reliability
- Resume parsing and Copilot context correctness
- Frontend/backend alignment around upload status and selected resume/material IDs

### RemoteStar Candidate Platform
- **Period covered:** November 29, 2025 – June 27, 2026
- **Repos:** [`RemoteStar-AI/Candidate-backend`](https://github.com/RemoteStar-AI/Candidate-backend), [`RemoteStar-AI/Candidate-frontend`](https://github.com/RemoteStar-AI/Candidate-frontend), [`RemoteStar-AI/Gitops`](https://github.com/RemoteStar-AI/Gitops), [`govindup63/remotestar-candidate`](https://github.com/govindup63/remotestar-candidate), [`govindup63/remotestar-backend`](https://github.com/govindup63/remotestar-backend)
- **Stack:** Go, TypeScript, MongoDB, Redis, OpenAI Structured Outputs, Gotenberg, Firebase Auth, SQS, GitOps

**Significant Work:**
- Built saved jobs end-to-end across backend and frontend through [Candidate-backend#33](https://github.com/RemoteStar-AI/Candidate-backend/pull/33) and [Candidate-frontend#28](https://github.com/RemoteStar-AI/Candidate-frontend/pull/28), adding idempotent save/unsave APIs, a `saved_jobs` collection, indexes, hydration, UI save toggles, and a Saved tab.
- Added on-demand per-job resume analysis in [Candidate-backend#25](https://github.com/RemoteStar-AI/Candidate-backend/pull/25) and [Candidate-frontend#14](https://github.com/RemoteStar-AI/Candidate-frontend/pull/14), with 7-day cache hits, Redis per-user/job locks, credit ledger charging/refunds, and structured-output scoring.
- Reworked resume parsing in [Candidate-backend#31](https://github.com/RemoteStar-AI/Candidate-backend/pull/31), moving `POST /api/resumes/parse` from legacy JSON mode to OpenAI Structured Outputs with a strict `ParsedResume` schema and validator.
- Rebuilt the tailor-resume pipeline around Jake-style one-page output in [Candidate-backend#20](https://github.com/RemoteStar-AI/Candidate-backend/pull/20), passing raw Tika text plus parsed JSON to the LLM and adding a normalizer with tests for section order, aliases, forbidden sections, and empty-section cleanup.
- Added admin job management in [remotestar-candidate#84](https://github.com/govindup63/remotestar-candidate/pull/84), including admin-only job CRUD, soft archive/restore, public filtering for archived jobs, and verified-domain admin access.
- Added IP-geolocated display currency support in [Candidate-backend#35](https://github.com/RemoteStar-AI/Candidate-backend/pull/35) and [Candidate-frontend#31](https://github.com/RemoteStar-AI/Candidate-frontend/pull/31), using display-only currency labels while leaving Stripe checkout as the source of truth for conversion.
- Added a free daily cap for Compass coach/counsellor chat in [Candidate-backend#34](https://github.com/RemoteStar-AI/Candidate-backend/pull/34) and [Candidate-frontend#30](https://github.com/RemoteStar-AI/Candidate-frontend/pull/30), returning a 429 cap response before streaming and surfacing the limit in chat.
- Added resume processing queue work in [remotestar-backend#11](https://github.com/govindup63/remotestar-backend/pull/11), introducing SQS-backed queue integration and logging for resume processing.
- Added Gotenberg deployment support in [Gitops#1](https://github.com/RemoteStar-AI/Gitops/pull/1) for HTML-to-PDF resume rendering.

**June work — auth correctness and resume-editor fidelity:**
- Reworked signup to defer backend-user creation until email verification across [Candidate-frontend#35](https://github.com/RemoteStar-AI/Candidate-frontend/pull/35) and [#34](https://github.com/RemoteStar-AI/Candidate-frontend/pull/34), adding an idempotent `ensureBackendUser` helper so a mistyped/abandoned email leaves no Firebase or backend record and the corrected address can register cleanly.
- Built an achievements bullet-list editor in [Candidate-frontend#36](https://github.com/RemoteStar-AI/Candidate-frontend/pull/36) and preserved achievement/profile-section bullets through render in [Candidate-backend#39](https://github.com/RemoteStar-AI/Candidate-backend/pull/39).
- Stopped silently dropping saved jobs on a transient lookup error in [Candidate-backend#36](https://github.com/RemoteStar-AI/Candidate-backend/pull/36), and allowed signup without a last name in [Candidate-backend#40](https://github.com/RemoteStar-AI/Candidate-backend/pull/40).
- Raised the ingress `proxy-read-timeout` to 180s on the wildcard host in [Gitops#2](https://github.com/RemoteStar-AI/Gitops/pull/2).

**Impact Themes:**
- Candidate job discovery, saved jobs, and per-job resume analysis
- Resume parsing, tailoring, rendering, and preview reliability
- Signup/auth correctness (email-verification-gated user creation)
- Admin job operations, billing display UX, chat quota control, and deployment support

### Kubeflow Follow-ups
- Shipped [trainer#3560](https://github.com/kubeflow/trainer/pull/3560), fixing Trainer examples to use the namespaced SQuAD dataset.
- Continued [sdk#415](https://github.com/kubeflow/sdk/pull/415), wiring `ActiveDeadlineSeconds` into TrainJob configuration, and kept [sdk#403](https://github.com/kubeflow/sdk/issues/403) open as the tracking issue.
- Continued [sdk#382](https://github.com/kubeflow/sdk/pull/382), an OpenTelemetry integration KEP for the Kubeflow SDK.

### ChatOps / Internal Tooling
- Shipped [remotestar-chatops#4](https://github.com/XploY04/remotestar-chatops/pull/4), [#5](https://github.com/XploY04/remotestar-chatops/pull/5), [#6](https://github.com/XploY04/remotestar-chatops/pull/6), and [#7](https://github.com/XploY04/remotestar-chatops/pull/7), adding Mixpanel mode, routing Plane channels to Mixpanel context, rewriting prompts to avoid early-refusal hallucinations, and respecting the OpenAI 128-tool cap.

---

## 🚀 Projects

### 1. SIH Internal Round Portal
- **Stack:** Next.js 14, TypeScript, MongoDB, Firebase Auth, JWT, Nodemailer
- **Links:** [Live](https://sih.dsce.in/) · [GitHub](https://github.com/XploY04/SIH-Internal-Round-Portal)

**Highlights:**
- Built and deployed a **production REST API** serving **200+ teams** and **100K+ page views**.
- **3-tier RBAC**, optimized MongoDB aggregation pipelines, transactional integrity, and automated endpoint testing across **35+ routes**.
- Engineered multi-evaluator ranking, duplicate detection, Cloudinary integration, and automated SMTP workflows.
- Delivered as a fully live, zero-downtime production platform.

**Impact Metrics:**
- 200+ teams served
- 100K+ page views
- 35+ tested API routes
- Zero downtime

---

### 2. jobs.ai
- **Stack:** Python, FastAPI, PostgreSQL, Gemini 2.5 Flash-Lite, Docker, APScheduler
- **Links:** [GitHub](https://github.com/XploY04/jobs.ai)

**Highlights:**
- Architected an **async ingestion pipeline** aggregating **10,000+ jobs per run** from 6 sources (ATS scrapers, RSS, Adzuna, HackerNews) with **100% processing success** (12,182/12,182).
- Engineered an AI enrichment pipeline using **Gemini 2.5 Flash-Lite** with 10 concurrent batch calls, extracting a structured 40-field schema and executing save-per-batch DB writes.
- Designed **PostgreSQL full-text search** with tsvector + GIN indexing, weighted fields, and relevance ranking achieving **~35ms** query latency.
- Implemented data deduplication (title + company hashing), age filtering, and SerpAPI-powered company discovery within an extensible agent-based architecture.

**Impact Metrics:**
- 10,000+ jobs processed per run
- 40-field structured AI extraction
- ~35ms complex search latency

---

### 3. Lakshya (Ascend)
- **Stack:** Next.js 15, React 19, TypeScript, MongoDB, Tailwind CSS
- **Links:** [Live](https://ascend-rosy.vercel.app/) · [GitHub](https://github.com/XploY04/Ascend)

**Highlights:**
- Built an **AI-powered learning roadmap platform** to simplify skill-building with structured, personalized learning paths.
- Engineered dynamic generation logic that tailors roadmaps to fit users' specific time constraints.
- Developed real-time roadmap editing capabilities, allowing for high flexibility and adaptability in learning paths.

**Impact Metrics:**
- AI-tailored personalized learning paths
- Real-time roadmap adaptability

---

### 4. Throtl - Wi-Fi Bandwidth Monitor & Throttler
- **Stack:** Python, Scapy, Django REST Framework, Django Channels (WebSockets), Redis pub/sub, React 19, TypeScript, Vite, `tc`/`iptables`
- **Links:** [Live](https://throtl.vercel.app) · [GitHub](https://github.com/XploY04/throtl)

**Highlights:**
- Built a **distributed system** that turns a Linux laptop into a Wi-Fi hotspot with real-time per-device bandwidth monitoring and throttling, split into three independent components communicating exclusively over **Redis pub/sub** (network engine, Django backend, React dashboard).
- Wrote a **Scapy packet sniffer** that sums bytes per client IP over a sliding window with 1-second update intervals, applying throttling via `tc`/`iptables`.
- Implemented **WebSocket-driven live stats** through Django Channels, streaming per-device download speed and throttle status, plus one-click REST/WebSocket throttle controls.
- Added **automatic throttling** that self-throttles devices exceeding configured thresholds with debounce logic.

**Impact Metrics:**
- Real-time per-device monitoring at 1-second granularity
- Engine-on-router / backend-anywhere topology via decoupled Redis messaging

---

### 5. Other Public Projects

- **[wispr](https://github.com/XploY04/wispr)** (Rust) — Cross-platform desktop app for real-time audio recording and live speech-to-text transcription.
- **[linkedin-ai-reply](https://github.com/XploY04/linkedin-ai-reply)** (JavaScript) — Chrome extension that turns `// + Enter` into an AI-generated reply on any LinkedIn comment box, with five-layer self-healing CSS selector resolution (gpt-4.1-mini).
- **[EcoMine](https://github.com/XploY04/EcoMine)** (JavaScript) — Web app that quantifies carbon emissions in coal mines, with data visualization and carbon-neutrality pathways.
- **[pomodoro-discord-bot](https://github.com/XploY04/pomodoro-discord-bot)** (Python) — Productivity Discord bot with Pomodoro timers, customizable intervals, and reminders.
- **[AI-Feedback-form](https://github.com/XploY04/AI-Feedback-form)** (Python) — Dynamic feedback system using Flask, OpenAI API, and MySQL for generating and storing contextual feedback.

---

## 🏆 Achievements

| Achievement | Details |
|---|---|
| **Linux Foundation LiFT Scholarship** | Awarded 2026 (Linux Foundation Training scholarship) |
| **Reliance Foundation Scholarship** | Top 5,000 of 100,000+ applicants nationwide |
| **1st Prize - Clone Wars** | Inter-college coding competition (1st year of college) |
| **4th / 500 - Incubate Hackathon** | Organized by JIPMER & IIT Bombay |
| **3rd / 1,200 - Sandbox Hackathon** | Cybersecurity hackathon, project: [Sentinel](https://devfolio.co/projects/sentinel-761d) |

---

## 🤝 Volunteering & Community

### Point Blank - Member
- **Location:** Bengaluru, India
- **Duration:** April 2024 – Present
- Elite multidisciplinary team of programmers from DSCE.

### Talks
- **Linux Foundation Mentorship Showcase:** Presented my LFX Mentorship project, CNCF CloudNativePG Chaos Testing, a reproducible chaos-testing framework that deletes the PostgreSQL primary under active traffic, observes failover, and verifies correctness with Jepsen (stack: CloudNativePG, LitmusChaos, Jepsen, Prometheus/Grafana, GitHub Actions).
- **Kubeflow Commons Hub Meetup, Pune (June 2026):** Delivered my first talk, on my Kubeflow contribution running Megatron-LM with tensor parallelism on Kubeflow Trainer.

---

## 📋 Resume Tailoring Cheat Sheet

Use this section to quickly map your experience to common JD themes:

| JD Theme | Relevant Experience |
|---|---|
| **Backend / Go** | RemoteStar candidate backend, Seeqlo (full Go rewrite), LitmusChaos (Go contributions), CloudNativePG (Go framework) |
| **Kubernetes / Cloud Native** | LFX CloudNativePG, LitmusChaos CRDs, Docker/K8s across roles |
| **Python / FastAPI** | Final Round AI backend reliability work, jobs.ai project (async pipeline, PostgreSQL FTS) |
| **Full-Stack / TypeScript** | RemoteStar candidate platform, Final Round AI Copilot setup flow, SIH Portal (Next.js 14, MongoDB, Firebase Auth) |
| **DevOps / CI-CD** | Seeqlo (Docker Compose + GCP Cloud Run + CI/CD), monitoring stacks |
| **AI / ML Integration** | Seeqlo (Gemini API presentations), AMX (OpenCV 95% accuracy), jobs.ai (Gemini batch), RemoteStar resume parsing/tailoring with OpenAI Structured Outputs |
| **Databases** | PostgreSQL (FTS, tsvector, GIN), MongoDB (aggregation), Redis |
| **Payments / Billing** | Final Round AI async payment-account deletion, Stripe idempotency race handling, RemoteStar display-currency billing hints |
| **Resume / Jobs Platforms** | RemoteStar saved jobs, on-demand job analysis, resume parsing/tailoring/rendering, admin job CRUD |
| **Open Source** | CNCF LitmusChaos contributor, Kubeflow contributor (Trainer/SDK/Website/Pipelines/Notebooks), Linux Foundation mentee |
| **ML / Distributed Training** | Kubeflow Trainer (Megatron-Core Tensor Parallelism notebook + docs), TrainJob CRD lifecycle proposal |
| **Hackathons** | 4th/500 Incubate, 3rd/1200 Sandbox |
| **AWS** | AMX (S3 batch pipeline for 50K+ images) |
| **GCP** | Seeqlo (Cloud Run, Firebase) |
| **RBAC / Auth** | RemoteStar verified-domain admin access, Seeqlo (Firebase RBAC), SIH Portal (3-tier RBAC + JWT) |

---

## 📝 Update Log

| Date | Change |
|---|---|
| 2026-02-24 | Initial creation from `new.tex` + `Profile (1).pdf` |
| 2026-02-24 | Added Kubeflow SDK PR #307 context, missing links, and Clone Wars achievement |
| 2026-02-24 | Added Ascend (Lakshya) project context |
| 2026-04-29 | Expanded Kubeflow open-source section: Trainer Megatron-Core TP work (#3201, #3434), TrainJob CRD lifecycle proposal (#3068), Website user guides (#4348, #4360), SDK ActiveDeadlineSeconds follow-up (#415, #403), Pipelines Helm charts proposal (#12728); added ML/distributed-training cheat-sheet row and Megatron/PyTorch skills |
| 2026-05-29 | Added significant GitHub activity from the last six months: Final Round AI payment/resume/Copilot work, RemoteStar candidate platform backend/frontend/GitOps work, Kubeflow follow-ups, ChatOps tooling, and related skill/cheat-sheet updates |
| 2026-05-29 | Added RemoteStar (March 2026 – Present) and Final Round AI (April 2026 – Present) as current work experience entries |
| 2026-06-26 | Added "Other Public Projects" from public GitHub (wispr, linkedin-ai-reply, EcoMine, pomodoro-discord-bot, AI-Feedback-form) |
| 2026-06-27 | Added June work from FRAI + RemoteStar orgs: FRAI Report V2 backend (frai_js_server#744), report-delivery reliability (interviewai#2452, #2463), report routing/email fixes, scanned-PDF rejection; RemoteStar email-verification-gated signup, achievements bullet editor, saved-jobs transient-error fix, ingress timeout; added Datadog/RabbitMQ/GCS skills |
| 2026-06-27 | Added newly surfaced Kubeflow contributions: merged TrainJob `activeDeadlineSeconds` implementation (trainer#3258), Pipelines Helm-charts KEP (pipelines#12842), and Notebooks dev-guide expansion (notebooks#835); added Notebooks to contributor scope |
| 2026-06-27 | Added missed CNCF upstream items found via full GitHub sweep: merged CloudNativePG chaos-testing#3, and LitmusChaos litmus-go#778 (PreTargetSelection probe-mode feature request) |
| 2026-06-27 | Added Linux Foundation LiFT Scholarship (2026) to Achievements and a Talks entry for the first Kubeflow Commons Hub Pune meetup talk (Megatron-LM tensor parallelism on Kubeflow Trainer) |
| 2026-06-27 | Added LF Mentorship Showcase talk + LFX mentor names and poster/project links to the CloudNativePG entry |

<!-- 
  HOW TO USE:
  1. Read the JD carefully and identify key themes (see cheat sheet above).
  2. Pick the most relevant experiences, projects, and skills.
  3. Reorder sections to match JD priorities (e.g., put Open Source first for CNCF roles).
  4. Adjust bullet points to emphasize matching keywords and metrics.
  5. Keep to 1 page, drop least relevant items.
  6. Update this master_profile.md whenever you learn something new!
-->
