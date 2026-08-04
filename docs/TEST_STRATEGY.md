# Test Strategy — DummyJSON API Assessment

## 1. Purpose

Define how API quality will be validated for the DummyJSON public API using Postman, with a suite that is maintainable, reviewer-friendly, and aligned with professional QA practices.

## 2. Assumptions

- DummyJSON remains publicly available at the configured `baseUrl`.
- Official demo credentials are acceptable as non-sensitive test data.
- Protected endpoints are exercised with a Bearer `accessToken` obtained via login.
- Token refresh is out of scope for this assessment.

## 3. Scope

### In scope

- Automated API tests in Postman for selected DummyJSON resources
- Positive and priority negative scenarios
- Authentication flow and authorized request reuse
- Products as a single resource module (read scenarios, then simulated write scenarios)
- Environment-driven configuration (base URL, tokens, shared IDs)
- Reusable collection/folder-level scripts to reduce assertion duplication

### Out of scope (unless explicitly added later)

- UI / end-to-end browser testing
- Performance or load testing
- Security penetration testing beyond basic auth/authorization checks
- Token refresh flows
- Full coverage of every DummyJSON endpoint
- CI/CD pipeline and Newman execution (deferred until requested)
- Project README (deferred until implementation is complete)

## 4. Target under test

| Item | Value |
|------|--------|
| API | DummyJSON |
| Base URL | `https://dummyjson.com` (via environment variable) |
| Style | REST / JSON |

## 5. Test approach

| Principle | Application |
|-----------|-------------|
| Architecture first | Folders by resource; consistent request naming |
| Reuse over copy | Shared scripts at collection/folder level; request-level scripts only for case-specific checks |
| Config externalized | URLs, tokens, and chained IDs stored in environment variables |
| Risk-based depth | Focus on assessment resources rather than exhaustive endpoint coverage |
| Traceability | Requests named by method, resource, and scenario for clear review and failure triage |

## 6. Test types

| Type | Intent |
|------|--------|
| Positive | Valid inputs and authorized flows return the expected status and critical response fields |
| Negative | Invalid credentials, missing/invalid auth, not-found, and basic validation failures |
| Contract (lightweight) | Presence and type of critical fields; avoid brittle full-schema locks unless justified |
| Chaining | Login → store token → call protected/dependent resources |

## 7. Quality standard

Every API test in this project follows the same minimum bar.

**Positive requests** validate, whenever applicable:

- HTTP status
- Response time (via `{{responseTimeLimit}}`)
- Content-Type
- Required response fields
- Data types
- Response schema (when appropriate)

**Negative requests** validate:

- Expected HTTP status
- Error response structure
- Error message (when stable)
- No unintended side effects (for example, environment variables must not be overwritten after failed authentication)

Shared checks live at collection level when they apply broadly. Request-level scripts own case-specific assertions and state updates only.

## 8. Suite organization

```
postman/
  collections/     # Postman collection JSON (one primary assessment collection)
  environments/    # Environment JSON (base URL, tokens, shared IDs)
docs/
  TEST_STRATEGY.md # This document
```

Planned collection coverage (see implementation sequence):

1. Authentication
2. Products (Read)
3. Products (Write)

## 9. Environments and data

- Use a dedicated Postman environment for local/reviewer runs
- Prefer dynamic data from responses over hardcoded fixtures when stable
- Do not commit secrets; DummyJSON demo credentials may be documented as non-sensitive test data when used

## 10. Entry and exit criteria

### Entry

- Base URL reachable
- Environment variables defined for the flows under test

### Exit (per iteration)

- Agreed scenarios implemented and passing against DummyJSON
- Quality standard met for positive and negative cases
- No duplicated assertions that belong in shared scripts
- Naming and folder structure consistent with this strategy

## 11. Risks and mitigations

| Risk | Mitigation |
|------|------------|
| Public API data/behavior changes | Favor stable assertions; avoid over-coupling to volatile fields |
| Auth token expiry during long runs | Obtain a fresh token in the Authentication folder before dependent requests |
| Scope creep | Implement only the assessment sequence below |

## 12. Implementation sequence

1. Authentication
2. Products (Read)
3. Products (Write)
4. Documentation

## 13. Known API Behaviors

DummyJSON is a public demo API. Some responses reflect its current implementation rather than ideal REST conventions. This suite validates that documented/observed behavior.

Example: `GET Current User - Invalid Token` currently returns HTTP `500` with message `invalid token`. Assertions follow that contract instead of enforcing a theoretical `401`.
