# Quiet Chatter: You Belong Here

<img alt="Quiet Chatter Logo" height="200" src="https://quiet-chatter.com/images/quiet-chatter-icon2.png" width="200"/>

> **"수줍음이 많은 사람들을 위한 독서 SNS"**
>
> Quiet Chatter는 내향적인 독자들이 책을 통해 서로의 생각에 조용히 공감할 수 있는 공간입니다.
> 익명성과 휘발성을 통해 기록에 대한 부담을 덜고, 오직 '공감'만으로 소통하는 따뜻한 커뮤니티를 지향합니다.

## 🔗 주요 링크

| 구분 | Production (운영) | Development (개발) |
| :--- | :--- | :--- |
| **서비스 URL** | [https://quiet-chatter.com](https://quiet-chatter.com) | [https://dev.quiet-chatter.com](https://dev.quiet-chatter.com) |
| **API 문서** | [https://api.quiet-chatter.com/docs](https://api.quiet-chatter.com/docs) | [https://dev-api.quiet-chatter.com/docs](https://dev-api.quiet-chatter.com/docs) |

## 📚 프로젝트 문서 (Documentation)

이 리포지토리(`quiet-chatter-docs`)는 프로젝트의 기획, 정책, 히스토리를 관리하는 **Single Source of Truth**입니다.

- **[📖 서비스 기획서 (Service Specification)](service_specification.md)**
    - 서비스 개요, 핵심 가치, 기능 명세, 운영 정책 (기획자/비개발자 권장)
- **[🏗 인프라 및 배포 정책 (Infrastructure Policy)](infrastructure_policy.md)**
    - Staging 환경, CI/CD 파이프라인, 배포 프로세스 (개발자/DevOps 권장)
- **[📜 프로젝트 연혁 (History)](project_history.md)**
    - 프로젝트의 시작부터 현재까지의 주요 의사결정 및 개발 이력
- **[🚀 로드맵 (Roadmap)](roadmap.md)**
    - 향후 개발 예정 기능 및 개선 아이디어

## 🛠 기술 스택 (Tech Stack)

### Backend (`quiet-chatter`)
- **Language**: Java 21
- **Framework**: Spring Boot 3.x
- **Database**: PostgreSQL, Redis
- **Architecture**: Hexagonal Architecture

### Frontend (`quiet-chatter-front-end`)
- **Language**: TypeScript
- **Framework**: React 19, Vite
- **UI Library**: Material UI (MUI) v6
- **State Management**: Context API, TanStack Query (Planned)

## 👥 팀

- **개발**: 정인호
- **기획**: 신정원
---
© 2026 Quiet Chatter Team. All rights reserved.
