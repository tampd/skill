# SKILL MEMORY — Báo Cáo Tối Ưu Hóa Bộ Nhớ AI
## Hệ Thống Trí Nhớ Toàn Diện cho Google Antigravity

> **Phiên bản:** 1.0.0 | **Ngày:** 2026-02-27 | **Tác giả:** Antigravity AI System

---

## 1. EXECUTIVE SUMMARY

Google Antigravity là một AI agent làm việc theo từng **phiên độc lập** (session-based). Đây vừa là điểm mạnh (stateless, không bị ô nhiễm context cũ) nhưng cũng là điểm yếu lớn nhất: **AI có thể quên mọi thứ giữa các phiên**.

Báo cáo này trình bày:
- Phân tích **5 lỗ hổng memory** nghiêm trọng trong quy trình hiện tại
- Kiến trúc **3-Layer Memory System** để giải quyết triệt để
- Hệ thống **ACTIVE_CONTEXT.md** — working memory cho AI
- Skill mới **memory-optimizer** với 3 lệnh `/memory`, `/checkpoint`, `/recall`
- Cách **tích hợp memory hooks** vào tất cả 25 skill hiện có
- Quy trình làm việc **mới hoàn toàn** sau khi tối ưu hóa

**Kết quả kỳ vọng:** AI không bao giờ quên mình đang làm gì, đang ở đâu trong dự án, và không bao giờ lặp lại sai lầm cũ.

---

## 2. PHÂN TÍCH LỖ HỔNG MEMORY HIỆN TẠI

### 2.1 Ma Trận Lỗ Hổng

| ID | Lỗ Hổng | Mức Độ | Tần Suất | Hậu Quả |
|---|---|---|---|---|
| M-001 | Context mất hoàn toàn giữa phiên | CRITICAL | Mỗi phiên | AI bắt đầu lại từ đầu |
| M-002 | Không có working memory trong phiên | HIGH | Mỗi task phức tạp | AI quên context giữa chừng |
| M-003 | LESSONS.md không được enforce tự động | HIGH | Thường xuyên | Lặp lại lỗi cũ |
| M-004 | Không có signal khi context window đầy | MEDIUM | Dự án lớn | AI hallucinate |
| M-005 | Skill chỉ load khi gọi thủ công | MEDIUM | Mọi lúc | Không có memory baseline |

### 2.2 Root Cause Analysis

```
AI Session Lifecycle:
  [Phiên N kết thúc] → [Toàn bộ context RAM bị xóa] → [Phiên N+1 bắt đầu]
                                ↑
                     LỖ HỔNG LỚN NHẤT
                     Chỉ LESSONS.md + STATE.md tồn tại
                     Nhưng AI phải nhớ để chủ động đọc

Vấn đề cụ thể:
  - /start đọc nhiều file nhưng KHÔNG có ưu tiên "task đang dang dở"
  - vibe-code KHÔNG lưu context sau mỗi sub-task
  - save KHÔNG checkpoint working memory trước khi kết thúc
  - Khi context window > 80%, AI bắt đầu quên các file đã đọc trước đó
  - Không có file nào capture "luồng suy nghĩ hiện tại" của AI
```

### 2.3 Kịch Bản Thực Tế Hay Gặp

**Kịch Bản A — "Phiên Gián Đoạn":**
```
14:00 — AI bắt đầu code feature phức tạp (3 file, 2 service)
15:30 — User đóng browser đột ngột
16:00 — User mở lại, gõ /start
16:00 — AI đọc NEXT-TODO.md, STATE.md... nhưng KHÔNG biết:
          → Đang code dang dở file nào?
          → Đã quyết định dùng pattern gì?
          → Đang ở sub-step nào trong task?
RESULT: AI bắt đầu lại từ đầu, có thể code theo cách khác!
```

**Kịch Bản B — "Context Window Overflow":**
```
Phiên dài 3 tiếng, nhiều file được đọc
Context window dần đầy (Gemini 2.0: ~1M tokens)
AI bắt đầu "quên" các file đọc từ đầu phiên
AI đưa ra quyết định MÂU THUẪN với quyết định trước đó
User: "Sao anh lại làm vậy? Lúc nãy anh nói khác mà?"
AI: [Không nhớ gì]
```

**Kịch Bản C — "Lỗi Lặp Lại":**
```
#BUG-012 đã ghi trong LESSONS.md: "Không dùng passthru() trong PHP"
Phiên mới, task mới liên quan đến PHP
AI KHÔNG kiểm tra LESSONS.md (chỉ /start mới đọc, không phải giữa phiên)
AI gợi ý dùng passthru() → User phải sửa lại
```

---

## 3. KIẾN TRÚC 3-LAYER MEMORY SYSTEM

### 3.1 Tổng Quan Kiến Trúc

```
╔══════════════════════════════════════════════════════════════╗
║  LAYER 3: EPISODIC MEMORY (Bộ nhớ dài hạn — Không bao giờ xóa)
║  ─────────────────────────────────────────────────────────────
║  Files: LESSONS.md | CHANGE_LOG.md | DECISIONS.md
║  Đặc điểm: Append-only, cross-project, permanent record
║  Cập nhật: Sau mỗi /save, sau mỗi bug fix
║  Truy cập: /start (đọc relevant) + bug-memory-journal
╠══════════════════════════════════════════════════════════════╣
║  LAYER 2: SEMANTIC MEMORY (Bộ nhớ ngữ nghĩa — Context dự án)
║  ─────────────────────────────────────────────────────────────
║  Files: GEMINI.md | STATE.md | NEXT-TODO.md | PLAN.md
║  Đặc điểm: Cấu trúc dự án, tech stack, phase hiện tại
║  Cập nhật: Theo milestone, theo phase
║  Truy cập: /start (đọc tất cả) + /resume trong GSD
╠══════════════════════════════════════════════════════════════╣
║  LAYER 1: WORKING MEMORY (Bộ nhớ làm việc — MỚI, quan trọng nhất)
║  ─────────────────────────────────────────────────────────────
║  Files: ACTIVE_CONTEXT.md (MỚI)
║  Đặc điểm: Real-time task tracking, trong phiên làm việc
║  Cập nhật: Sau mỗi sub-task, sau mỗi quyết định quan trọng
║  Truy cập: Tự động đầu /start, đầu mỗi task trong vibe-code
╚══════════════════════════════════════════════════════════════╝
```

### 3.2 Luồng Dữ Liệu Memory

```
┌─────────────────────────────────────────────────────────────┐
│                    MEMORY LIFECYCLE                          │
│                                                              │
│  /start ──────→ Load Layer 3 + 2 + 1 → Báo cáo đầy đủ     │
│     │                                                        │
│     ↓                                                        │
│  /supper ─────→ Đọc ACTIVE_CONTEXT.md → Suggest phù hợp   │
│     │                                                        │
│     ↓                                                        │
│  vibe-code ───→ Load ACTIVE_CONTEXT → Code → Checkpoint →  │
│     │           Update ACTIVE_CONTEXT → Loop                │
│     ↓                                                        │
│  /checkpoint ─→ Snapshot toàn bộ working memory            │
│     │                                                        │
│     ↓                                                        │
│  /review ─────→ Cross-check with LESSONS.md                │
│     │                                                        │
│     ↓                                                        │
│  /save ────────→ Consolidate → Compress → Archive →        │
│                  Update Layer 2 + 3 → Clear Layer 1         │
└─────────────────────────────────────────────────────────────┘
```

---

## 4. ACTIVE_CONTEXT.MD — WORKING MEMORY SCHEMA

### 4.1 Cấu Trúc File Đầy Đủ

```markdown
# ACTIVE_CONTEXT — [Tên Dự Án]
> Auto-generated by memory-optimizer skill. DO NOT edit manually during session.
> Last checkpoint: YYYY-MM-DD HH:MM | Session duration: Xh Ym

## ⚡ SNAPSHOT HIỆN TẠI
- **Phiên bắt đầu:** YYYY-MM-DD HH:MM
- **Task đang làm:** [Tên task cụ thể — lấy từ NEXT-TODO.md]
- **Skill đang dùng:** [vibe-code / gsd / debugger / ...]
- **Wave/Phase:** Wave N — [Tên wave]
- **Files đang mở/chỉnh:** 
  - `path/to/file1.ts` — [đang làm gì]
  - `path/to/file2.php` — [đang làm gì]

## 🔑 QUYẾT ĐỊNH ĐÃ RA (Phiên này)
- [YYYY-MM-DD HH:MM] Quyết định dùng [pattern X] vì [lý do Y]
- [YYYY-MM-DD HH:MM] Bỏ approach [Z] vì [lý do W]

## ✅ ĐÃ HOÀN THÀNH (Phiên này)
- [x] [Task/sub-task đã xong] — commit: feat(x): mô tả
- [x] [Task/sub-task đã xong]

## 🔄 ĐANG LÀM (Checkpoint gần nhất)
- [ ] [Sub-task đang ở giữa chừng]
  - Đã làm: [những gì đã làm]
  - Chưa làm: [những gì còn lại]
  - Context cần nhớ: [thông tin quan trọng]

## ⛔ BLOCKERS & ISSUES
- [Issue nếu có]
- [Cần hỏi user về: ...]

## 🧠 CONTEXT KỸ THUẬT QUAN TRỌNG
- [API key đang dùng: từ .env, key X]
- [DB schema thay đổi gần nhất: ...]
- [Pattern đang áp dụng: ...]
- [Dependency mới thêm: ...]

## ⚠️ LESSONS ÁP DỤNG PHIÊN NÀY
- #BUG-XXX: [Đang cẩn thận tránh vì lý do Y]

## 🎯 NEXT IMMEDIATE ACTION
```
[Câu lệnh hoặc hành động CỤ THỂ tiếp theo — đủ để AI tiếp tục ngay]
Ví dụ: "Tiếp tục vibe-code: implement function validatePayroll() trong PayrollService.php"
```
```

### 4.2 Quy Tắc Vàng cho ACTIVE_CONTEXT.md

```
1. KHÔNG BAO GIỜ để ACTIVE_CONTEXT.md trống khi đang làm task phức tạp
2. Cập nhật NGAY khi: ra quyết định quan trọng, xong 1 sub-task, gặp blocker
3. NEXT IMMEDIATE ACTION phải đủ chi tiết để AI mới context vào code ngay
4. Files listed phải cụ thể (path đầy đủ + đang làm gì)
5. KHÔNG ghi thông tin nhạy cảm (password, API key thật) vào đây
6. Xóa sau /save — không để tồn tại sang phiên không liên quan
```

---

## 5. MEMORY-OPTIMIZER SKILL — LỆNH VÀ CÁCH DÙNG

### 5.1 Ba Lệnh Cốt Lõi

#### `/checkpoint` — Lưu Trạng Thái Working Memory
```
Khi nào gọi:
  - Sắp nghỉ giữa task phức tạp (ngay cả 10 phút)
  - Sau mỗi sub-task quan trọng hoàn thành
  - Khi sắp chuyển sang task khác
  - Khi context window cảm giác đầy (nhiều file đã đọc)
  - Trước mỗi /save

AI sẽ làm:
  1. Tổng hợp toàn bộ context hiện tại
  2. Ghi vào ACTIVE_CONTEXT.md theo schema
  3. Xác nhận: "✅ Checkpoint #N lưu thành công lúc HH:MM"
```

#### `/recall` — Khôi Phục Context Từ Checkpoint
```
Khi nào gọi:
  - Vừa mở lại sau khi nghỉ giữa chừng
  - Cảm thấy AI đang "lạc" không biết đang làm gì
  - Sau khi /start nhưng muốn detail hơn về task đang dở

AI sẽ làm:
  1. Đọc ACTIVE_CONTEXT.md
  2. Tóm tắt: "Bạn đang ở đây..."
  3. Đọc FILES đang chỉnh → load lại context kỹ thuật
  4. Đề xuất: "Tôi sẵn sàng tiếp tục: [NEXT IMMEDIATE ACTION]"
```

#### `/memory` — Xem Tổng Quan Memory System
```
Khi nào gọi:
  - Muốn check trạng thái 3-layer memory
  - Muốn biết context window đang dùng bao nhiêu %
  - Review tất cả decisions đã ra trong phiên

AI sẽ làm:
  1. Báo cáo Layer 1: ACTIVE_CONTEXT.md status
  2. Báo cáo Layer 2: STATE.md / GEMINI.md relevant info
  3. Báo cáo Layer 3: Top 3 relevant LESSONS từ LESSONS.md
  4. Estimate context window usage: "~X% context used"
  5. Gợi ý: checkpoint nếu > 70%, review nếu nhiều decisions
```

### 5.2 Khi Nào Checkpoint Tự Động (Auto-Checkpoint Rules)

```
TRIGGER                              ACTION
─────────────────────────────────────────────────────
Sau mỗi commit  ────────────────────→ Mini checkpoint (3 dòng)
Xong 1 wave đầy đủ ─────────────────→ Full checkpoint
Context > 70% (estimate) ───────────→ Cảnh báo + gợi ý checkpoint
Gặp blocking error ─────────────────→ Auto-capture vào BLOCKERS
User dùng "nhớ lấy / ghi lại" ──────→ Append vào DECISIONS
Trước /save ─────────────────────────→ Mandatory full checkpoint
```

---

## 6. TÍCH HỢP VỚI 25 SKILL HIỆN CÓ

### 6.1 Bảng Tích Hợp Đầy Đủ

| Skill | Memory Hook Được Thêm | Thời Điểm |
|---|---|---|
| `start` | Đọc ACTIVE_CONTEXT.md → hiển thị đầu báo cáo | Đầu phiên |
| `vibe-code` | Load + Update ACTIVE_CONTEXT sau mỗi sub-task | Trong task |
| `save` | Full checkpoint → Consolidate → Archive | Cuối phiên |
| `supper` | Đọc ACTIVE_CONTEXT để context-aware skill select | Khi chọn skill |
| `bug-memory-journal` | Cross-ref LESSONS.md với ACTIVE_CONTEXT | Khi code/debug |
| `gsd` | /pause = /checkpoint, /resume = /recall | Project flow |
| `review` | Kiểm tra ACTIVE_CONTEXT.md có đúng với code thực tế không | Trước commit |
| `debugger` | Ghi capture vào ACTIVE_CONTEXT.BLOCKERS | Khi debug |
| `performance-engineer` | Ghi findings vào ACTIVE_CONTEXT.CONTEXT | Khi optimize |
| `security-auditor` | Ghi vulnerabilities vào ACTIVE_CONTEXT | Khi audit |
| Tất cả skill còn lại | Đọc ACTIVE_CONTEXT khi bắt đầu, không bắt buộc update | Tùy ngữ cảnh |

### 6.2 Chi Tiết Cập Nhật Từng Skill Core

#### 6.2.1 `start` — Memory Bootstrap Protocol (THAY ĐỔI LỚN)

**TRƯỚC khi cập nhật:**
```
Bước 1: Đọc GEMINI.md + CHANGE_LOG.md + NEXT-TODO.md + LESSONS.md + STATE.md
Bước 2: Phân tích và tổng hợp
Bước 3: Cảnh báo LESSONS.md
Bước 4: Báo cáo
```

**SAU khi cập nhật — Thêm Bước 0:**
```
[MỚI] Bước 0 — MEMORY BOOTSTRAP (TRƯỚC TẤT CẢ):
  1. Kiểm tra ACTIVE_CONTEXT.md có tồn tại không?
     - NẾU CÓ → 🚨 ALERT: "Phiên làm việc cũ chưa kết thúc đúng cách!"
                  Hiển thị ngay: Task đang dở, Files đang chỉnh, Next action
                  Hỏi: "Bạn muốn tiếp tục hay bắt đầu mới?"
     - NẾU KHÔNG → Tiếp tục Bước 1 như bình thường

  [THAY ĐỔI] Bước 4 — Báo cáo thêm section:
  ━━━━━━━━━━━━━━━━━━━━
  🔴 PHIÊN CŨ CHƯA LƯU (nếu có ACTIVE_CONTEXT.md)
  ━━━━━━━━━━━━━━━━━━━━
  Task đang dở: [task]
  Đã làm: [danh sách]
  Tiếp theo: [NEXT IMMEDIATE ACTION]
  → Gõ "/recall" để load đầy đủ context + tiếp tục ngay
```

**Impact:** AI không bao giờ bắt đầu sai khi còn task đang dở.

#### 6.2.2 `vibe-code` — Memory Hooks (THAY ĐỔI TRUNG BÌNH)

**TRƯỚC:**
```
Bước 0: Load LESSONS.md + GEMINI.md
Bước 1: SPEC TASK
Bước 2: CODE
Bước 3: VERIFY
Bước 4: COMMIT
Bước 5: LESSONS CHECK
Bước 6: UPDATE STATE
```

**SAU — Memory Hooks được chèn:**
```
Bước 0 (giữ nguyên): Load LESSONS.md + GEMINI.md
[MỚI] Bước 0.5 — LOAD WORKING MEMORY:
  → Kiểm tra ACTIVE_CONTEXT.md tồn tại không?
  → Nếu có: hiển thị TÓM TẮT 3 dòng về task đang làm
  → Đọc FILES đang chỉnh (trong ACTIVE_CONTEXT)

Bước 1-4 giữ nguyên

[MỚI] Sau Bước 4 (sau mỗi commit):
  → Auto mini-checkpoint: Update ACTIVE_CONTEXT.md
    - Mark task vừa commit là [x] COMPLETED
    - Set NEXT IMMEDIATE ACTION = task tiếp theo
    - Record decision nếu có

Bước 5-6 (giữ nguyên)

[MỚI] Bước 6.5 — CHECKPOINT CHECK:
  → Nếu đây là task cuối phiên: gợi ý /save
  → Nếu còn task tiếp: "Context đã checkpoint. Tiếp tục [task]?"
```

#### 6.2.3 `save` — Memory Consolidation (THAY ĐỔI TRUNG BÌNH)

**THÊM VÀO Bước 0 (trước review):**
```
[MỚI] Bước -1 — MEMORY AUDIT:
  1. Đọc ACTIVE_CONTEXT.md (nếu có)
  2. Liệt kê: task nào đã xong, task nào còn dở
  3. Nếu còn task dở → cảnh báo: "Còn [N] task chưa xong. Lưu tạm hay tiếp tục?"
  4. Nếu lưu tạm → chạy /checkpoint trước khi save

[MỚI] Bước 7.5 — MEMORY CONSOLIDATION (sau commit, trước push):
  1. Merge ACTIVE_CONTEXT thông tin quan trọng vào STATE.md
  2. Archive ACTIVE_CONTEXT.md → ACTIVE_CONTEXT.archive.md (timestamp)
  3. Xóa ACTIVE_CONTEXT.md (phiên mới sẽ tạo lại)
  4. Xác nhận: "✅ Working memory đã được consolidate và archive"
```

#### 6.2.4 `supper` — Memory-Aware Skill Selection

**THÊM VÀO Bước 1 (phân tích yêu cầu):**
```
[MỚI] Bước 0 — CONTEXT READ:
  → Đọc ACTIVE_CONTEXT.md (nếu tồn tại)
  → Extract: task đang làm, files đang chỉnh, decisions đã ra
  → Dùng thông tin này để suggest skill PHÙ HỢP HƠN

[MỚI] Rule bổ sung:
  - Task phức tạp > 30 phút HOẶC nhiều file: → đề xuất dùng /checkpoint định kỳ
  - Đang ở giữa task kỹ thuật: → hiển thị "Tôi thấy bạn đang làm [X]..."

[MỚI] Skill mới trong danh sách (26th skill):
  | 26 | memory-optimizer | /checkpoint /recall /memory — quản lý working memory |
```

### 6.3 Quy Trình Làm Việc MỚI (Updated Workflow)

```
OLD WORKFLOW:
/start → /supper → vibe-code → /review → /save

NEW WORKFLOW (Memory-Enhanced):
/start ──────────→ Đọc Layer 2+3 + Check ACTIVE_CONTEXT (Layer 1)
   │                    ↓ (có ACTIVE_CONTEXT.md)
   │               /recall → Load working memory → Tiếp tục ngay
   │                    ↓ (không có)
   ↓               Báo cáo bình thường
/supper ─────────→ Memory-aware skill selection
   ↓
vibe-code ───────→ Bước 0.5: Load ACTIVE_CONTEXT
   │               Bước 2: Code
   │               [After commit]: Mini checkpoint ACTIVE_CONTEXT
   │               Bước 6.5: Full checkpoint nếu task lớn
   ↓
/checkpoint ─────→ (khi cần, giữa phiên)
   ↓
/review ─────────→ Cross-check code vs ACTIVE_CONTEXT
   ↓
/save ───────────→ Memory Audit → Consolidate → Archive → Push
```

---

## 7. CONTEXT WINDOW MANAGEMENT

### 7.1 Vấn Đề Context Window

Gemini 2.0 Flash có context window ~1M tokens — rất lớn nhưng không vô hạn.

```
Context Window Usage Estimate:
  - GEMINI.md: ~2K tokens
  - LESSONS.md (đầy đủ): ~10K tokens
  - STATE.md: ~3K tokens
  - Mỗi file code đọc: ~2-5K tokens
  - Conversation history: tích lũy theo thời gian

Danger Zone: Khi đọc nhiều file code + conversation dài
→ AI bắt đầu "quên" file đọc đầu phiên
→ Đưa ra quyết định mâu thuẫn
```

### 7.2 Memory Compression Protocol

Khi cần giảm context load (phiên dài):

```
COMPRESSION LEVEL 1 — Tóm tắt LESSONS.md (nhẹ):
  Thay vì đọc full → chỉ đọc entries có tags liên quan đến task hiện tại
  Tiết kiệm: 70% context của LESSONS.md

COMPRESSION LEVEL 2 — Progressive file loading (trung bình):
  Thay vì đọc tất cả ngay → chỉ đọc file đang làm
  Đọc file khác khi cần thật sự
  Tiết kiệm: 60% context của code files

COMPRESSION LEVEL 3 — ACTIVE_CONTEXT summary mode (nặng):
  Khi context > 80%: chỉ làm việc với ACTIVE_CONTEXT.md
  Không đọc thêm file mới trừ khi bắt buộc
  Checkpoint ngay và gợi ý user bắt đầu phiên mới
```

### 7.3 Dấu Hiệu Nhận Biết Context Overflow

```
AI nói: "Xin lỗi, tôi không nhớ/thấy..." → Dấu hiệu đỏ
AI đưa ra quyết định mâu thuẫn với trước → Cần checkpoint ngay
AI yêu cầu bạn nhắc lại info đã nói → Context bị truncate
```

---

## 8. BEST PRACTICES — QUY TẮC VÀNG

### 8.1 Cho AI (Antigravity phải tuân thủ)

```
RULE 1: Không bao giờ quên checkpoint
  → Sau mỗi commit quan trọng → mini checkpoint
  → Trước khi chuyển task hoàn toàn khác → full checkpoint
  → Trước /save → mandatory checkpoint

RULE 2: ACTIVE_CONTEXT phải đủ để "AI lạ" tiếp tục được
  → "AI lạ" = một AI chưa biết gì về phiên này
  → Nếu AI đọc xong ACTIVE_CONTEXT mà có thể tiếp tục → ĐÚNG
  → Nếu không → ACTIVE_CONTEXT chưa đủ chi tiết

RULE 3: LESSONS.md là bộ nhớ linh hồn — không bao giờ bỏ qua
  → Đọc LESSONS.md TRƯỚC KHI code, không phải sau
  → Tag search: chỉ đọc tags liên quan đến task (tiết kiệm context)

RULE 4: Không để context window > 80% mà không checkpoint
  → Estimate bằng: số file đã đọc × 3K + conversation ÷ khoảng = X%
  → Khi > 70% → cảnh báo user, gợi ý checkpoint

RULE 5: NEXT IMMEDIATE ACTION phải actionable
  → SAI: "Tiếp tục code"
  → ĐÚNG: "Implement function validatePayroll(int $month) trong /app/Services/PayrollService.php, dùng bcmath, tham khảo LESSONS.md #BUG-012"
```

### 8.2 Cho User (Best Practices khi làm việc với Antigravity)

```
Thói quen tốt:
  ✅ Gõ /start ĐẦU mỗi phiên — không skip
  ✅ Gõ /checkpoint khi nghỉ > 15 phút
  ✅ Gõ /recall khi quay lại sau khi nghỉ
  ✅ Gõ /save CUỐI phiên — không skip
  ✅ Nói "nhớ lấy" hoặc "ghi lại" khi ra quyết định quan trọng

Thói quen xấu:
  ❌ Đóng browser đột ngột khi đang giữa task phức tạp (mất context)
  ❌ Không /save sau phiên dài — mất tất cả learning
  ❌ Phiên quá dài (> 4 tiếng) không checkpoint — context overflow
  ❌ Không xem LESSONS.md khi bắt đầu — lặp lỗi cũ
```

---

## 9. SCHEMA TỔNG HỢP: TẤT CẢ FILE MEMORY

| File | Layer | Tạo khi | Cập nhật khi | Xóa khi | Giữ lại |
|---|---|---|---|---|---|
| `ACTIVE_CONTEXT.md` | 1 | Bắt đầu task phức tạp | Sau mỗi sub-task, commit | Sau /save | Không |
| `STATE.md` | 2 | /new-project hoặc GSD | Sau mỗi wave, /save | Không bao giờ | Mãi mãi |
| `NEXT-TODO.md` | 2 | Đầu dự án | Sau mỗi /save | Không bao giờ | Mãi mãi |
| `GEMINI.md` | 2 | Project init | Khi arch thay đổi | Không bao giờ | Mãi mãi |
| `LESSONS.md` | 3 | Sau bug đầu tiên | Append-only | Không bao giờ | Mãi mãi |
| `CHANGE_LOG.md` | 3 | Đầu dự án | Sau mỗi /save | Không bao giờ | Mãi mãi |
| `DECISIONS.md` | 3 | Khi có arch decision | Append-only | Không bao giờ | Mãi mãi |

---

## 10. ROADMAP & NEXT STEPS

### Immediate (Thực hiện ngay)
- [x] Viết báo cáo này (skill_memory.md)
- [ ] Tạo `/root/skill/memory-optimizer/SKILL.md`
- [ ] Cập nhật `start/SKILL.md` với Memory Bootstrap Protocol
- [ ] Cập nhật `vibe-code/SKILL.md` với Memory Hooks
- [ ] Cập nhật `save/SKILL.md` với Memory Consolidation

### Short-term (1-2 tuần sau khi dùng thực tế)
- [ ] Evaluate: ACTIVE_CONTEXT.md có đủ chi tiết không?
- [ ] Tune: checkpoint frequency (quá nhiều vs quá ít)
- [ ] Add: automatic context window estimation

### Long-term
- [ ] Template ACTIVE_CONTEXT.md cho từng loại dự án (Laravel, Next.js, N8N)
- [ ] Cơ chế auto-archive LESSONS.md khi > 100 entries
- [ ] Cross-project memory: học từ dự án này áp dụng cho dự án khác

---

*Báo cáo được tạo bởi Antigravity AI System — 2026-02-27*
*"Một AI không nhớ quá khứ sẽ mãi lặp lại sai lầm. Một AI nhớ đủ ngữ cảnh sẽ làm việc như một senior engineer thực thụ."*
