# CHANGE LOG — Skill Repository

## 2026-03-14

### Changed
- **APEX v4.2 — Biomimetic Memory Upgrade (Hindsight-inspired)**
  - **Rule 26 — BIOMIMETIC MEMORY**: Mỗi memory entry PHẢI có `type`: `world` | `experience` | `mental_model`
    - `world` = facts về tech/framework/tool (tĩnh)
    - `experience` = trải nghiệm debug/build/deploy cụ thể
    - `mental_model` = pattern tổng hợp từ nhiều experiences (reflect output)
  - **3-Strategy Recall**: semantic + keyword + temporal (thay vì chỉ vector search)
    - Merge top-5 từ 3 strategies, ưu tiên entries match ≥2 strategies
  - **Reflect Auto-Trigger**: `/consolidate` BƯỚC 2.5 — LLM reflect trên ≥3 experiences cùng domain → tạo mental_model mới
  - **Type-Aware Retrieval**: `/build` ưu tiên world+mental_model, `/fix` ưu tiên experience+mental_model
  - **Enriched Qdrant Metadata**: thêm `memory_type`, `timestamp`, `related_entries`
  - Research-based: vectorize-io/hindsight (SOTA agent memory, LongMemEval benchmark)
  - Files changed: GEMINI.md, learn/SKILL.md, session/SKILL.md, build/SKILL.md, fix/SKILL.md, LESSONS.md, CHANGE_LOG.md

### Changed
- **APEX v4.1 — Superpowers Integration**
  - **Verification Gate** (Rule 24): NO completion claims without FRESH evidence. Anti-rationalization table.
  - **Subagent Orchestration** (Rule 25): Fresh subagent per task + 2-stage review (spec → quality).
  - **New file: `build/references/subagent-prompts.md`**: 3 prompt templates (Implementer, Spec Reviewer, Code Quality Reviewer) + status protocol (DONE/CONCERNS/CONTEXT/BLOCKED) + orchestrator checklist
  - **Model selection strategy** in `build/references/parallel-guide.md`: mechanical → cheap, integration → standard, architecture → capable
  - **Anti-rationalization tables**: Added to `fix/SKILL.md` Phase 3 (debugging) + `session/SKILL.md` /save STEP 2 (verification)
  - **README.md**: Complete rewrite for APEX v4.1
  - Research-based: obra/superpowers (6.9k⭐, MIT) — best of breed agentic skills framework
  - Files new: build/references/subagent-prompts.md
  - Files changed: GEMINI.md, session/SKILL.md, fix/SKILL.md, build/references/parallel-guide.md, README.md, CHANGE_LOG.md

### Changed
- **APEX v4.0 Cleanup** — Comprehensive audit & fixes
  - **Deleted 9 nested duplicate directories** (`skill/skill/`) containing stale v3.0 copies
    - All 9 skills had duplicate nested SKILL.md + references/ with old version refs
    - Nested copies missing v4.0 features: Ultrathink, Context Health, Progressive Disclosure
  - **Added `/e2e` section** to `craft/SKILL.md` (was listed in GEMINI.md but missing implementation)
    - Multi-breakpoint screenshots, keyboard navigation, interaction tests
  - **Rewrote `session/references/trigger-keywords.md`** — now covers all 35 commands
    - Fixed stale refs: `blueprint` → `change folder`, `/build --mode plan` → `/plan`
    - Added missing commands: `/gsd`, `/e2e`, `/tokens`, `/audit`, `/mcp`, `/quality`, etc.
  - **Verified references files** — `fix/references/patterns.md` and `craft/references/patterns.md` contain full v3.0 inline content
  - Files changed: craft/SKILL.md, session/references/trigger-keywords.md, CHANGE_LOG.md
  - Files deleted: 9 nested directories (automate/automate, build/build, content/content, craft/craft, fix/fix, learn/learn, secure/secure, session/session, spec/spec)

### Changed
- **APEX v4.0** — Progressive Disclosure + GSD + Ultrathink + Context Health
  - **Progressive Disclosure**: references/ folders cho build, fix, craft skills
    - SKILL.md body giữ core workflow (~compact), verbose content → references/
    - `build/references/patterns.md` + `build/references/parallel-guide.md`
    - `fix/references/patterns.md` (bug classification, common patterns, browser agent)
    - `craft/references/patterns.md` (component docs, token structure, audit detail)
  - **GSD Command** (`/gsd`): Full cycle Discuss → Plan → Execute → Verify
    - Fresh context checkpoint giữa phases
    - Không skip Phase V (Verify)
  - **Ultrathink Mode**: prefix "ultrathink:" cho complex tasks
    - Architecture decisions, complex bugs, refactoring >5 files
    - Full analysis plan (≥500 words) trước khi code
  - **Context Health Monitor**: 🟢/🟡/🔴/💀 tracking
    - Hiển thị trong /start output
    - Auto-suggest /save + fresh session khi Heavy/Critical
  - **3 Rules mới** (21-23): ULTRATHINK GATE, CONTEXT HEALTH MONITOR, GSD CYCLE
  - **Sub-Agent Orchestration**: Formalized trong build/references/parallel-guide.md
  - Research-based: GSD2 (23k⭐), Anthropic SKILL.md patterns, Claude Code docs
  - Files changed: GEMINI.md, build/SKILL.md, fix/SKILL.md, craft/SKILL.md, session/SKILL.md

## 2026-03-10

### Changed
- **APEX v3.0 Memory** — 4-Layer Smart Retrieval upgrade
  - Layer A (Rules): GEMINI.md + STATE.md — luôn đọc
  - Layer B (Critical Lessons): LESSONS.md giới hạn ≤10 entries (importance ≥0.8)
  - Layer C (Semantic): Qdrant Vector DB — semantic search, cross-project
  - Layer D (Auto-Memory): .ai/memory/MEMORY.md — AI tự ghi (200-line rule)
  - LESSONS_ARCHIVE.md: lưu lessons importance < 0.8 (Qdrant searchable)
  - `/start`: 4-layer smart load (static + Qdrant + auto-memory)
  - `/save`: embed lessons mới vào Qdrant + auto-memory update
  - `/fix`: Qdrant semantic match cho error patterns
  - `/build`: Qdrant search trong MEMORY LOAD step
  - `/learn`: STORE+EMBED + auto-archive khi >10 entries
  - Templates mới: `MEMORY.md`, `LESSONS_ARCHIVE.md`
  - Research-based: Claude Code MEMORY.md, Cursor context layering, ReMe, MemOS

## 2026-03-10

### Changed
- **APEX v2.0** — Full system upgrade from AG-SKILL v2.0 to APEX v2.0
  - 6-Layer Memory Architecture (+Layer 0 Ingest, +Layer 5 Consolidation)
  - 35 commands (up from 32): +`/consolidate`, `/cross-link`, `/recall`
  - INSIGHTS.md: compound insights from memory consolidation
  - Importance Scoring (0.0–1.0) for LESSONS entries
  - Auto-Ingest tagging (Entity/Topic/Importance)
  - Sleep-Brain Consolidation Pass
  - Combo Skills workflow table
  - ACTIVE_CONTEXT.md standardized schema
  - Skills upgraded: session (/recall, 6-Layer Bootstrap), build (Memory Load), fix (Auto-Ingest), learn (/consolidate, /cross-link)
  - Templates added: `INSIGHTS.md`, `ACTIVE_CONTEXT.md`
  - Synthesized from: AG-SKILL v2.0 + Google ADK memory-agent + APEX v1.0 draft

### Removed
- `ag-skill/` subfolder (duplicate of root skills)
- `AG-SKILLv2.zip`, `skill-mainv6.1.zip` (old archives)
- `skill_memory.md`, `Bkns Dev Operating System Packv2.docx` (integrated into templates)
- `APEX-SKILL-SYSTEM.md` (integrated into GEMINI.md + skills)

## 2026-03-06

### Added
- **v6.2.1 UNIFIED SPEC STANDARD** — Chuẩn hóa quy trình viết SPEC theo BKNS
  - Hệ thống SPEC 3 tầng thống nhất:
    - **Full SPEC** (19 sections) → Dự án mới / Module lớn → `SPEC.md` ở root
    - **Feature SPEC** (10 sections) → Feature ≥ 3 files → `changes/<name>/SPEC.md`
    - **Mini-SPEC** (bảng 10 dòng) → Task ≤ 2 files → inline
  - Section numbers đồng nhất 1-19 giữa các tầng cho tính kế thừa
  - **Files thay đổi chi tiết** (cho rollback):

  | Action | File | Mô tả |
  |---|---|---|
  | REWRITE | `templates/SPEC.md` | Thêm 3-tier system header, enriched Full SPEC, thêm Feature SPEC template, cập nhật Mini-Spec |
  | REWRITE | `plan/SKILL.md` Step 2 | Change folder: gộp `proposal.md` + `specs/spec.md` + `design.md` → 1 file `SPEC.md` (Feature SPEC) |
  | MODIFY | `build/SKILL.md` Step 0.5 | `.spec.md` (6 sections frontend) → BKNS SPEC reference (3 tầng) |
  | MODIFY | `save/SKILL.md` Step 4 | Archive verify: thêm check SPEC.md trạng thái Done |
  | MODIFY | `GEMINI.md` docs table | `changes/` description: "proposal + specs + design + tasks" → "SPEC.md + tasks + delta-specs" |
  | MODIFY | `CHANGE_LOG.md` | Thêm entry v6.2.1 chi tiết cho rollback |

  - **Files REMOVED từ change folder template**: `proposal.md`, `specs/spec.md`, `design.md`
  - **Files GIỮA NGUYÊN trong change folder**: `tasks.md`, `delta-specs.md`
  - **Rollback**: Revert commit chứa "feat(spec): unified SPEC standard v6.2.1"

### Added
- **v6.2 BKNS-ALIGNED** — Chuẩn hóa theo BKNS Dev Operating System Pack v2
  - Rule 14: BKNS Repo Standard — 6 files bắt buộc + naming convention
  - +8 templates: SPEC (19 sections + Mini-Spec), DEPLOYMENT (11 sections), PROJECT-META, README-BKNS, CHANGELOG (Keep a Changelog), WEEKLY-REPORT, PROJECT-AUDIT, REPO-STRUCTURE
  - `/docs init bkns` — khởi tạo repo BKNS chuẩn từ templates
  - `/start` — thêm BKNS compliance check (6 mandatory files)
  - `/plan` — thêm BKNS SPEC template cho dự án mới
  - `/save` — thêm DEPLOYMENT.md, PROJECT-META.md, SPEC.md vào docs update
  - `/ship check` — thêm BKNS release checklist (10 items)
  - Updated docs table (Skill System Files + BKNS Repo Standard Files)

### Added
- **v6.1 SELF-REASONING GATE** — Global Rule 13
  - 3-Question Self-Check trước MỌI quyết định thực thi
  - Q1: "Đây đã là phương án tốt nhất chưa?" (≥2 alternatives)
  - Q2: "Có risk/side-effect nào đang bỏ qua?" (breaking, regression, perf, security)
  - Q3: "User có cần approve không?" (scope-based auto-decide)
  - Embedded: `/build` Step 1.5, `/fix` Phase 3, `/craft` Step 2.5, `/plan` Q2 enhancement
  - Updated: GEMINI.md (13 Rules), AGENTS.md

## 2026-03-05

### Changed
- **v6.0 MEMORY-FIRST ARCHITECTURE** — Gộp thông minh 18→14 skills
  - 🆕 `/craft`: Gộp design + frontend → UI lifecycle (setup/component/css/a11y)
  - 🆕 `/quality`: Gộp guard + perf + review-website → quality gate (test/a11y/perf/all)
  - 🆕 `/ship`: Pre-launch checklist + CI/CD + Monitoring + Rollback
  - ⬆ `/plan`: Gộp brainstorm → Ideation Step 0
  - ⬆ `/memory`: Context Compression Protocol + 3-Tier Load Order
  - +4 Global Rules (9-12): A11y WCAG AA, Security Headers, Design Tokens, Perf Budget
  - Website Development Workflow
  - Zero-overlap documentation model
  - Archived: design→craft, guard→quality, brainstorm→plan, review-website→quality

### Added
- `LESSONS.md` — Bài học đầu tiên: #WARN-001 cross-reference consistency
- `CHANGE_LOG.md` — File theo dõi thay đổi theo timeline

### Changed
- **v5.0 HYBRID UPGRADE** — 4 core skills nâng cấp (build, plan, fix, save)
  - `/build`: TDD Iron Law (RED→GREEN→REFACTOR) + Spec Compliance Check
  - `/plan`: Change Folder + Brainstorming Hard Gate
  - `/fix`: 4-Phase Systematic Debugging + Root Cause Iron Law
  - `/save`: 2-Stage Review + Change Archive + Delta Merge
- **Self-audit fixes** — 9 bugs + 6 improvements
  - `/start`: scan `changes/`, keyword table v5.0, version fix
  - `/memory`: version v4.1 → v5.0
  - `/guard`: TDD Rule 6 note
  - `/brainstorm`: v5.0 change folder output
  - `/design`: TDD exception note
  - `/integrate`: change folder reference
  - `AGENTS.md`: v5.0 Core Rules section
  - `GEMINI.md`: v5.0 header, Rules 6-8, workflow, cheat sheet

## 2026-03-04

### Changed
- v4.1 HYBRID MEMORY: +1 skill (/memory). Layer 5 Beads + Layer 4 Qdrant.
- v4.0 STRUCTURED VIBE: +2 skills (/docs, /seo). Architecture Spec Phase.
- v3.0 EXTENDED: +4 skills (brainstorm, n8n-pro, web-security, review-website).

## 2026-02-28

### Changed
- v2.0: Gộp 26 skills → 8 unified skills. Thêm /plan, /guard.
