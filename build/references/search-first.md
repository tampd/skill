# Search-First — Research Before You Code

> Chạy trong `/build` PHASE 1, TRƯỚC khi viết code mới.
> Rule 15: SEARCH-FIRST TRƯỚC BUILD.

## Khi nào kích hoạt
- Bắt đầu feature mới mà likely có existing solution
- Thêm dependency hoặc integration
- User yêu cầu "add X functionality" → trước khi viết code
- Trước khi tạo utility/helper/abstraction mới

## Khi nào SKIP
- Đang fix bug cụ thể (đã rõ root cause)
- Task đã rõ approach (spec approved)
- Sửa CSS/UI nhỏ
- Config changes

## Workflow

```
┌─────────────────────────────────────────────┐
│  1. NEED ANALYSIS                           │
│     "Cần functionality gì cụ thể?"          │
│     Identify: language, framework, constraints│
├─────────────────────────────────────────────┤
│  2. PARALLEL SEARCH                         │
│     ┌──────────┐ ┌──────────┐ ┌──────────┐  │
│     │ Package  │ │ GitHub   │ │ Framework│  │
│     │Registry  │ │ Search   │ │ Built-in │  │
│     │npm/PyPI  │ │ stars,   │ │ solutions│  │
│     │Packagist │ │ activity │ │ docs     │  │
│     └──────────┘ └──────────┘ └──────────┘  │
├─────────────────────────────────────────────┤
│  3. EVALUATE                                │
│     Score = Functionality(30%) +            │
│             Maintenance(25%) +              │
│             Community(20%) +                │
│             Bundle Size(15%) +              │
│             License(10%)                    │
├─────────────────────────────────────────────┤
│  4. DECIDE                                  │
│     ┌─────────┐ ┌──────────┐ ┌──────────┐  │
│     │  ADOPT  │ │  EXTEND  │ │  BUILD   │  │
│     │ as-is   │ │  /Wrap   │ │  Custom  │  │
│     └─────────┘ └──────────┘ └──────────┘  │
├─────────────────────────────────────────────┤
│  5. RECORD                                  │
│     Ghi quyết định vào SPEC.md hoặc ADR    │
└─────────────────────────────────────────────┘
```

## Decision Matrix

| Signal | Action | Example |
|--------|--------|---------|
| Exact match, well-maintained, MIT/Apache | **ADOPT** — install and use directly | lodash, date-fns |
| Partial match, good foundation | **EXTEND** — install + write thin wrapper | Base library + custom adapter |
| Multiple weak matches | **COMPOSE** — combine 2-3 small packages | Several focused packages |
| Nothing suitable found | **BUILD** — write custom, but informed by research | Unique domain logic |

## Anti-Patterns

❌ Viết 500 dòng utility mà npm package làm trong 1 line
❌ Chọn package chưa maintained depuis 2 năm
❌ Chọn package quá lớn cho 1 feature nhỏ (moment.js → dùng date-fns)
❌ KHÔNG search và tự code ngay ("Not Invented Here" syndrome)
❌ Copy-paste từ StackOverflow không hiểu code
