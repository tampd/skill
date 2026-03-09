# Trigger Keywords — Skill Auto-Select

| Từ khóa trong task | Skill được chọn |
|---|---|
| code, feature, thêm, tạo, viết, build, module, TDD | `/build` |
| plan, thiết kế, spec, blueprint, roadmap, proposal, brainstorm, ý tưởng | `/build --mode plan` |
| fix, bug, lỗi, sửa, error, debug, crash, root cause | `/fix` |
| design, UI, giao diện, CSS, theme, layout, component, frontend, responsive, token | `/craft` |
| test, coverage, lint, audit, a11y, accessibility, WCAG, axe, performance, lighthouse | `/craft --mode audit` |
| deploy, launch, ship, CI/CD, production, pre-launch, checklist, monitor | `/secure --mode ship` |
| bảo mật, security, OWASP, CVE, hack, pentest | `/secure` |
| API, webhook, payment, integrate, 3rd-party | `/automate --mode integrate` |
| n8n, workflow, automation, trigger, queue | `/automate --mode n8n` |
| SEO, viết bài, content, keyword, GEO, article, blog | `/content --mode seo` |
| docs, tài liệu, document, ADR, handoff, onboard | `/content --mode docs` |
| spec, architecture, kiến trúc, data flow, api reference | `/spec` |
| save, kết thúc, đóng phiên, commit, push | `/save` |
| memory, checkpoint, recall, bộ nhớ | `/checkpoint` |
