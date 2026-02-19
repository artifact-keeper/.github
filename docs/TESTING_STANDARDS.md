# Artifact Keeper Testing Standards

This document defines the testing standards, practices, and expectations across all artifact-keeper repositories. It serves as the single source of truth for QA processes.

## Test Pyramid

Each repo should follow the test pyramid principle: more unit tests than integration tests, more integration tests than E2E tests.

| Layer | Target Ratio | Purpose |
|-------|-------------|---------|
| Unit | 70% | Fast, isolated tests for individual functions and modules |
| Integration | 20% | Tests that verify component interactions (DB, API calls) |
| E2E | 10% | Full user-flow tests through the real UI or CLI |

## Repo-Specific Requirements

### Backend (artifact-keeper)
- **Required CI checks:** `lint-rust`, `test-backend-unit`, `ci-complete`
- **Coverage target:** 60% line coverage
- **Test frameworks:** Rust built-in testing, reqwest for HTTP tests
- **E2E:** 28 native client scripts covering 12 package formats

### Web Frontend (artifact-keeper-web)
- **Required CI checks:** `lint`, `build`, `e2e`
- **Coverage target:** All routes have at least one Playwright E2E spec
- **Test frameworks:** Playwright for E2E, ESLint for static analysis
- **E2E:** 44 Playwright spec files

### iOS/macOS (artifact-keeper-ios)
- **Required CI checks:** `build`
- **Coverage target:** Unit tests for all API client methods and data models
- **Test frameworks:** XCTest, XCUITest (planned)

### Android (artifact-keeper-android)
- **Required CI checks:** `lint`, `build`, `test`
- **Coverage target:** Unit tests for all ViewModels and API client methods
- **Test frameworks:** JUnit, Mockk, Compose UI Test (planned)

### API Spec (artifact-keeper-api)
- **Required CI checks:** `lint`, all 5 SDK builds
- **Coverage target:** All SDK builds pass, no breaking changes undetected
- **Test frameworks:** Spectral for linting, SDK generators for build verification

### CLI (artifact-keeper-cli)
- **Required CI checks:** `check`, `test`
- **Test frameworks:** Rust built-in testing

### Plugin (artifact-keeper-example-plugin)
- **Required CI checks:** `check`
- **Test frameworks:** Rust built-in testing, WASM build verification

### Site (artifact-keeper-site)
- **Required CI checks:** `build`
- **Coverage target:** No broken links, Lighthouse score > 90

### IaC (artifact-keeper-iac)
- **Required CI checks:** (to be added)
- **Coverage target:** All Helm charts lint, all Terraform configs validate

## Naming Conventions

### Rust
```
test_<what>_<condition>_<expected>
// Example: test_parse_maven_pom_valid_xml_returns_artifact
```

### Playwright (TypeScript)
```
<feature>.spec.ts
// Example: service-accounts.spec.ts
```

### Swift (XCTest)
```
test<What><Condition><Expected>()
// Example: testAPIClientFetchArtifactsReturnsListSuccessfully()
```

### Kotlin (JUnit)
```
`<what> <condition> <expected>`()
// Example: `fetchArtifacts with valid token returns list`()
```

## CI Requirements

### All PRs Must Pass
1. **Lint** - Code style and static analysis
2. **Build** - Compilation and type checking
3. **Tests** - All existing tests pass

### Additional Requirements by Change Type
- **API handler changes:** Unit tests cover new logic, OpenAPI annotations present
- **UI page changes:** Playwright E2E spec covers the change
- **Migration changes:** Migration is reversible, reviewed by backend team
- **Breaking API changes:** All 5 SDK builds pass, documented in changelog

## Test Data Management

- Use fixtures and factories, never production data
- Databases in CI are ephemeral (Docker containers with tmpfs)
- Test data seed scripts live in `scripts/` or `e2e/fixtures/`
- Secrets in tests use environment variables, never hardcoded values

## Cross-Repo Testing

The artifact-keeper ecosystem has a dependency chain that requires coordinated testing:

```
Backend API change
  -> Export OpenAPI spec
  -> Generate 5 SDKs (TypeScript, Kotlin, Swift, Rust, Python)
  -> Web frontend consumes TypeScript SDK
  -> iOS app consumes Swift SDK
  -> Android app consumes Kotlin SDK
  -> CLI consumes Rust SDK
```

### Cross-Repo PR Dependencies
Use Mergify `Depends-On` headers to gate PRs across repos:
```
Depends-On: https://github.com/artifact-keeper/artifact-keeper-api/pull/42
```

### Release Testing Flow
1. Backend passes all CI + E2E
2. OpenAPI spec synced to API repo, all SDK builds pass
3. Web E2E passes against new backend
4. Mobile builds succeed with updated SDKs

## Agent-Assisted QA

Six Claude agent definitions are checked into repos for on-demand quality audits:

| Agent | Repo | Purpose |
|-------|------|---------|
| Security Auditor | `artifact-keeper` | Review code for security vulnerabilities |
| Dependency Health | `artifact-keeper` | Audit dependencies across all repos |
| Test Coverage | `artifact-keeper-web` | Identify untested code paths |
| Feature Parity | `artifact-keeper-web` | Compare web vs mobile feature sets |
| E2E Regression | `artifact-keeper-web` | Identify affected tests after changes |
| API Validator | `artifact-keeper-api` | Verify spec matches implementation |

### Invoking Agents
Agents are defined in `.claude/agents/<name>.md` in their respective repos. They can be invoked:

1. **On demand:** Read the agent file and follow its analysis procedure
2. **During code review:** Reference the agent's checklist when reviewing PRs
3. **Periodic audits:** Run agents monthly for health checks

## Review Checklist for Reviewers

When reviewing a PR, verify:

1. **Tests exist** for the change (unit, integration, or E2E as appropriate)
2. **Tests are meaningful** (not just asserting true === true)
3. **Edge cases** are covered (empty inputs, error states, boundary values)
4. **Test names** follow naming conventions
5. **No test pollution** (tests don't depend on execution order or shared state)
6. **CI passes** all required checks
7. **PR template** checklist items are checked

## Reporting

### Test Results
- CI test results visible on every PR via GitHub status checks
- Playwright HTML reports uploaded as artifacts on the web repo
- Backend test output available in CI logs

### Metrics to Track
- Test count per repo (trend over time)
- CI pass rate (target: >95%)
- Time to green (target: <10 min for lint+build+unit, <20 min for E2E)
- Flaky test rate (target: <2%)
