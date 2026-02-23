# 인프라 및 배포 정책 (Infrastructure & Deployment Policy)

## 1. 스테이징 환경 (Staging Environments)

Quiet Chatter는 안정적인 개발과 운영을 위해 두 개의 독립된 환경을 유지합니다.

| 환경 | 프론트엔드 도메인 | 백엔드 API 도메인 | 목적 | 배포 트리거 |
| :--- | :--- | :--- | :--- | :--- |
| **운영 (Production)** | `https://quiet-chatter.com` | `https://api.quiet-chatter.com` | 실제 사용자에게 서비스 제공 | `prod` 브랜치로 Push/Merge |
| **개발 (Development)** | `https://dev.quiet-chatter.com` | `https://dev-api.quiet-chatter.com` | 신규 기능 테스트 및 통합 | `dev` 브랜치로 Push/Merge |

### 1.1 데이터베이스 정책
- **운영 (Production)**: 전용 PostgreSQL 인스턴스와 Redis DB 0번을 사용합니다.
- **개발 (Development)**: 별도의 PostgreSQL 인스턴스(또는 스키마)와 Redis DB 1번을 사용하여 데이터를 격리합니다.

---

## 2. CI/CD 파이프라인

이 프로젝트는 **GitHub Actions**를 사용하여 지속적 통합(CI) 및 지속적 배포(CD)를 수행합니다.

### 2.1 백엔드 파이프라인 (AWS LightSail + Docker)
- **CI (빌드 및 테스트)**: `dev` 또는 `main` 브랜치에 대한 Pull Request 시 트리거됩니다. Gradle 빌드 및 테스트를 실행합니다.
- **CD (배포)**:
    1. **이미지 빌드**: Docker 이미지를 생성합니다 (`maskun2/quiet-chatter` 또는 `quiet-chatter-dev`).
    2. **레지스트리 푸시**: 생성된 이미지를 Docker Hub에 푸시합니다.
    3. **Watchtower**: AWS 서버에서 실행 중인 Watchtower 컨테이너가 새로운 이미지를 감지하고, 애플리케이션 컨테이너를 자동으로 재시작합니다.

### 2.2 프론트엔드 파이프라인 (Cloudflare Pages)
- **CI/CD**: Cloudflare Pages가 리포지토리의 변경 사항을 자동으로 감지합니다.
- **운영 (Production)**: `main` 브랜치 변경 시 운영 도메인에 배포됩니다.
- **개발 (Development)**: `dev` 브랜치 변경 시 개발 도메인에 배포됩니다.

---

## 3. 배포 관리 (Release Management)

우리는 자동화 도구를 사용하여 **Semantic Versioning** (Major.Minor.Patch)을 따릅니다.

- **도구**: `semantic-release`
- **워크플로우**:
    1. 개발자는 **Conventional Commits** 규칙을 준수하여 커밋합니다 (예: `feat: allow book search`, `fix: correct typo`).
    2. 코드가 `main` 브랜치에 병합되면 릴리즈 워크플로우가 실행됩니다.
    3. 커밋 메시지를 분석하여 버전 업그레이드 단계를 결정합니다.
    4. 자동으로 `package.json` / `build.gradle`의 버전을 업데이트하고, `CHANGELOG.md`를 생성하며, Git 태그 및 GitHub Release를 발행합니다.
