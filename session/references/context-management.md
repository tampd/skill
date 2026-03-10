# Context Management — Strategic Context Engineering

> Adapted từ NeoLabHQ/context-engineering-kit + muratcankoylan
> Áp dụng trong /start và mọi lúc context dài

## Tại sao cần Context Engineering?

```
Gemini: 1M tokens context window — LỚN nhưng KHÔNG VÔ HẠN
Vấn đề: Token nhiều ≠ chất lượng cao
         "Lost-in-middle" effect: AI quên info ở giữa context
         Noise nhiều → output quality giảm
```

## 3 Nguyên tắc

### 1. STRATEGIC LOADING (Load thông minh)

```
TIER 1 — LUÔN LOAD (nhỏ, quan trọng):
  □ GEMINI.md (rules, brain)
  □ LESSONS.md → grep tags liên quan task
  □ INSTINCTS.md → relevant patterns
  □ ACTIVE_CONTEXT.md (nếu tồn tại)

TIER 2 — LOAD KHI CẦN:
  □ docs/architecture.md → khi cần hiểu hệ thống
  □ docs/business-rules.md → khi cần domain logic
  □ STATE.md → khi cần biết project state

TIER 3 — LOAD ĐÚNG LÚC:
  □ Chỉ đọc file đang cần edit (không đọc toàn bộ src/)
  □ Chỉ đọc test liên quan (không đọc toàn bộ test/)
  □ Dùng grep/search TRƯỚC khi view_file toàn bộ
```

### 2. CONTEXT COMPRESSION (Nén khi dài)

```
KHI CHECKPOINT / GIỮA SESSION:
  □ Tóm tắt what's done + what's remaining
  □ Ghi ACTIVE_CONTEXT.md → nếu session bị cut, có thể recover
  □ Không lặp lại code đã viết — reference bằng file path

SIGNALS CONTEXT BỊ DEGRADATION:
  ⚠️ AI bắt đầu lặp lại instruction đã nói
  ⚠️ AI quên decision đã make trước đó
  ⚠️ AI hỏi lại thông tin đã provide
  → Khi thấy dấu hiệu → /checkpoint ngay
```

### 3. CONTEXT OPTIMIZATION (Chất lượng > Số lượng)

```
DO:
  ✅ Put quan trọng nhất ở ĐẦU và CUỐI context (recency + primacy effect)
  ✅ Structured data (tables, YAML) thay vì prose dài
  ✅ Code snippets NGẮN với context đủ → thay vì toàn bộ file
  ✅ Reference file paths thay vì paste code inline

DON'T:
  ❌ Load 10 files "just in case"
  ❌ Paste toàn bộ database schema khi chỉ cần 1 table
  ❌ Duplicate info đã có trong GEMINI.md
  ❌ Long narratives khi bullet points đủ
```

## Antigravity-Specific Tips

```
CONVERSATION CHECKPOINTS:
  Antigravity tự tạo checkpoints khi context dài
  → Tận dụng: ghi ACTIVE_CONTEXT.md TRƯỚC khi checkpoint
  → Sau checkpoint: AI đọc lại context từ files

KNOWLEDGE ITEMS:
  Antigravity có knowledge system cross-conversation
  → Tận dụng: đọc KI summaries trước khi research
  → Nếu KI relevant → đọc artifacts thay vì research lại

BROWSER SUB-AGENT:
  Browser sub-agent có context RIÊNG → nhỏ
  → Ghi task description CHI TIẾT cho sub-agent
  → Nó không biết conversation history của bạn
```
