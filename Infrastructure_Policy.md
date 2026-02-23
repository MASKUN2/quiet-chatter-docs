# Infrastructure & Deployment Policy

## 1. Staging Environments

Quiet Chatter maintains two distinct environments for development and production stability.

| Environment | Frontend Domain | Backend API Domain | Purpose | Deployment Trigger |
| :--- | :--- | :--- | :--- | :--- |
| **Production** | `https://quiet-chatter.com` | `https://api.quiet-chatter.com` | Live service for end-users. | Push/Merge to `prod` branch |
| **Development** | `https://dev.quiet-chatter.com` | `https://dev-api.quiet-chatter.com` | Testing new features and integrations. | Push/Merge to `dev` branch |

### 1.1 Database Policy
- **Production**: Uses a dedicated PostgreSQL instance and Redis DB 0.
- **Development**: Uses a separate PostgreSQL instance (or schema) and Redis DB 1 to isolate data.

---

## 2. CI/CD Pipeline

The project uses **GitHub Actions** for Continuous Integration and Continuous Deployment.

### 2.1 Backend Pipeline (AWS LightSail + Docker)
- **CI (Build & Test)**: Triggered on Pull Requests to `dev` or `main`. Runs Gradle build and tests.
- **CD (Deployment)**:
    1. **Build Image**: Creates a Docker image (`maskun2/quiet-chatter` or `quiet-chatter-dev`).
    2. **Push Registry**: Pushes the image to Docker Hub.
    3. **Watchtower**: A Watchtower container on the AWS server detects the new image and automatically restarts the application container.

### 2.2 Frontend Pipeline (Cloudflare Pages)
- **CI/CD**: Cloudflare Pages automatically detects changes in the repository.
- **Production**: Deploys from the `main` branch to the production domain.
- **Development**: Deploys from the `dev` branch to the development domain.

---

## 3. Release Management

We follow **Semantic Versioning** (Major.Minor.Patch) using automated tools.

- **Tool**: `semantic-release`
- **Workflow**:
    1. Developers use **Conventional Commits** (e.g., `feat: allow book search`, `fix: correct typo`).
    2. When code is merged into `main`, the release workflow runs.
    3. It analyzes commits to determine the version bump.
    4. Automatically updates `package.json` / `build.gradle`, generates `CHANGELOG.md`, creates a Git tag, and publishes a GitHub Release.

---

## 4. Local Development

### 4.1 Backend
- Run with `./gradlew bootRun`.
- Requires local PostgreSQL and Redis (can be run via `docker-compose up`).

### 4.2 Frontend
- Run with `npm run dev`.
- **Proxy**: Requests to `/api` are proxied to `https://api.quiet-chatter.com` (or `dev-api` depending on config) to avoid CORS issues during development.
- **Mocking**: Run `npm run dev:mock` to use MSW (Mock Service Worker) for independent UI development without a backend.
