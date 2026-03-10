# Onboarding Template — New Developer Guide

```markdown
# 👋 ONBOARDING — [Project Name]
> For new team members | Updated: YYYY-MM-DD

## Day 1: Setup & Explore

### 1. Get Access
- [ ] GitHub repo access (ask Team Lead)
- [ ] Database credentials (ask DevOps)
- [ ] Monitoring dashboards (ask DevOps)
- [ ] Slack channels: #dev, #incidents, #product

### 2. Clone & Run (< 10 minutes)
  ```bash
  git clone git@github.com:org/repo.git
  cd repo
  cp .env.example .env
  # Edit .env with your credentials
  npm install
  npm run dev
  ```

### 3. Read Essential Docs
- [ ] README.md — Quick overview
- [ ] ARCHITECTURE.md — System design (15 sections)
- [ ] LESSONS.md — Known gotchas (READ THIS!)

## Day 2-3: Understand the Code

### 4. Key Files to Study
| File | Why | Time |
|------|-----|------|
| [file 1] | Core business logic | 30min |
| [file 2] | Main entry point | 15min |
| [file 3] | Data models | 20min |

### 5. Run the Test Suite
  ```bash
  npm test
  # Understand: what's tested, what's not
  ```

### 6. Try Common Tasks
- [ ] Create a test API endpoint
- [ ] Add a database column (migration)
- [ ] Fix a CSS issue
- [ ] Deploy to staging

## Day 4-5: First Contribution

### 7. Pick a Starter Task
- Check NEXT-TODO.md for "good first issue" items
- Ask team lead for a suitable starter task

### 8. Follow the Workflow
  ```
  /start [task]     → Load context
  /build [task]     → TDD implementation
  /save             → Quality gate + commit
  ```

## Questions?
| Topic | Who to Ask |
|-------|------------|
| Architecture | [name] |
| Business rules | [name] |
| DevOps/infra | [name] |
```
