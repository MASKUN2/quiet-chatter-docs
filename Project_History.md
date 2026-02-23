# Project History

Quiet Chatter: You Belong Here.
This document tracks the journey of the Quiet Chatter project from its inception to the present.

## 2025

### November: The Beginning (Woowa Tech Course Open Mission)
- **2025-11-08**: **Initial Infrastructure Setup**. Started as an anonymous book review SNS. Chose Java 21, Spring Boot, PostgreSQL, Thymeleaf for rapid prototyping. Deployed on AWS LightSail with GitHub Actions CI/CD.
- **2025-11-12**: **Testing Strategy**. Improved tests for external APIs (Naver Book Search) using `@RestClientTest` and `Instancio`.
- **2025-11-15**: **Authentication Design**. Implemented custom session-based auth to support anonymous users ("Guest").
- **2025-11-18**: **Performance Optimization**. Introduced "Delayed Write" strategy for Likes using background threads and JDBC Batch Update to handle high concurrency.
- **2025-11-23**: **Refactoring**. Applied rate limiting (Nginx), reorganized packages by domain, and implemented caching for recommendations.

---

## 2026

### January: Transition to Modern Stack & Collaboration
- **2026-01-20**: **Frontend Separation**. Decided to split the monolithic Thymeleaf app into a RESTful API Backend and a React Frontend (TypeScript + Vite) for better UX and maintainability.
- **2026-01-21**: **Auth Overhaul**. Switched from Session-based auth to **JWT (Stateless)** for mobile compatibility. Introduced Redis for token management.
- **2026-01-25**: **Domain Simplification**. Removed complex Value Objects for performance. Implemented the core **"Auto-Hidden" policy** (1-year expiration) using scheduled bulk updates.
- **2026-01-26**: **Soft Delete & History**. Implemented "Soft Delete" (hiding data instead of removing it) and tracking modification times.
- **2026-01-29**: **Infinite Scroll**. Added infinite scroll support for book search APIs (Backend) and implemented it on the Frontend using Intersection Observer.
- **2026-01-30**: **API Documentation Automation**. Integrated Spring Rest Docs + Swagger (OpenAPI) to auto-generate API specs. Frontend set up `openapi-typescript` to auto-sync types.

### February: Stabilization & Feature Expansion
- **2026-02-01**: **Staging Environments**. Established separate `dev` and `prod` environments with distinct domains and databases.
- **2026-02-10**: **Talk CRUD**. Completed full Create/Read/Update/Delete flow for Book Talks on the frontend.
- **2026-02-15**: **Reaction UI**. Implemented "Like" and "Empathy" buttons with Optimistic Updates for instant feedback.
- **2026-02-20**: **Automated Release Management**. Transitioned to **Semantic Release** based on Conventional Commits for automated versioning and changelog generation across both repositories.
