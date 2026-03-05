# LESSONS — Skill Repository

> **Mục đích**: Ghi nhận bugs, bài học, quy tắc rút ra. KHÔNG BAO GIỜ xóa entries.
> **Format**: #WARN-NNN — mỗi entry có triệu chứng + root cause + fix (✅❌ code) + quy tắc.

---

## 📊 Thống kê

| Tổng entries | 🔴 Critical | 🟡 Warning | 🟢 Info |
|---|---|---|---|
| 1 | 0 | 1 | 0 |

---

## Entries

### 🟡 #WARN-001 — Upgrade skills phải update cross-references (2026-03-05)

- **Mức độ:** 🟡 Warning
- **Thẻ:** #upgrade, #consistency, #cross-reference
- **Triệu chứng:** Sau upgrade 4 core skills v4.1→v5.0 (build/plan/fix/save), `/start` vẫn scan `.agent/commands/` thay vì `changes/`. Version refs cũ (v4.1) còn lại trong `/start` và `/memory`. Keyword auto-select thiếu v5.0 terms (TDD, change folder).
- **Nguyên nhân gốc:** Chỉ focus upgrade 4 skills target mà không audit cross-references trong 11 skills còn lại + AGENTS.md.
- **Cách fix:**
  ```
  // ✅ Đúng — Sau mỗi skill upgrade, chạy consistency check:
  grep -rn "v4.1" /root/skill/*/SKILL.md  # catch stale version refs
  grep -rn "blueprint\|\.agent/commands" /root/skill/*/SKILL.md  # catch old refs

  // ❌ Sai — Chỉ sửa files target, không check impact
  # Sửa build/SKILL.md nhưng quên start/SKILL.md vẫn scan .agent/commands/
  ```
- **Quy tắc rút ra:** Mỗi khi upgrade N skills → **BẮT BUỘC** grep toàn bộ repo tìm refs cũ + test /start output.
- **File liên quan:** `start/SKILL.md`, `memory/SKILL.md`, `AGENTS.md`
