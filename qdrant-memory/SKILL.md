---
name: qdrant-memory
description: >
  Hướng dẫn cài đặt Qdrant Vector Database làm Layer 4 Memory cho Google Antigravity.
  Kết nối MCP để AI có thể lưu và tìm kiếm semantic memory cross-session, cross-project.
  Cài lên VPS để persistent 24/7 và accessible từ mọi thiết bị.
---

# HƯỚNG DẪN: QDRANT MEMORY — LAYER 4 SETUP

## 📋 Tổng quan

Qdrant là vector database — cho phép AI tìm kiếm memory theo **nghĩa** thay vì keyword.
Cài trên VPS → persistent, cross-session, cross-project.

```
Google Antigravity
   ↓ MCP tools
Qdrant trên VPS (:6333)
   ├── Collection: payroll.trieuphu.biz
   ├── Collection: [dự án khác]
   └── Collection: global_patterns
```

---

## 🚀 BƯỚC 1: CÀI QDRANT TRÊN VPS

### 1.1 — Cài Docker (nếu chưa có)
```bash
# Ubuntu/Debian VPS
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER
```

### 1.2 — Chạy Qdrant container
```bash
# Tạo thư mục lưu data persistent
mkdir -p ~/qdrant_storage

# Chạy Qdrant (auto-restart khi VPS reboot)
docker run -d \
  --name qdrant \
  --restart unless-stopped \
  -p 6333:6333 \
  -p 6334:6334 \
  -v ~/qdrant_storage:/qdrant/storage \
  qdrant/qdrant

# Kiểm tra đang chạy
docker ps | grep qdrant
curl http://localhost:6333/health
```

### 1.3 — Bảo mật Qdrant (bắt buộc nếu public VPS)
```bash
# Tạo API key trong Qdrant config
# File: ~/qdrant_storage/config/config.yaml
cat << 'EOF' > ~/qdrant_storage/config/config.yaml
service:
  api_key: "your-secret-api-key-here"
  host: 0.0.0.0
  http_port: 6333
EOF

# Restart để áp dụng
docker restart qdrant

# Chỉ cho phép kết nối từ máy của bạn (firewall)
ufw allow from YOUR_IP_ADDRESS to any port 6333
```

### 1.4 — Tạo Collections
```bash
# Collection cho payroll project
curl -X PUT 'http://localhost:6333/collections/payroll_trieuphu' \
  -H 'Content-Type: application/json' \
  -H 'api-key: your-secret-api-key-here' \
  -d '{
    "vectors": {
      "size": 1536,
      "distance": "Cosine"
    }
  }'

# Collection global (cross-project patterns)
curl -X PUT 'http://localhost:6333/collections/global_patterns' \
  -H 'Content-Type: application/json' \
  -H 'api-key: your-secret-api-key-here' \
  -d '{
    "vectors": {
      "size": 1536,
      "distance": "Cosine"
    }
  }'

# Verify
curl 'http://localhost:6333/collections' -H 'api-key: your-secret-api-key-here'
```

---

## 🔌 BƯỚC 2: CÀI QDRANT MCP SERVER

### 2.1 — Cài mcp-server-qdrant
```bash
# Trên máy local của bạn (không phải VPS)
pip install mcp-server-qdrant

# Hoặc dùng uvx (recommended)
pip install uv
uvx mcp-server-qdrant --help
```

### 2.2 — Cấu hình Antigravity (Gemini CLI)
Thêm vào file cấu hình Gemini CLI (`.gemini/settings.json` hoặc tương đương):

```json
{
  "mcpServers": {
    "qdrant-memory": {
      "command": "uvx",
      "args": [
        "mcp-server-qdrant",
        "--qdrant-url", "http://YOUR_VPS_IP:6333",
        "--qdrant-api-key", "your-secret-api-key-here",
        "--collection-name", "global_patterns",
        "--embedding-provider", "gemini",
        "--embedding-model", "models/text-embedding-004"
      ]
    }
  }
}
```

### 2.3 — Verify MCP tools hoạt động
Sau khi Antigravity restart, các tools mới sẽ xuất hiện:
- `qdrant_store` — lưu memory
- `qdrant_find` — tìm memory theo nghĩa

```
Test: "Hãy lưu vào Qdrant: Dự án payroll dùng bcmath cho tính tiền"
Test: "Tìm trong Qdrant: Cách tính lương chính xác"
→ Phải trả về kết quả liên quan ngay cả khi không dùng keyword "bcmath"
```

---

## 📚 BƯỚC 3: TÍCH HỢP VỚI WORKFLOW

### 3.1 — Khi nào store vào Qdrant
```
STORE sau mỗi sự kiện này:
  ✅ Fix bug quan trọng → store vào {project}_collection VÀ global_patterns
  ✅ Quyết định kiến trúc → store với tags: [architecture, project, date]
  ✅ Pattern code hiệu quả → store vào global_patterns
  ✅ Phiên /save → batch store highlights của phiên

Metadata cần ghi kèm:
  { project: "payroll", date: "2026-02-27", tags: ["php", "bcmath", "money"],
    type: "lesson" | "decision" | "pattern" | "bug_fix" }
```

### 3.2 — Cách query Qdrant trong luồng làm việc
```
/start → qdrant_find("task hiện tại: [mô tả task]")
         → Trả về: top 5 memories liên quan nhất
         → Hiển thị: "🧠 Qdrant nhớ: [N] pattern liên quan"

Khi gặp vấn đề → qdrant_find("vấn đề: [mô tả lỗi]")
                → Tìm: đã giải quyết tương tự chưa?

Khi /save → qdrant_store(session_highlights, metadata)
```

### 3.3 — Schema memory payload
```json
{
  "content": "Mô tả memory dạng plain text — đủ để AI hiểu không cần context thêm",
  "metadata": {
    "project": "payroll.trieuphu.biz",
    "type": "bug_fix",
    "tags": ["php", "bcmath", "money"],
    "date": "2026-02-27",
    "severity": "critical",
    "lesson": "Luôn dùng bcmath cho tính tiền — float không chính xác",
    "reference": "#BUG-012"
  }
}
```

---

## 🗂️ MULTI-PROJECT SETUP

```
Cấu trúc Collections khuyến nghị:

global_patterns       ← Patterns áp dụng cho mọi dự án
  "Luôn dùng DB transaction cho multi-step writes"
  "N+1 query fix: dùng whereIn + groupBy"

payroll_trieuphu      ← Workspace-specific
  "PayrollService.calculateForEmployee: bcmath scale=4"
  "ConfigOverride cascade: Employee → Level → LoaiNV → Global"

[project_name]        ← Thêm collection cho mỗi dự án mới
```

Query order khi /start:
```
1. Query {current_project} collection → highest relevance
2. Query global_patterns collection → universal lessons
3. Merge + rank → hiển thị top 5
```

---

## ⚠️ LƯU Ý QUAN TRỌNG

```
BẢO MẬT:
  - KHÔNG store password, API key thật vào Qdrant
  - Nội dung Qdrant có thể bị truy cập nếu VPS bị hack
  - Luôn dùng API key cho Qdrant (xem Bước 1.3)
  - Firewall chặn port 6333 với IP lạ

BACKUP:
  - ~/qdrant_storage/ = toàn bộ data → backup folder này định kỳ
  - docker cp qdrant:/qdrant/storage ./qdrant_backup

COST:
  - Qdrant: Free (self-hosted)
  - Embedding: Gemini text-embedding-004 = Free tier có sẵn
  - VPS: Tận dụng VPS payroll đang có → $0 thêm
```

---

## 📋 CHECKLIST CÀI ĐẶT

```
[ ] Bước 1: Qdrant Docker chạy trên VPS — curl health OK
[ ] Bước 1.3: API key đã set, firewall đã cấu hình
[ ] Bước 1.4: Collections đã tạo (payroll_trieuphu + global_patterns)
[ ] Bước 2.1: mcp-server-qdrant đã cài trên máy local
[ ] Bước 2.2: .gemini/settings.json đã cấu hình MCP
[ ] Bước 2.3: qdrant_store và qdrant_find tools xuất hiện trong Antigravity
[ ] Bước 3: Test store + find hoạt động đúng
```
