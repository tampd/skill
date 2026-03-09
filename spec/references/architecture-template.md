# Architecture Documentation Template

## ARCHITECTURE.md — 10 Sections

```markdown
# 🏗️ ARCHITECTURE.md — [Project Name]
> **Version**: vX.Y.Z | **Updated**: YYYY-MM-DD | **Framework**: [stack]

## 1. Tổng quan hệ thống
[Mô tả 3-5 dòng: hệ thống làm gì, cho ai, tính năng chính]

## 2. Infrastructure
| Component | Thông tin |
|---|---|
| Domain | [url] |
| Server | [IP/Cloud] |
| Database | [type + version] |
| Framework | [name + version] |

## 3. Kiến trúc tổng thể
[ASCII diagram hoặc mermaid: Request → Server → App → DB]

## 4. RBAC — Phân quyền
[Bảng roles + quyền hạn]

## 5. Service Layer
[Mô tả từng service chính: input → process → output]

## 6. Database Schema
[Nhóm bảng + mô tả + quan hệ quan trọng]

## 7. Module Map — Controllers
[Bảng controllers: name, route prefix, roles required]

## 8. Core Business Flow
[Sequence diagram luồng nghiệp vụ chính]

## 9. Config System
[Config keys quan trọng + override mechanism]

## 10. Security Architecture
[Auth flow + rate limiting + input validation]
```

## DATA_FLOW.md Template

```markdown
# 🔄 Data Flow — [Project Name]

## 1. [Core Flow Name]
[mermaid flowchart diagram]

## 2. [Secondary Flow Name]
[mermaid stateDiagram]

## N. Sơ đồ Tổng hợp
[mermaid flowchart TB with subgraphs]
```

## SETUP.md Template

```markdown
# ⚙️ SETUP — [Project Name]
> Hướng dẫn cài đặt môi trường development

## Prerequisites
- Node.js v20+
- Database: [type + version]

## Quick Start
[Numbered steps: clone → install → env → migrate → run]

## Environment Variables
[Bảng: key, description, required, default]

## Common Commands
[dev server, test, build, lint, migrate]
```
