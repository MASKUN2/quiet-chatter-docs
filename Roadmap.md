# Future Roadmap & Ideas

This document outlines the vision for the future of Quiet Chatter, including technical improvements, feature expansions, and better collaboration strategies.

## 1. Frontend-Backend Collaboration & Productivity

### 1.1 Automated Type Sync (Done ✅)
- **Goal**: Eliminate manual type definitions on the frontend.
- **Solution**: Use `openapi-typescript` to generate TypeScript interfaces directly from the backend's OpenAPI JSON spec.
- **Workflow**: `Backend Change -> CI/CD Spec Update -> Frontend 'npm run gen:types' -> Compile Error -> Fix`.

### 1.2 Visual Documentation (In Progress 🚧)
- **Goal**: Provide a human-readable API reference.
- **Tool**: Swagger UI or Redoc hosted at `/docs` or a separate subdomain.
- **Benefit**: Easier onboarding for new developers and better communication between teams.

### 1.3 Breaking Change Detection (Planned 📅)
- **Goal**: Prevent API changes from breaking the frontend build.
- **Tool**: `openapi-diff` or `oasdiff` in the CI pipeline.
- **Action**: If a breaking change is detected (e.g., field removal), fail the build or post a warning comment on the PR.

---

## 2. Technical Improvements

### 2.1 Performance Optimization
- **Goal**: Improve initial load time and search responsiveness.
- **Plan**:
    - Implement **Server-Side Rendering (SSR)** or Static Site Generation (SSG) for SEO and faster First Contentful Paint (FCP).
    - Introduce **CDN caching** for static assets and API responses where appropriate.
    - Optimize database queries for complex aggregations (e.g., "Most Popular Talks").

### 2.2 Scalability
- **Goal**: Handle increased traffic and data volume.
- **Plan**:
    - **Database Sharding/Partitioning**: As the `Talk` table grows, partition it by date (since older talks are hidden).
    - **Message Queue**: Introduce RabbitMQ or Kafka for asynchronous processing of "Like" events and notifications.

---

## 3. Feature Expansion

### 3.1 Mobile Application
- **Goal**: Reach users on their preferred devices.
- **Plan**: Develop a mobile app using **React Native** or **Flutter**, leveraging the existing REST API.

### 3.2 Personalized Recommendations
- **Goal**: Move beyond random recommendations.
- **Plan**: Implement collaborative filtering or content-based recommendation algorithms to suggest books based on a user's reading history and "Like" patterns.

### 3.3 Enhanced "Quiet" Interactions
- **Goal**: Add more ways to interact without breaking the "quiet" philosophy.
- **Idea**: Allow users to send predefined "stickers" or "moods" as reactions, strictly without text comments.
