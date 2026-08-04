# DummyJSON API Automation Assessment

## Overview

This repository contains a Postman-based API automation suite developed as part of a technical QA assessment.

Rather than attempting to cover the entire DummyJSON API surface, the implementation focuses on representative scenarios that demonstrate practical API testing skills, maintainability, reusable validations, request chaining, and risk-based test design.

The solution was intentionally built incrementally, treating each feature as a small pull request to mimic a real collaborative development workflow.

---

## Project Structure

```
.
├── docs/
│   └── TEST_STRATEGY.md
│
├── postman/
│   ├── collections/
│   │   └── DummyJSON_API_Tests.postman_collection.json
│   └── environments/
│       └── DummyJSON_Local.postman_environment.json
│
├── prompts.txt
├── README.md
└── .cursor/
```

---

## Implemented Coverage

### Authentication

- Login with valid credentials
- Login with invalid password
- Login without username
- Login without password
- Get current authenticated user
- Unauthorized request validation
- Invalid token validation

### Products

Read scenarios

- Get products list
- Get product by ID
- Search products
- Non-existent product

Write scenarios

- Create product (simulated)
- Update product (simulated)
- Delete product (simulated)

---

## Testing Strategy

The project follows a small set of engineering principles intended to keep the suite maintainable and reviewer-friendly.

### Environment-driven configuration

- Base URL
- Credentials
- Runtime tokens
- Dynamic resource IDs

are managed through Postman environments instead of hardcoded values.

### Shared validations

Collection-level scripts centralize validations that apply to every request, including:

- Response time
- JSON Content-Type

Request-level scripts contain only behavior-specific assertions.

### Lightweight contract validation

Schemas validate only critical response fields while allowing additional properties.

This minimizes brittleness if the API evolves.

### Request chaining

Runtime values are reused across requests.

Examples include:

Authentication

```
Login
    ↓
accessToken
    ↓
Current User
```

Products

```
List Products
      ↓
 productId
      ↓
Get / Update / Delete
```

### Stable assertions

Assertions intentionally avoid:

- fixed collection sizes
- exact catalog contents
- unstable search relevance
- assumptions about simulated persistence

The suite validates stable API behavior rather than implementation details.

---

## Known API Behaviors

DummyJSON is a public demonstration API.

Some endpoints intentionally behave differently from production-grade REST APIs.

Examples:

- Invalid authentication tokens currently return **HTTP 500** with the message `invalid token`.
- Product **POST**, **PUT** and **DELETE** operations are simulated and do not persist server-side changes.

The test suite validates the documented and observed API behavior rather than theoretical REST expectations.

---

## Design Principles

The implementation prioritizes:

- Maintainability over quantity
- Readability over cleverness
- Reusability over duplication
- Stable assertions over brittle validations
- Incremental development
- Risk-based testing
- YAGNI (avoid unnecessary complexity)

---

## Running the Collection

1. Import the collection located in:

```
postman/collections/
```

2. Import the environment located in:

```
postman/environments/
```

3. Select **DummyJSON Local**.

4. Execute the collection using the Collection Runner.

Authentication scenarios should execute before Products to populate runtime variables.

---

## Documentation

Additional project documentation is available in:

- `docs/TEST_STRATEGY.md` — testing approach and implementation strategy.
- `prompts.txt` — AI-assisted development log used throughout the assessment.

---

## Prompts.txt

AI was used as an engineering assistant to support planning, documentation drafting, implementation reviews, and repetitive tasks.

All architectural decisions, implementation choices, code reviews, and final changes were manually evaluated and approved before being committed.

The objective was to improve development efficiency while maintaining full ownership of the solution.

---

## Future Improvements

If the project continued beyond the scope of this assessment, the next logical steps would include:

- Newman integration
- CI/CD pipeline
- JSON Schema extraction into reusable assets
- Data-driven testing
- Additional API resources
- HTML reporting
- Performance smoke checks

---

Thank you for taking the time to review this assessment.