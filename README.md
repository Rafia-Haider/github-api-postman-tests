# GitHub API Test Suite - Postman

An API test suite built in Postman for the GitHub REST API, covering happy path, negative, and edge case scenarios with environment variables, Bearer token authentication, and chained requests.

---

## Project Overview

This collection tests real GitHub API endpoints using a structured, automated test suite. Requests are chained together, each request dynamically passes data to the next using Postman environment variables, simulating a real end-to-end workflow.

**Workflow tested:**
> Authenticate → Create Repo → Create Issue → Verify Issue → Cleanup → Verify Deletion

---

## Collection Structure

```
📁 Happy Path
   ├── Get My Profile
   ├── Create Repo
   ├── Create Issue
   └── Get Issue

📁 Edge Cases
   └── Create Issue Whitespace Title

📁 Negative Tests
   ├── Create Issue No Title
   ├── Create Repo No Name
   ├── Get Nonexistent Repo
   └── Unauthorized Request

📁 Cleanup
   ├── Delete Repo
   └── Get Deleted Repo
```

---

## Key Features

- **Bearer Token Authentication**: real GitHub Personal Access Token auth flow
- **Environment Variables**: `base_url` and `token` stored once, reused across all requests
- **Chained Requests**: `repo_name` and `issue_number` set dynamically via `pm.environment.set()` and passed automatically to subsequent requests
- **Automated Test Scripts**: `pm.test()` assertions on every request covering status codes, response body structure, field values, error messages, and response time
- **Happy Path + Negative + Edge Case coverage**: tests both correct and incorrect API behavior
- **Performance Testing**: response time assertions on every request
- **Collection Runner ready**: entire suite runs in one click, top to bottom

---

## Test Coverage

| Request | Type | Status | Tests |
|---|---|---|---|
| Get My Profile | Happy Path | 200 | 3 |
| Create Repo | Happy Path | 201 | 4 |
| Create Issue | Happy Path | 201 | 5 |
| Get Issue | Happy Path | 200 | 5 |
| Create Issue Whitespace Title | Edge Case | 422 | 6 |
| Create Issue No Title | Negative | 422 | 5 |
| Create Repo No Name | Negative | 422 | 5 |
| Get Nonexistent Repo | Negative | 404 | 4 |
| Unauthorized Request | Negative | 401 | 5 |
| Delete Repo | Cleanup | 204 | 2 |
| Get Deleted Repo | Cleanup | 404 | 3 |

**Total: 47 tests across 11 requests**

---

## What's Being Tested

**Status codes**: every request asserts the correct HTTP status code

**Response body validation**: field existence, correct values, correct types

**Error message validation**: negative tests verify GitHub returns meaningful, specific error messages not just correct status codes

**Chained data integrity**: `Get Issue` verifies the issue created by `Create Issue` is actually fetchable using the dynamically stored `{{issue_number}}`

**Security boundary**: `Unauthorized Request` verifies the API correctly rejects invalid tokens with 401

**Edge case behavior**: whitespace-only issue title reveals GitHub strips spaces and treats it as blank, returning 422

**Performance**: all requests assert response time is under 5000ms

---

## Setup & Usage

### Prerequisites
- [Postman](https://www.postman.com/downloads/) installed
- A GitHub account
- A GitHub Personal Access Token with `repo`, `user`, and `delete_repo` scopes

### How to run

1. **Import the collection**
   - Open Postman → Import → select `GitHub-API-Test-Suite.postman_collection.json`

2. **Import the environment**
   - Open Postman → Environments → Import → select `GitHub-API.postman_environment.json`

3. **Fill in your environment variables**

   | Variable | Value |
   |---|---|
   | `base_url` | `https://api.github.com` |
   | `token` | your GitHub Personal Access Token |
   | `repo_name` | leave empty, set automatically |
   | `issue_number` | leave empty, set automatically |

4. **Select the environment**
   - Top right dropdown → select `GitHub API`

5. **Run the collection**
   - Right click the collection → Run collection → Start Run

---

## Concepts Demonstrated

- REST API testing with Postman
- Bearer token authentication
- Environment variables for reusable, clean requests
- Request chaining with `pm.environment.set()` and `pm.environment.get()`
- Happy path, negative, and edge case test design
- Error response validation
- Performance assertions
- Read-your-writes verification pattern
- Collection Runner for automated test execution

---

## Tools Used

- Postman
- GitHub REST API v3
