# DeployForge Testing Guidelines

Thank you for contributing to DeployForge! Consistent testing helps us maintain quality while rapidly developing new features. These guidelines ensure our tests remain valuable, reliable, and maintainable.

## Table of Contents
- [Types of Tests](#types-of-tests)
- [Best Practices](#best-practices)
- [Test Data Guidelines](#test-data-guidelines)
- [Infrastructure Testing](#infrastructure-testing)
- [Code Coverage](#code-coverage)
- [Running Tests Locally](#running-tests-locally)
- [Common Pitfalls to Avoid](#common-pitfalls-to-avoid)

## Types of Tests

### 1. Unit Tests
**Scope:** Isolated functions/modules (mock all external dependencies)  
**Location:** `src/**/__tests__/*.unit.test.ts`  
**Framework:** Jest  
```typescript
// Example unit test structure
describe('ConfigParser', () => {
  it('should handle empty YAML gracefully', () => {
    const parser = new ConfigParser();
    expect(parser.load('')).toThrow(InvalidConfigError);
  });
});
```

### 2. Integration Tests
**Scope:** Component interactions (mock only external services)  
**Location:** `src/**/__tests__/*.integration.test.ts`  
**Framework:** Jest + Supertest  
**Note:** Use `testcontainers` for database tests

### 3. End-to-End (E2E) Tests
**Scope:** Full user workflows across subsystems  
**Location:** `e2e/**/*.test.ts`  
**Framework:** Playwright  
**Data:** Always reset to baseline state before each test

### 4. Infrastructure Tests
**Scope:** Terraform/CloudFormation template validation  
**Tools:** `terraform validate`, `cfn-lint`, `checkov`  
**Location:** `infra/**/__tests__/*.test.ts`

## Best Practices

✅ **Naming Convention:**  
`<filename>.<test-type>.test.ts`  
`describe('ClassName') → it('should do X when Y condition occurs')`

✅ **Test Isolation:**  
- Clear all mocks after each test (`afterEach(jest.clearAllMocks)`)
- Never share state between tests
- Use `jest.spyOn()` instead of direct module mocking

✅ **Assertions:**  
- One logical assertion per test
- Validate error messages explicitly
- Use `.toThrow(errorInstance)` for error testing

✅ **Performance:**  
- Unit tests: < 100ms/test
- Integration tests: < 2s/test
- E2E tests: < 10s/test

❌ **Anti-Patterns:**  
- `console.log` in tests
- Unconditional `setTimeout`
- Test logic in `describe` blocks

## Test Data Guidelines

1. Always use synthetic data (never real credentials/production snapshots)
2. Clean up cloud resources within test teardown:
```typescript
afterAll(async () => {
  await destroyTestStack();
});
```
3. For consistency:
```typescript
const TEST_USER = Object.freeze({
  id: 'user_123',
  role: 'viewer'
});
```

## Infrastructure Testing

### Cloud Resources
Use our `MockCloudProvider` class except for designated live tests:
```typescript
const cloud = new MockCloudProvider({
  simulateLatency: true,
  failureRate: 0.1
});
```

### Security Checks
Required in all infrastructure tests:
```bash
# Run in CI pipeline
checkov -d infra/
tfsec
```

### Live Environment Tests
1. Tag with `@live` (only run in nightly build)
2. Must clean up ALL resources post-execution
3. Never run against production accounts

## Code Coverage

| Threshold     | Requirement                  |
|---------------|------------------------------|
| **Minimum**   | 70% (unit), 50% (integration)|
| **Goal**      | 85% (unit), 65% (integration)|
| **Critical**  | 100% for error handlers      |

Generate report locally:
```bash
npm run test:coverage
```

## Running Tests Locally

1. Start dependencies:
```bash
docker-compose -f dev/docker-compose.test-deps.yml up
```

2. Run specific test suite:
```bash
npm run test:unit -- src/core/__tests__/config.integration.test.ts
```

3. Update snapshots cautiously:
```bash
npm run test:unit -u -- path/to/test.file.test.ts
```

4. Profile slow tests:
```bash
npm run test:perf -- --logHeapUsage
```

**Recommended Workflow:**
```bash
# Watch mode during development
npm run test:watch
```

## Common Pitfalls to Avoid

**Flaky Tests:**  
- Never use random/fuzzy data without seeding
- Avoid time-based assertions without mocking clocks
- Retry mechanisms only in E2E tests (max 2 retries)

**Resource Leaks:**  
- Always implement cleanup in `afterAll`/`afterEach`
- Use `--detectOpenHandles` in Jest
- Verify AWS/Azure resource cleanup via provider dashboards

**False Positives:**  
- Validate negative cases (ensure test can fail)
- Avoid over-mocking (mock only what's necessary)
- Regularly audit tests marked `skip`/`only`

**Performance Issues:**  
- Mock network calls (`nock` for HTTP)
- Limit DOM operations in unit tests
- Run database tests against local containers only

Let's build reliability together! Reach out in our #testing Slack channel with questions.