# ADR Template — Architecture Decision Records

## Format

```markdown
# ADR-NNN: [Decision Title]

- **Date**: YYYY-MM-DD
- **Status**: Proposed | Accepted | Deprecated | Superseded by ADR-XXX
- **Deciders**: [ai đưa ra quyết định]

## Context
[Tại sao cần quyết định này? Vấn đề gì đang gặp?]

## Decision
[Chọn gì? Mô tả chi tiết]

## Alternatives Considered
| Option | Ưu điểm | Nhược điểm |
|---|---|---|
| A: [chosen] | [pros] | [cons] |
| B: [rejected] | [pros] | [cons] |

## Consequences
### Tích cực
- [positive effect]

### Tiêu cực / Rủi ro
- [negative effect / risk]

## References
- [links, tickets, PRs]
```

## Khi nào tạo ADR

- Chọn framework/library mới
- Thay đổi kiến trúc (monolith → microservice, thêm queue, v.v.)
- Quyết định database schema quan trọng
- Thay đổi auth/security model
- Bất kỳ quyết định nào mà "tại sao" không rõ ràng từ code

## Quy tắc

- ADR KHÔNG BAO GIỜ XÓA — chỉ thay đổi status
- Status flow: Proposed → Accepted → Deprecated / Superseded
- NNN tăng dần, scan docs/adr/ để lấy số tiếp theo
- File name: `ADR-NNN-kebab-case-title.md`
