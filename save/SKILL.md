---
name: Save — Đóng gói & Xuất bản Production Code
description: "Kết thúc phiên làm việc toàn diện: review 7 tiêu chí, ghi LESSONS, cập nhật docs, atomic commit, push. Thay thế review + save + checkpoint cũ. Kích hoạt bằng lệnh /save."
---

# /save

> **1 lệnh thay 3** — Gộp review + save + checkpoint cũ
> Mục tiêu: Code đã review → docs cập nhật → committed → pushed → memory archived

---

## QUY TRÌNH 7 BƯỚC

### Bước 1 — SCOPE REVIEW

```bash
git status          # Files đã thay đổi
git diff --stat     # Tổng quan changes
```

Liệt kê files thay đổi, nhóm theo module.

---

### Bước 2 — 7 TIÊU CHÍ REVIEW (Từ review skill — giữ nguyên)

Cho MỖI file thay đổi, kiểm tra:

| # | Tiêu chí | Kiểm tra |
|---|---|---|
| ① | **Bugs & Logic** | Null pointer, edge cases, infinite loop, logic đúng? |
| ② | **UI/UX** | Responsive, contrast, ARIA, loading/empty/error states? |
| ③ | **Security** | SQL injection, XSS, CSRF, hardcoded secrets? |
| ④ | **Performance** | N+1 query, missing index, heavy computation? |
| ⑤ | **Code Quality** | Magic numbers, DRY, naming, function length, strict types? |
| ⑥ | **Verification** | Evidence đã có? (test pass / curl output / screenshot) |
| ⑦ | **LESSONS Compliance** | Code mới có vi phạm bài học cũ trong LESSONS.md? |

**Output format:**
```
① BUGS:          [✅ | ⚠️ N warning | 🛑 N lỗi]
② UI/UX:         [✅ | ⚠️ | 🛑 | N/A]
③ SECURITY:      [✅ | ⚠️ | 🛑]
④ PERFORMANCE:   [✅ | ⚠️ | 🛑]
⑤ CODE QUALITY:  [✅ | ⚠️ | 🛑]
⑥ VERIFICATION:  [✅ Evidence đủ | ❌ Thiếu]
⑦ LESSONS:       [✅ Tuân thủ | 🛑 Vi phạm #WARN-XXX]
```

> 🛑 Nếu có CRITICAL → **DỪNG**, yêu cầu sửa trước khi tiếp tục.

---

### Bước 3 — LESSONS CAPTURE

```
Hỏi: "Phiên này có gặp bug hoặc rút ra bài học gì không?"

NẾU CÓ → Ghi vào LESSONS.md ngay:

### [🔴|🟡|🟢] #WARN-NNN — Tiêu đề (YYYY-MM-DD)

- **Mức độ:** 🔴 Critical | 🟡 Warning | 🟢 Info
- **Thẻ:** #tag1, #tag2 (tối đa 3)
- **Triệu chứng:** Mô tả lỗi quan sát được
- **Nguyên nhân gốc:** WHY
- **Cách fix:** Code snippet cụ thể
- **Quy tắc rút ra:** Rule LUÔN áp dụng từ nay
  // ✅ Đúng: [code example]
  // ❌ Sai: [code example]
- **File liên quan:** `path/to/file`

Cập nhật thống kê đầu LESSONS.md.

NẾU KHÔNG → Bỏ qua, tiếp tục.
```

> ❌ KHÔNG BAO GIỜ xóa LESSONS.md — chỉ thêm mới.
> ✅ MỚI: Luôn thêm ✅❌ code examples (học từ bkns-minicrm)

---

### Bước 4 — DOCS UPDATE

Cập nhật theo quy tắc One Source of Truth:

| File | Hành động |
|---|---|
| `CHANGE_LOG.md` | Thêm entry `## [vX.Y.Z] - YYYY-MM-DD` với Added/Fixed/Changed |
| `NEXT-TODO.md` | XÓA task đã hoàn thành, THÊM task mới phát sinh |
| `GEMINI.md` | Cập nhật version, Implementation Status, Development History |
| `STATE.md` | Cập nhật phase/wave/task hiện tại (nếu dùng GSD) |

---

### Bước 5 — ATOMIC COMMIT & PUSH

```bash
# Nhóm commit theo module/feature:
git add [files cùng feature]
git commit -m "feat(scope): mô tả 72 ký tự max"

git add [docs files]
git commit -m "docs: update changelog vX.Y.Z"

# Push
git push origin main
```

Convention commits: `feat | fix | chore | docs | perf | sec | test`

> 🔑 Mỗi feature/fix = 1 commit riêng. Không gom.

---

### Bước 6 — MEMORY CONSOLIDATION

```
1. NẾU có ACTIVE_CONTEXT.md:
   → Archive hoặc xóa (context đã persist vào docs)
   → "✅ Working memory archived"

2. NẾU STATE.md tồn tại:
   → Cập nhật: version, phase, task xong, task tiếp theo

3. NẾU Qdrant available:
   → Store lessons mới vào collection (curl scripts)
```

---

### Bước 7 — END REPORT

```
✅ /save hoàn tất!

📊 REVIEW: [✅ 7/7 pass | ⚠️ N warnings accepted]
📝 LESSONS: [N bài học mới ghi | Không thay đổi]
📋 DOCS: CHANGE_LOG ✅ | NEXT-TODO ✅ | GEMINI ✅
📦 COMMITS: [N commits]
🔗 PUSHED: main → origin (hash: xxxxxxx)

🔜 TASK TIẾP THEO:
  → [Task ưu tiên cao nhất từ NEXT-TODO.md]

Gõ /start [task] để bắt đầu phiên mới!
```

---

## QUY TẮC QUAN TRỌNG

- 🛑 CRITICAL trong review → KHÔNG cho phép commit
- ❌ Thiếu evidence verify → KHÔNG cho phép commit
- ❌ Vi phạm LESSONS → phải sửa trước
- ✅ Luôn dùng tool edit file — không chỉ in ra màn hình
- ✅ Commit message rõ WHY, không chỉ WHAT
- ✅ LESSONS entry mới PHẢI có ✅❌ code examples
