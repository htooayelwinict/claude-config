---
description: |
  Run, write, and fix tests using TDD approach with Pest/Vitest/Playwright.
  Examples: /test, /test PostTest, /test "all", /test "coverage"
argument-hint: [test file, pattern, or "all"]
allowed-tools: Read, Edit, Bash, Grep, Glob, mcp_gemini-bridge, mcp_open-bridge, mcp_codex-bridge, mcp_playwright, mcp_zai-mcp-server
---
# Test Mode

**Routes to:** `testing-expert` agent
**Skill:** `testing`

> Follow TDD: Write test first, verify failure, implement, verify pass, refactor.

## Target
$ARGUMENTS

## 🖼️ Visual Testing (Web Apps)

For UI/UX testing, use Playwright MCP to capture screenshots and Vision AI to analyze:

### Step 1: Navigate and Capture
```
mcp_playwright_browser_navigate(url="http://localhost:8000/page")
mcp_playwright_browser_snapshot()  # Get accessibility tree
mcp_playwright_browser_take_screenshot(filename="current-state.png")
```

### Step 2: Analyze with Vision AI
```
mcp_zai-mcp-server_analyze_image(image_path="current-state.png", prompt="Analyze this UI for...")
```

### Step 3: Visual Regression
- Compare screenshots before/after changes
- Verify layout, colors, spacing match design
- Check responsive behavior at different viewports

## TDD Workflow

1. **Write Test First**
   - Define expected behavior
   - Create failing test

2. **Verify Failure**
   ```bash
   php artisan test --filter=TestName
   ```

3. **Implement**
   - Write minimal code to pass

4. **Verify Pass**
   ```bash
   composer test
   ```

5. **Refactor**
   - Clean up while tests stay green

## Test Commands

### Laravel (Pest)
```bash
composer test                        # All tests
php artisan test --filter=PostTest   # Specific test
php artisan test --coverage          # Coverage report
php artisan test --parallel          # Parallel execution
```

### Python (Pytest)
```bash
python -m pytest                     # All tests
python -m pytest tests/api -k login  # Pattern
python -m pytest --cov=src --cov-report=term-missing
```

### React (Vitest)
```bash
npm run test                         # All tests
npm run test:watch                   # Watch mode
npm run test:coverage                # Coverage report
npm run test -- PostForm             # Specific test
```

### E2E (Playwright)
```bash
npx playwright test                  # All E2E tests
npx playwright test --ui             # Interactive mode
npx playwright test login.spec.ts    # Specific test
```

## Output Format

```markdown
## 🧪 Test Results

### Summary
✅ X tests passed
❌ Y tests failed

### Failed Tests
1. `TestName` — [Error message]

### Coverage
- Lines: X%
- Functions: Y%

### Recommendations
- [ ] Add test for edge case
```

## Quick Actions

| Action | Command |
|--------|---------|
| Run all | `composer test && npm run test` |
| Watch | `npm run test:watch` |
| Coverage | `php artisan test --coverage` |
| Python all | `python -m pytest` |
| Python cov | `python -m pytest --cov=src --cov-report=term-missing` |
