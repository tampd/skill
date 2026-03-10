---
name: content
description: SEO content creation and blog publishing pipeline. Use for /seo (SEO-optimized article), /article (blog post), /brief (content brief), /audit-content (content SEO audit). Triggers on "SEO", "bài viết", "article", "blog", "content", "viết bài", "keyword", "từ khóa".
---

# Content Skill — SEO Pipeline + Content Engine

## /brief [topic or keyword]
> Research và lập content brief trước khi viết

```
RESEARCH PHASE:
  □ Keyword analysis: search volume, difficulty, intent
  □ SERP analysis: top 10 results đang cover gì?
  □ Content gap: gì họ có nhưng thiếu? Gì họ không có?
  □ Search intent: informational / navigational / transactional / commercial?
  □ Related keywords: LSI, long-tail variations

BRIEF STRUCTURE:
  Target keyword: [primary]
  Secondary keywords: [list 5-10]
  Search intent: [type]
  Target audience: [persona]
  Word count target: [range based on SERP]
  Required sections: [H2 list]
  Must-answer questions: [People Also Ask]
  Tone: [technical/casual/professional]
  CTA: [what action after reading?]
```

---

## /article [topic or brief]
> Viết bài chuẩn SEO, E-E-A-T, đúng format WordPress

```
PRE-WRITING:
  □ /brief đã chạy? Nếu không → chạy brief trước
  □ INSTINCTS.md: có pattern viết bài liên quan?

STRUCTURE (SEO-optimized):
  □ Title (H1): target keyword trong 60 chars, compelling
  □ Meta description: 150-160 chars, include keyword, có CTA
  □ Introduction (150-200 words):
      - Hook: câu hỏi / stat / pain point
      - Problem acknowledgment
      - Promise: bài này giải quyết gì
      - NO keyword stuffing trong 2 câu đầu
  
  □ Body (H2/H3 structure):
      - H2s chứa secondary keywords tự nhiên
      - Mỗi section: 200-400 words
      - Bullet points / numbered lists cho scanability
      - Internal linking: 2-3 links relevant
      - External linking: 1-2 authoritative sources
      - Images: descriptive alt text với keyword
  
  □ Conclusion:
      - Tóm tắt key points
      - CTA rõ ràng (comment / share / next article)
      - FAQ section nếu relevant (rich snippet potential)

E-E-A-T SIGNALS:
  □ Author expertise hiện rõ (nếu có byline)
  □ Cite sources cụ thể (không "một nghiên cứu nào đó")
  □ Cập nhật ngày publish + last updated
  □ Practical examples từ experience
  □ Unique insight (không paraphrase lại những gì có sẵn)

READABILITY:
  □ Flesch-Kincaid: grade 8-12 (technical: có thể cao hơn)
  □ Sentence length: avg <25 words
  □ Paragraph: max 3-4 sentences
  □ Transition words: 30%+ sentences
  □ Active voice: 80%+

TECHNICAL SEO:
  □ Keyword density: 1-2% (không spam)
  □ Keyword trong: title, first 100 words, ≥1 H2, conclusion
  □ Schema markup: Article schema nếu có thể
  □ Canonical URL set đúng
  □ No duplicate content
```

---

## /seo [topic]
> Full SEO article với Yoast/RankMath optimization

```
YOAST / RANKMATH FIELDS (tất cả phải filled):
  □ Focus Keyphrase: exact match target keyword
  □ SEO Title: keyword đầu, ≤60 chars
  □ Meta Description: keyword, benefit, CTA, ≤160 chars
  □ Slug: keyword-only, no stop words, no dates
  □ OG Image: 1200×630px, text overlay nếu cần

CONTENT ANALYSIS TARGETS:
  □ Keyphrase in first paragraph ✓
  □ Keyphrase in SEO title ✓
  □ Keyphrase density: 0.5-2.5% ✓
  □ Outbound links: ≥1 ✓
  □ Internal links: ≥1 ✓
  □ Image alt attributes ✓
  □ Word count ≥ competitor average ✓
  □ Text/HTML ratio ≥25% ✓
  □ Subheadings (H2/H3) ✓
  □ Flesch reading ease ≥60 ✓

WORDPRESS REST API PAYLOAD:
  {
    title: "...",
    content: "...[formatted HTML]...",
    status: "draft",              // always draft, human reviews
    categories: [term_id],
    tags: [term_ids],
    meta: {
      _yoast_wpseo_title: "...",
      _yoast_wpseo_metadesc: "...",
      _yoast_wpseo_focuskw: "...",
      rank_math_focus_keyword: "..."
    },
    featured_media: wp_media_id
  }
```

---

## /audit-content [URL or post ID]
> SEO audit bài viết đã publish

```
TECHNICAL SEO:
  □ Page load <3s (Core Web Vitals)
  □ Mobile-friendly (responsive layout)
  □ HTTPS
  □ Canonical URL correct
  □ No broken links (internal + external)
  □ Images have alt text
  □ Structured data valid (Schema.org)

ON-PAGE SEO:
  □ Title: keyword present, ≤60 chars, compelling
  □ Meta description: filled, ≤160 chars
  □ H1: exists, unique, contains keyword
  □ H2/H3 structure: logical, keyword variations
  □ Content length vs top competitors
  □ Internal links: quantity và relevance
  □ Keyword density: 1-2%

CONTENT QUALITY:
  □ Last updated: nếu >12 tháng → cần refresh?
  □ Facts/stats: còn accurate?
  □ Outbound links: không bị broken / redirected?
  □ New information cần thêm?
  □ Better media (video, infographic) có thể add?

OUTPUT: Score /100 + Priority fix list
```
