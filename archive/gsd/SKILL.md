---
name: GSD — Get Shit Done Methodology
description: "Quy trình phát triển phần mềm theo spec-driven, context-engineered methodology. Tích hợp Wave-Based Execution, Atomic Commits và Empirical Verification. Kích hoạt khi bắt đầu dự án mới hoặc feature lớn cần plan rõ ràng."
---

# HƯỚNG DẪN KỸ NĂNG: GSD — GET SHIT DONE

## Triết lý cốt lõi
> "Stop vibecoding. Start shipping. Describe your idea → GSD extracts everything the AI needs → Watch it build correctly."

GSD ngăn chặn việc code mà không có plan, tránh build sai thứ, tránh context bị mất giữa các phiên làm việc.

---

## SPEC TRƯỚC KHI CODE — Quy tắc vàng

> **Không có SPEC → Không được code.** Đây là nguyên tắc duy nhất cần nhớ.

### Khi nào cần SPEC?
| Loại task | Cần SPEC? |
|---|---|
| Fix bug nhỏ / sửa UI | ❌ Không — dùng vibe-code trực tiếp |
| Feature mới 1-2 ngày | ✅ SPEC ngắn (15 phút viết) |
| Feature lớn / refactor | ✅ SPEC đầy đủ + Wave plan |
| Dự án mới | ✅ SPEC + ROADMAP bắt buộc |

### SPEC 1 phút (cho feature nhỏ-vừa)
```
Tạo file: .gsd/SPEC.md

WHY: [1 câu — vấn đề đang giải quyết]
WHO: [EMPLOYEE / ADMIN / API]
WHAT: [2-3 bullet tính năng cụ thể]
DONE: [Acceptance criteria — khi nào xong?]
FILES: [List file cần tạo/sửa]
WAVE: [1. Foundation → 2. Logic → 3. UI]
```

> 📋 **Template đầy đủ**: Xem `docs/ARCHITECTURE.md` Section 11

### Quy trình khi SPEC xong
```
SPEC.md FINALIZED
  → /supper [tên feature] → chọn skill combo
  → /gsd /plan N → Wave 1 → commit → Wave 2 → commit → Wave 3
  → /review → /save → cập nhật STATE.md
```

---



## VÒNG ĐỜI DỰ ÁN GSD

```
/new-project → SPEC.md → /discuss-phase N → /plan N → /execute N → /verify N → /complete-milestone
```

### Các file trạng thái GSD
| File | Mục đích |
|---|---|
| `SPEC.md` | Đặc tả yêu cầu — phải FINALIZED trước khi code |
| `PLAN.md` | Kế hoạch chi tiết từng phase với XML tasks |
| `STATE.md` | Trạng thái hiện tại: phase, wave, task đang làm |
| `DECISIONS.md` | Các quyết định thiết kế và lý do |

---

## 26 LỆNH GSD

### 🔵 Core Workflow
| Lệnh | Mục đích |
|---|---|
| `/map` | Tạo/đọc ARCHITECTURE.md của dự án |
| `/plan [N]` | Lập plan cho phase N → PLAN.md |
| `/execute [N]` | Thực thi phase N theo PLAN.md |
| `/verify [N]` | Xác minh phase N có evidence |
| `/debug [desc]` | Debug vấn đề cụ thể |

### 🟢 Project Setup
| Lệnh | Mục đích |
|---|---|
| `/new-project` | Khởi tạo dự án mới → tạo SPEC.md |
| `/new-milestone` | Bắt đầu milestone mới |
| `/complete-milestone` | Hoàn thành milestone, tag version |
| `/audit-milestone` | Rà soát toàn bộ milestone |

### 🟠 Phase Management
| Lệnh | Mục đích |
|---|---|
| `/discuss-phase N` | Thảo luận yêu cầu phase N → DECISIONS.md |
| `/research-phase N` | Nghiên cứu kỹ thuật cho phase N |
| `/add-phase` | Thêm phase mới vào PLAN.md |
| `/list-phase-assumptions` | Liệt kê assumptions của phase |

### 🟣 Navigation & State
| Lệnh | Mục đích |
|---|---|
| `/progress` | Xem tiến độ theo PLAN.md |
| `/resume` | Load context từ STATE.md để tiếp tục |
| `/pause` | Lưu trạng thái hiện tại vào STATE.md |
| `/add-todo` | Thêm todo vào PLAN.md |
| `/check-todos` | Kiểm tra todo list |

### 🔴 Utilities
| Lệnh | Mục đích |
|---|---|
| `/web-search [query]` | Tìm kiếm web |
| `/whats-new` | Xem changelog GSD |
| `/help` | Hiển thị help |

---

## WAVE-BASED EXECUTION

Mỗi phase chia thành 3 waves, thực thi tuần tự:

```
🌊 Wave 1 — Foundation (Nền tảng)
   DB schema, models, migrations, core types

🌊 Wave 2 — Integration (Tích hợp)
   Services, controllers, API endpoints, business logic

🌊 Wave 3 — Polish (Hoàn thiện)
   UI, validation, error handling, edge cases, tests
```

**Quy tắc**: Phải hoàn thành 100% Wave 1 trước khi bắt đầu Wave 2.

---

## XML TASK FORMAT

Dùng format này khi plan task phức tạp trong PLAN.md:

```xml
<task type="auto">
  <name>Tên task ngắn gọn</name>
  <files>path/to/file1, path/to/file2</files>
  <action>
    Mô tả CHÍNH XÁC cần làm gì. Không mơ hồ.
    Chỉ rõ: dùng thư viện nào, pattern nào, approach nào.
  </action>
  <verify>Cách kiểm tra task đã xong: curl URL? test pass? screenshot?</verify>
  <done>Định nghĩa "hoàn thành" rõ ràng, có thể đo lường được</done>
</task>
```

---

## ATOMIC GIT COMMITS

Sau mỗi task hoàn thành → commit ngay:

```bash
# Format bắt buộc:
git commit -m "feat(phase-N): mô tả task cụ thể"
git commit -m "fix(phase-N): mô tả bug đã sửa"
git commit -m "chore(phase-N): mô tả công việc kỹ thuật"

# Ví dụ:
git commit -m "feat(phase-3): add attendance calculation service"
git commit -m "fix(phase-3): fix null pointer in payroll loop"
```

**Tại sao atomic?**
- Git bisect tìm được task gây lỗi chính xác
- Mỗi task có thể revert độc lập
- AI trong phiên sau đọc history dễ hiểu bối cảnh

---

## EMPIRICAL VERIFICATION

Không chấp nhận "nó chạy rồi" — phải có bằng chứng:

| Loại thay đổi | Evidence yêu cầu |
|---|---|
| API endpoint | `curl` response hoặc HTTP test |
| UI component | Screenshot từ browser |
| Service logic | Unit test passed |
| DB migration | `php artisan migrate:status` output |
| Import/export | File output sample |

---

## QUY TRÌNH KHỞI ĐỘNG DỰ ÁN MỚI

```
1. /new-project
   → Điền tên, mô tả, tech stack → tạo SPEC.md

2. Review SPEC.md với user
   → Khi SPEC.md đạt FINALIZED mới được code

3. /plan 1 → PLAN.md với Wave breakdown + XML tasks

4. /execute 1 → Code theo từng task trong PLAN.md

5. /verify 1 → Chạy evidence (test/curl/screenshot)

6. /pause → Lưu STATE.md → commit → push

7. Phiên sau: /resume → đọc STATE.md → tiếp tục
```

---

## CONTEXT ENGINEERING

Cung cấp đủ context trước mỗi task lớn:

```
✅ PHẢI CÓ: SPEC.md | ARCHITECTURE.md | LESSONS.md | PLAN.md hiện tại
✅ GIỚI HẠN: Không để context window quá lớn — chỉ load file liên quan
✅ STATE: Sau mỗi wave hoàn thành → cập nhật STATE.md
```

---

## TÍCH HỢP VỚI HỆ THỐNG HIỆN TẠI

| GSD Command | Tương đương Antigravity |
|---|---|
| `/resume` | `/start` |
| `/pause` | `/save` |
| `/plan N` | Plan phase trong `/supper` |
| Evidence step | Bước verify trong `/review` |
| SPEC.md | Mô tả task trước khi code |
