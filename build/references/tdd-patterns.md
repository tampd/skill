# TDD Patterns Reference

## TDD Cycle

```
🔴 RED:    Viết test mô tả behavior mong muốn → chạy → FAIL
🟢 GREEN:  Viết code TỐI THIỂU → test PASS (KHÔNG optimize)
🔵 REFACTOR: Clean up → DRY, YAGNI, naming → test vẫn PASS → COMMIT
```

## Test Pyramid

```
Unit (70%):        Service/util functions, isolated
Integration (20%): API endpoints, DB operations
E2E (10%):         Critical user journeys
```

## PHP Test Pattern
```php
public function test_calculate_overtime_with_holiday(): void
{
    $salary = $this->service->calculateOvertime(
        baseSalary: 10000000,
        hours: 8,
        type: OvertimeType::HOLIDAY
    );
    $this->assertEquals(2400000, $salary);
}
```

## TypeScript Test Pattern
```typescript
describe('Button', () => {
  it('renders with accessible label', () => {
    render(<Button>Submit form</Button>)
    expect(screen.getByRole('button', { name: /submit/i })).toBeInTheDocument()
  })

  it('shows loading state accessibly', () => {
    render(<Button isLoading>Submit</Button>)
    expect(screen.getByRole('button')).toHaveAttribute('aria-busy', 'true')
  })
})
```

## TDD Exceptions
- **Prototype**: Exploratory code, throwaway
- **Config**: env, yaml, json configuration
- **CSS-only**: Visual changes → visual verify (screenshot)
