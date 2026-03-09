# 2-Stage Review Checklist

## Stage 1 — SPEC COMPLIANCE

```
1. CODE ĐÚNG SPEC?
   - Tất cả requirements trong SPEC.md đã implement?
   - Scenarios trong spec đã handle?

2. CODE THỪA?
   - Có code/feature nào KHÔNG ở trong spec?
   - Nếu có → xóa hoặc tạo change mới

3. BEHAVIOR KHỚP?
   - Behavior thực tế matches expected behavior?
   - Edge cases handled?
```

## Stage 2 — CODE QUALITY (7 tiêu chí)

| # | Tiêu chí | Kiểm tra |
|---|---|---|
| ① | **Bugs & Logic** | Null pointer, edge cases, infinite loop, logic đúng? |
| ② | **UI/UX** | Responsive, contrast, ARIA, loading/empty/error states? |
| ③ | **Security** | SQL injection, XSS, CSRF, hardcoded secrets? |
| ④ | **Performance** | N+1 query, missing index, heavy computation? |
| ⑤ | **Code Quality** | Magic numbers, DRY, YAGNI, naming, types? |
| ⑥ | **Test Coverage** | Tests viết trước code (TDD)? All pass? Evidence? |
| ⑦ | **LESSONS Compliance** | Code mới có vi phạm bài học cũ? |

## LESSONS Entry Template

```markdown
### [🔴|🟡|🟢] #WARN-NNN — Tiêu đề (YYYY-MM-DD)

- **Mức độ:** 🔴 Critical | 🟡 Warning | 🟢 Info
- **Thẻ:** #tag1, #tag2
- **Triệu chứng:** [mô tả lỗi]
- **Nguyên nhân gốc:** [WHY]
- **Cách fix:**
  // ✅ Đúng: [code]
  // ❌ Sai: [code]
- **Quy tắc rút ra:** [Rule]
- **File liên quan:** `path/to/file`
```

## Context Load Order

| File | Trích xuất |
|---|---|
| `GEMINI.md` | Version, tech stack, Architecture Rules |
| `LESSONS.md` | ⚠️ Warnings liên quan task |
| `CHANGE_LOG.md` | Phiên trước: tóm tắt 1 dòng |
| `NEXT-TODO.md` | Top 3 task ưu tiên |
| `STATE.md` | Phase/wave đang dở (nếu có) |
| `PROJECT-META.md` | Owner, status, URLs |
| `SPEC.md` | Yêu cầu nghiệp vụ tổng thể |
| `DEPLOYMENT.md` | Deployment status |

> File không tồn tại → bỏ qua, không báo lỗi.
