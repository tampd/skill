---
name: content
description: "Viết content SEO + tài liệu hướng dẫn người dùng: keyword research, GEO optimization, E-E-A-T, Schema markup, user guide, onboarding docs. Kích hoạt khi người dùng nói /seo, 'viết bài', 'SEO', 'content', 'keyword', 'blog', 'article', 'user guide', 'tài liệu hướng dẫn', 'onboarding'."
metadata:
  author: tampd
  version: 7.0.0
  category: content
---

# Content — SEO Writing & User Documentation

> **v7.0** — Gộp seo + docs (user-facing parts)
> Mục tiêu: Content rank Google + AI search + user docs dễ hiểu.

---

## 2 MODES

### Mode 1 — `/seo [topic]` (SEO + GEO Content)

```
STEP 1 — RESEARCH:
  1. Keyword Clustering:
     Primary: [volume cao nhất]
     Secondary: [3-5 từ phụ]
     LSI: [5-10 từ ngữ nghĩa]
     Long-tail: [2-3 câu hỏi]
  2. Search Intent: Informational / Transactional / Navigational
  3. SERP Analysis: top 10, content gap, featured snippet type
  4. E-E-A-T: Experience, Expertise, Authority, Trust

STEP 2 — OUTLINE:
  Title Tag: ≤ 60 chars, chứa primary keyword, compelling
  Meta Description: ≤ 155 chars, có CTA
  Heading Hierarchy: 1× H1 → H2s (secondary kw) → H3s
  Content Blocks: Intro (150-200w) → Sections → FAQ (3-5 Qs) → CTA
  Links: ≥ 3 internal + ≥ 2 external (nguồn uy tín)

STEP 3 — DRAFT (GEO-optimized):
  - Mở đầu mỗi section = 1 câu trả lời trực tiếp (AI extract được)
  - Bullet points / numbered lists / bảng so sánh
  - Cite nguồn: "Theo [Source], ..." (AI ưu tiên có citations)
  - E-E-A-T: kinh nghiệm thực, dữ liệu cụ thể, con số
  - Paragraph ≤ 3-4 câu (mobile-friendly)
  - Visual break: image/table mỗi 300 words
  > Chi tiết SEO checklist: references/seo-checklist.md

STEP 4 — OPTIMIZE:
  [ ] Keyword density: primary 1-2%, secondary 0.5-1%
  [ ] Readability: ≤ Grade 8, ≤ 20 từ/câu
  [ ] Internal links ≥ 3 per 1000 words
  [ ] URL slug: ngắn, chứa primary keyword

STEP 5 — PUBLISH CHECKLIST:
  On-Page: title, meta, H1, images alt, links
  Schema: Article, FAQ, HowTo, Breadcrumb
  Technical: canonical, OG/Twitter cards, sitemap
  Post: Submit GSC, share social, add internal links từ bài cũ
```

**Output:**
```
📝 SEO CONTENT REPORT — [Topic]
  🔑 Primary: [kw] | Secondary: [N] | Long-tail: [N Qs]
  📋 Outline: [N sections] | [N words target] | [content type]
  ✅ [Draft ✓] [Optimized ✓] [Schema ✓] [Publish ✓]
```

---

### Mode 2 — `/docs user` (User-facing Documentation)

> Tài liệu cho end-user và handoff (khác với `/spec` cho kỹ thuật nội bộ).

```
STEP 1 — SCAN: Liệt kê user docs hiện có → thiếu gì?

STEP 2 — GENERATE:

  USER_GUIDE.md:
    - Feature tour cho end-user
    - Screenshots + step-by-step
    - FAQ / troubleshooting

  ONBOARDING.md:
    - 5 phút nắm toàn cảnh
    - Core business flow
    - DO's and DON'T's (code examples ✅❌)
    - Debug commands sẵn copy-paste
    - Links tới docs khác

STEP 3 — HANDOFF PACKAGE:
  [ ] ONBOARDING.md có "5 phút nắm toàn cảnh"
  [ ] Tài khoản test đầy đủ (→ PASSWORD.md)
  [ ] Core flow mô tả rõ → người/AI zero context hiểu được
  [ ] File tree có annotation
  > Chi tiết template: references/handoff-template.md

STEP 4 — VERIFY:
  [ ] Links hoạt động? Content khớp code hiện tại?
```

---

## QUY TẮC

- KHÔNG keyword stuffing — tự nhiên, đọc mượt trước
- GEO-first — AI extract được câu trả lời trực tiếp
- E-E-A-T — kinh nghiệm thực > lý thuyết suông
- Cite sources — mọi claim cần dẫn chứng
- ONBOARDING.md viết cho "zero context readers"
- Docs SAI còn tệ hơn KHÔNG CÓ docs → luôn verify
