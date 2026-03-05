---
name: Brainstorm — Ideation to Prototype Pipeline
description: "Quy trình sáng tạo có hệ thống: từ ý tưởng mơ hồ → phân tích → spec rõ ràng → prototype/wireframe. Dùng khi brainstorm tính năng, lên ý tưởng sản phẩm, thiết kế luồng UX, hoặc cần tư duy sáng tạo có cấu trúc. Kích hoạt bằng /brainstorm [idea]."
---

# /brainstorm [idea or problem]

> **Ideation Pipeline** — Biến ý tưởng mơ hồ thành spec actionable
> Mục tiêu: Không bao giờ code mà chưa nghĩ kỹ

---

## KHI NÀO DÙNG

| Tình huống | Dùng /brainstorm? |
|---|---|
| "Tôi muốn làm tính năng X nhưng chưa rõ" | ✅ Có |
| "Có ý tưởng sản phẩm mới, cần phân tích" | ✅ Có |
| "Cần so sánh nhiều approaches" | ✅ Có |
| "Bug cần fix" | ❌ → /fix |
| "Feature đã rõ, cần blueprint" | ❌ → /plan |

---

## QUY TRÌNH 5 BƯỚC

### Step 1 — CAPTURE (Thu thập raw idea)
```
1. User mô tả ý tưởng (có thể mơ hồ, lộn xộn — OK)
2. AI ghi lại CHÍNH XÁC lời user (không interpret sai)
3. Hỏi 3 câu hỏi mở:
   - "Vấn đề gốc mà bạn muốn giải quyết là gì?"
   - "Ai là người dùng chính? Họ đang làm gì khi cần feature này?"
   - "Kết quả lý tưởng nhất trong đầu bạn trông như thế nào?"
```

### Step 2 — EXPAND (Mở rộng khả năng)
```
Brainstorm 3-5 approaches khác nhau:

| # | Approach | Ưu điểm | Nhược điểm | Effort |
|---|---|---|---|---|
| A | [approach 1] | [pros] | [cons] | S/M/L |
| B | [approach 2] | [pros] | [cons] | S/M/L |
| C | [approach 3] | [pros] | [cons] | S/M/L |

Kỹ thuật mở rộng:
- "Nếu không có giới hạn kỹ thuật, bạn sẽ làm gì?"
- "Đối thủ / product tương tự đang giải quyết thế nào?"
- "MVP nhỏ nhất mà vẫn giải quyết core problem là gì?"
- "Nếu chỉ có 1 ngày để ship, bạn cắt gì?"
```

### Step 3 — ANALYZE (Phân tích depth)
```
Cho approach được chọn (hoặc top 2):

1. User Stories:
   "Là [role], tôi muốn [action], để [benefit]"

2. Technical Feasibility:
   - Tech stack hiện tại support không?
   - Cần thêm dependency/service nào?
   - Có bottleneck performance nào?

3. Risk Assessment:
   | Risk | Xác suất | Impact | Mitigation |
   |---|---|---|---|
   | [risk] | High/Med/Low | High/Med/Low | [plan] |

4. Dependencies:
   - Cần API bên ngoài? (cost, rate limit, reliability)
   - Cần data nào chưa có?
   - Cần approval từ ai?
```

### Step 4 — SPEC (Output rõ ràng)
```
Output có cấu trúc, sẵn sàng cho /plan hoặc /build:

## Feature: [Tên Feature]

### Problem Statement
[1-2 câu mô tả vấn đề]

### Proposed Solution
[Approach được chọn, 3-5 câu]

### User Stories (ưu tiên)
1. [P0] Là X, tôi muốn Y, để Z
2. [P1] ...
3. [P2] ...

### Acceptance Criteria
- [ ] [Criterion đo lường được]
- [ ] [Criterion đo lường được]

### Technical Notes
- Architecture: [impact]
- New dependencies: [list]
- Migration needed: [yes/no]

### Effort Estimate
- Size: S/M/L
- Files: ~N files
- Suggested approach: /plan → /build (hoặc /build trực tiếp)

### Open Questions
- [Câu hỏi chưa trả lời được]
```

### Step 5 — PROTOTYPE (Optional — nếu cần visual)
```
Nếu feature có UI:
1. ASCII wireframe hoặc mô tả layout chi tiết
2. User flow diagram (text-based):
   [Màn hình A] → click [Button] → [Màn hình B] → submit → [Result]
3. Component list: những UI components cần tạo/sửa

Nếu feature là backend:
1. API design sketch: METHOD /path → request → response
2. Data flow diagram
3. Sequence diagram (text-based)
```

---

## OUTPUT FORMAT

```
🧠 BRAINSTORM REPORT — [idea]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📌 PROBLEM: [1 câu]
💡 APPROACHES: [N approaches analyzed]
✅ CHOSEN: Approach [X] — [lý do]
📋 SPEC: [Ready for /plan or /build]

🔜 NEXT STEP:
  Feature phức tạp? → /plan [feature name] (tạo change folder: proposal + specs + tasks)
  Feature đơn giản? → /build [feature name] (TDD: RED→GREEN→REFACTOR)
  Cần design UI?    → /design [feature name]
```

---

## QUY TẮC

- KHÔNG nhảy vào solution ngay — explore problem space trước
- LUÔN đưa ra ≥ 3 approaches (tránh tunnel vision)
- Effort estimate phải thực tế (không optimistic bias)
- Open Questions là OK — tốt hơn giả định sai
- Output PHẢI actionable: ai đọc xong cũng hiểu phải làm gì
