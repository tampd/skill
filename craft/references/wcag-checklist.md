# WCAG 2.2 AA Checklist

## PERCEIVABLE
- [ ] Text alternatives: mọi img có alt
- [ ] Color contrast: ≥ 4.5:1 text, ≥ 3:1 large text
- [ ] Info không phụ thuộc chỉ vào color

## OPERABLE
- [ ] Keyboard accessible: Tab order logic
- [ ] Focus visible: rõ ràng
- [ ] Touch target ≥ 44×44px
- [ ] No keyboard traps (trừ modal)

## UNDERSTANDABLE
- [ ] lang attr trên `<html>`
- [ ] Error messages specific, actionable
- [ ] Labels associated with inputs

## ROBUST
- [ ] Valid HTML (W3C validator)
- [ ] ARIA dùng đúng role/property
- [ ] axe DevTools: 0 violations

## Automated Test
```typescript
import { axe, toHaveNoViolations } from 'jest-axe'
expect.extend(toHaveNoViolations)

it('has no axe violations', async () => {
  const { container } = render(<Page />)
  expect(await axe(container)).toHaveNoViolations()
})
```
