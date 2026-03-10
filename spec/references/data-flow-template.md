# Data Flow Template

> Sử dụng Mermaid diagrams cho luồng dữ liệu.

```markdown
# 🔄 Data Flow — [Project Name]
> Updated: YYYY-MM-DD

## 1. [Core Flow Name]

```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend
    participant API as API Server
    participant DB as Database
    participant Q as Queue

    User->>FE: Submit form
    FE->>API: POST /api/v1/resource
    API->>API: Validate input
    API->>DB: INSERT record
    API->>Q: Dispatch notification job
    API-->>FE: 201 Created
    Q->>ExternalAPI: Send notification
```

## 2. [Authentication Flow]

```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend
    participant API as API Server
    participant DB as Database

    User->>FE: Enter credentials
    FE->>API: POST /auth/login
    API->>DB: Verify credentials
    alt Valid
        API-->>FE: JWT + Refresh token
        FE->>FE: Store in httpOnly cookie
    else Invalid
        API-->>FE: 401 Unauthorized
    end
```

## 3. [State Diagram Example]

```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Pending: Submit
    Pending --> Approved: Approve
    Pending --> Rejected: Reject
    Rejected --> Draft: Revise
    Approved --> [*]
```

## N. System Overview

```mermaid
graph TB
    subgraph Frontend
        A[Next.js App]
    end
    subgraph Backend
        B[API Server]
        C[Queue Worker]
    end
    subgraph Data
        D[(PostgreSQL)]
        E[(Redis Cache)]
    end
    subgraph External
        F[Payment API]
        G[Email Service]
    end

    A -->|REST API| B
    B --> D
    B --> E
    B -->|Dispatch| C
    C --> F
    C --> G
```
```
