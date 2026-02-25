# AI Agent Collaboration Task

## 1. Common Instructions

### Document Management Principles
1. **Dynamic Sections (Top)**: Sections `2. Developer's Request` and `3. Agent's Response` should be **continuously updated** to reflect the current status.
    - **Timestamp Requirement**: Whenever you update these sections, you **MUST** update the timestamp header using the format `[Last Updated: YYYY-MM-DD HH:mm]`.
2. **Static History (Bottom)**: Section `4. History` is strictly **Append-Only**. Summarize key decisions and completed tasks here. **Do not modify or delete** existing history.

### Working Procedure
1. **One Clear Path**: When creating a plan, do not offer multiple choices like "A or B". Specify **one clear implementation path** based on project conventions and requirements, and explain your reasoning clearly.
2. **Review Request**: If you need a decision or review from the developer, explicitly state **[Review Request]** at the end of the `3. Agent's Response` section and pause your work.
3. **Reference First**: Always check `AGENTS.md` and the guides in `docs/guide/` before starting any task.
4. **Context Update**: When a task is completed, summarize the `3. Agent's Response` content into `4. History`, and update `2. Developer's Request` with the next step.

---

## 2. Developer's Request
### [Last Updated: 2026-02-25 16:45]

### The core goal of this project
- We are developing the quiet-chatter service.
- This project (`quiet-chatter-docs`) is the documentation repository.
- The purpose of this project is to clarify requirements and handle the document-based workflow leading up to actual feature implementation.

### [Current Objective]
- Implement My Page and Account Management Features.
- Detailed requirements are defined in the Agent's Response section below.

---

## 3. Agent's Response
### [Last Updated: 2026-02-25 16:45]

### [Current Status] : Ready for Implementation

### [Analysis & Plan]

#### 1. Final API Specification (Security Enhanced)

**1.1. Auth & Account Management**

- **POST /v1/auth/login/naver** (Deactivated Account Handling)
  - Condition: Login attempt with a `DEACTIVATED` account.
  - Response: `401 Unauthorized`
  - Body (RFC 7807):
    - Replaced `memberId` with `reactivationToken`.
    - `reactivationToken` valid for **2 hours** (TTL).
    ```json
    {
      "type": "/errors/member-deactivated",
      "status": 401,
      "detail": "Your account is currently deactivated. Please reactivate your account to use the service.",
      "reactivationToken": "eyJhbGciOiJIUzI1NiJ9..." 
    }
    ```

- **GET /errors/member-deactivated**
  - Description: Provides error details and resolution for frontend modals.
  - Response: `200 OK`
    ```json
    {
      "title": "Member Deactivated",
      "detail": "Your account has been deactivated.",
      "resolution": "If you want to reactivate your account, please click the 'Reactivate' button in the popup."
    }
    ```

- **POST /v1/auth/reactivate**
  - Description: Reactivates a deactivated member using the token.
  - Request: `{ "token": "eyJhbGciOiJIUzI1NiJ9..." }`
  - Response:
    - **200 OK**: Success (Set-Cookie).
    - **401 Unauthorized**: Token expired or invalid.
      - Body (RFC 7807):
      ```json
      {
        "type": "/errors/token-expired",
        "title": "Reactivation Token Expired",
        "status": 401,
        "detail": "Reactivation token has expired. Please log in again to receive a new token.",
        "instance": "/v1/auth/reactivate"
      }
      ```

**1.2. My Page**

- **GET /v1/me/talks**
  - Auth: JWT Required
  - Response: `200 OK` (Pagination)
    ```json
    {
      "content": [
        {
          "id": "uuid",
          "content": "string",
          "book": { "id": "uuid", "title": "string", "author": "string", "cover": "string" },
          "createdAt": "ISO-8601",
          "dateToHidden": "ISO-8601",
          "likeCount": 10,
          "supportCount": 5
        }
      ],
      "page": { ... }
    }
    ```

- **PUT /v1/me/profile**
  - Request: `{ "nickname": "new_nickname" }`
  - Response: `204 No Content`

- **DELETE /v1/me**
  - Description: Change status to `DEACTIVATED`, hide all talks, invalidate session.
  - Response: `204 No Content`

#### 2. Implementation Roadmap

**Phase 1: Backend Domain & Exception Handling (quiet-chatter)**
1.  **Member Domain**: Add `status` change logic (`activate`, `deactivate`) and `updateNickname` method to `Member` entity.
2.  **AuthTokenService**: Add logic to generate `reactivationToken` (TTL: 2 hours) and verify/throw expiration exception.
3.  **MemberDeactivatedException**: Update fields to include `reactivationToken` instead of `memberId`.
4.  **WebExceptionHandler**: Handle `MemberDeactivatedException` (return token) and `TokenExpiredException` (return 401 ProblemDetail).
5.  **AuthMemberService**: Throw exception with token if member status is `DEACTIVATED` during login.

**Phase 2: Backend Business Logic & API (quiet-chatter)**
1.  **MeApi**: Implement `/v1/me/talks`, `/v1/me/profile`, `/v1/me` (Call `TalkCommandService.hideAllByMember` on withdrawal).
2.  **AuthReactivateApi**: Implement member reactivation via token verification and new session issuance.
3.  **Error Controller**: Implement controller to serve static info for `/errors/{code}`.
4.  **Documentation**: API testing and documentation using RestDocs.

**Phase 3: Frontend Auth Flow & Modal (quiet-chatter-front-end)**
1.  **api.ts**: Update `loginWithNaver` error handling (store token) and add `reactivateAccount(token)` function.
2.  **ReactivationModal**: Update to use token for reactivation request. Handle token expiration error (alert user to re-login).
3.  **AuthContext**: Supplement logic for auto-login and state update after successful reactivation.

**Phase 4: My Page UI & Integration (quiet-chatter-front-end)**
1.  **MyPage.tsx**: Implement "My Talks" list page using MUI.
2.  **Profile & Withdrawal**: Implement profile update and withdrawal UI (including confirmation dialog).
3.  **Header**: Update header to show "My Page" link when logged in.

### [Questions or Issues]
- None.

---

## 4. History

### [2026-02-25 15:55] Task Initialization
- Created task document based on `마이페이지 인터페이스 설계.md` and `마이페이지 및 계정 관리 기능 구현 계획.md`.
- Finalized API specifications and implementation roadmap.

### [2026-02-25 16:35] Security Enhancement (Updated)
- Replaced `memberId` with `reactivationToken` for secure reactivation.
- Set token **TTL to 2 hours**.
- Added **Token Expired (401)** handling for `/v1/auth/reactivate`.
