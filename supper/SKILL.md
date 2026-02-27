---
name: Supper Skill Selector
description: "Công cụ giúp lựa chọn skill tốt nhất trong số 25 skill của hệ thống dựa trên ngữ cảnh, lịch sử làm việc và từ khóa của người dùng. Kích hoạt bằng lệnh /supper."
---

# HƯỚNG DẪN KỸ NĂNG: SUPPER SKILL SELECTOR

## Mục tiêu
Phân tích yêu cầu → chọn đúng 1-3 skills → đưa ra combo tối ưu + prompt mẫu + context cần chuẩn bị. Không để người dùng mất thời gian dùng sai skill.

---

## DANH SÁCH 25 SKILLS (Phân loại theo lĩnh vực)

### 🖥️ VIBE CODE — Phát triển phần mềm
| # | Skill | Dùng khi |
|---|---|---|
| 1 | `vibe-code` | Bất kỳ task code nào — quy trình chuẩn hóa |
| 2 | `typescript-pro` | TypeScript, generic types, strict typing |
| 3 | `nextjs-app-router-patterns` | Next.js 14+, SSR, RSC, App Router |
| 4 | `react-modernization` | React hooks, state, component architecture |
| 5 | `nodejs-backend-patterns` | Node.js, Express, REST API |
| 6 | `api-design-principles` | Thiết kế API chuẩn REST/GraphQL/Webhook |
| 7 | `frontend-mobile-development-component-scaffold` | Scaffold UI component nhanh |
| 8 | `graphql-architect` | GraphQL schema, resolver, federation |
| 9 | `payment-integration` | Stripe, PayPal, thanh toán online |

### 🎨 DESIGN — Website & Brand
| # | Skill | Dùng khi |
|---|---|---|
| 10 | `design-workflow` | Trang web mới, redesign UI, logo/brand |
| 11 | `tailwind-design-system` | Design system, Tailwind CSS, dark mode |
| 12 | `ui-visual-validator` | Verify UI sau thay đổi, visual regression |
| 13 | `seo-structure-architect` | SEO, schema markup, heading structure |

### 🔗 N8N & AUTOMATION
| # | Skill | Dùng khi |
|---|---|---|
| 14 | `n8n-workflow` | N8N integration, webhook, automation flow |

### 🐛 DEBUG & QUALITY
| # | Skill | Dùng khi |
|---|---|---|
| 15 | `debugger` | Lỗi, exception, stack trace, unexpected behavior |
| 16 | `bug-memory-journal` | Trước khi code, sau fix bug, đọc LESSONS.md |
| 17 | `security-auditor` | Kiểm tra bảo mật, OWASP, audit trước deploy |
| 18 | `performance-engineer` | Tối ưu tốc độ, Core Web Vitals, N+1 query |
| 19 | `e2e-testing-patterns` | Playwright, Cypress, viết test suite |

### 📦 DEVOPS & INFRASTRUCTURE
| # | Skill | Dùng khi |
|---|---|---|
| 20 | `git-advanced-workflows` | Rebase, cherry-pick, merge conflict, reflog |

### 📋 SESSION MANAGEMENT
| # | Skill | Dùng khi |
|---|---|---|
| 21 | `start` | `/start` — khởi động phiên làm việc |
| 22 | `save` | `/save` — kết thúc phiên, push GitHub |
| 23 | `review` | `/review` — kiểm tra code trước save |
| 24 | `gsd` | Dự án mới, feature lớn cần plan GSD |
| 25 | `supper` | (Chính skill này) — chọn skill phù hợp |
| 26 | `memory-optimizer` | `/checkpoint` `/recall` `/memory` — quản lý working memory |

---

## COMBO SKILLS HIỆU QUẢ NHẤT

| Tình huống | Combo (theo thứ tự) |
|---|---|
| Feature code mới | `bug-memory-journal` → `vibe-code` → `review` |
| Fix bug | `bug-memory-journal` → `debugger` → `vibe-code` |
| API endpoint mới | `api-design-principles` → `vibe-code` → `security-auditor` |
| UI/Landing page mới | `design-workflow` → `tailwind-design-system` → `ui-visual-validator` |
| Logo/Brand | `design-workflow` (generate_image) → `tailwind-design-system` |
| N8N webhook | `api-design-principles` → `n8n-workflow` → `security-auditor` |
| Deploy/Release | `performance-engineer` → `security-auditor` → `review` → `save` |
| Dự án mới lớn | `gsd` → `/new-project` → `vibe-code` |
| Next.js feature | `bug-memory-journal` → `nextjs-app-router-patterns` → `typescript-pro` |
| Tối ưu performance | `performance-engineer` → `debugger` (nếu có N+1) |

---

## QUY TRÌNH THỰC HIỆN KHI ĐƯỢC GỌI

### Bước 1 — Phân tích yêu cầu
```
Đọc câu hỏi/yêu cầu của user → xác định:
- Đây là task gì? (code / design / debug / n8n / deploy)
- Framework/ngôn ngữ? (Laravel/PHP, Next.js/TS, Node.js)
- Mức độ phức tạp? (quick fix / feature / project)
- Có bug liên quan không? → luôn thêm bug-memory-journal
```

### Bước 2 — Chọn Skills & Combo

```
Rule quan trọng:
🔴 Debug/fix bug → LUÔN bắt đầu bằng bug-memory-journal + debugger
🎨 Design task → LUÔN bắt đầu bằng design-workflow
🔗 N8N task → LUÔN có security-auditor trong combo
💻 Code task → LUÔN bắt đầu bằng bug-memory-journal (đọc lessons cũ)
🚀 Dự án mới → LUÔN gợi ý gsd trước
```

### Bước 3 — Phản hồi theo format này

```
🎯 SUPPER ANALYSIS: [Loại task]

📦 SKILLS ĐƯỢC CHỌN:
  1️⃣ [skill-name] — Lý do chọn ngắn gọn
  2️⃣ [skill-name] — Lý do chọn ngắn gọn
  3️⃣ [skill-name] — Lý do chọn ngắn gọn (nếu cần)

🔄 COMBO THỰC HIỆN:
  [skill-1] → [skill-2] → [skill-3]
  (Giải thích: tại sao thứ tự này)

📋 CONTEXT CẦN CHUẨN BỊ:
  - Đọc file: [LESSONS.md / GEMINI.md / ...]
  - Biết rõ: [tech stack / existing code / requirements]

💡 PROMPT MẪU ĐỂ BẮT ĐẦU:
  "[Prompt cụ thể người dùng có thể copy-paste để bắt đầu task]"
```

---

## QUY TẮC QUAN TRỌNG
- CHỈ đề xuất skill — KHÔNG code ngay tại bước này
- Giữ phản hồi ngắn gọn — không giải thích dài dòng
- Bug-related task → **LUÔN** đề xuất `bug-memory-journal` + `debugger`
- N8N task → **LUÔN** đề xuất `security-auditor` trong combo
- Code task → **LUÔN** đề xuất đọc LESSONS.md trước
