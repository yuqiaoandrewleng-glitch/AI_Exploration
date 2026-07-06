# PRD: Central API Reference Page

## 1. Introduction / Overview

Knowledge Studio provides a **central API Reference page** that documents the complete programmatic API surface of the product. The API Reference is a top-level destination in the Knowledge Studio navigation where developers can discover, read about, and try all available endpoints.

The API Reference is the **single source of truth** for the API surface. It is not surfaced contextually on each screen — instead, developers go to one place to see everything, with full request/response schemas, authentication details, error formats, and usage examples.

This PRD covers:
- The structure and layout of the central API Reference page
- How endpoints are organised and discovered
- The level of detail shown for each endpoint
- Authentication and authorisation details
- Error response formats
- Code examples and language SDKs
- Search and filtering within the API Reference

This PRD is consumed by external developers who integrate Knowledge Studio into their applications, internal tools, and CI/CD pipelines.

---

## 2. Goals

- Provide a single, authoritative destination for the complete API surface
- Help developers discover all available endpoints
- Document each endpoint with full request/response schemas
- Show realistic code examples in multiple languages
- Make the API Reference easy to search and navigate
- Keep the documentation in sync with the actual API surface

---

## 3. User Stories

### US-001: Developer opens the API Reference
**Description:** As a developer, I want to open the central API Reference page so that I can browse the full API surface.

**Acceptance Criteria:**
- [ ] A top-level "APIs" tab is available in the Knowledge Studio navigation
- [ ] Clicking it opens the central API Reference page
- [ ] The page shows a list of all endpoints, organised by capability group
- [ ] Each endpoint is visible with: HTTP method, path, and brief description
- [ ] Typecheck passes
- [ ] Verify in browser

### US-002: Developer browses endpoints by capability
**Description:** As a developer, I want endpoints grouped by capability (Datasets, Pipelines, Retrieval, etc.) so that I can find what I need quickly.

**Acceptance Criteria:**
- [ ] Endpoints are organised into groups: Datasets, Pipelines, Retrieval & Chat, Evaluation, Observability, Graph, Internal KB, Admin
- [ ] Each group has a clear heading and description
- [ ] Each group lists its endpoints with HTTP method colour-coded (GET=blue, POST=green, PUT=orange, DELETE=red, PATCH=yellow)
- [ ] User can expand/collapse groups
- [ ] Typecheck passes
- [ ] Verify in browser

### US-003: Developer views endpoint detail
**Description:** As a developer, I want to click an endpoint to see its full request/response schema and code examples.

**Acceptance Criteria:**
- [ ] Clicking an endpoint opens a detail view (in-page or side panel)
- [ ] Detail shows: HTTP method, path, description, authentication requirements, query parameters, request body schema, response schema, error responses
- [ ] Code examples are shown in at least: cURL, Python, JavaScript
- [ ] User can copy any code example to clipboard
- [ ] Typecheck passes
- [ ] Verify in browser

### US-004: Developer searches the API Reference
**Description:** As a developer, I want to search the API Reference by keyword so that I can quickly find specific endpoints.

**Acceptance Criteria:**
- [ ] A search input is available at the top of the API Reference page
- [ ] Search matches: endpoint path, description, parameter names, tag names
- [ ] Search is case-insensitive and supports partial matches
- [ ] Results update as the user types (with debounce)
- [ ] Empty state: "No endpoints match your search."
- [ ] Typecheck passes
- [ ] Verify in browser

### US-005: Developer filters by HTTP method
**Description:** As a developer, I want to filter the API Reference by HTTP method so that I can focus on what I need.

**Acceptance Criteria:**
- [ ] Filter chips: All / GET / POST / PUT / PATCH / DELETE
- [ ] User can select one or more methods
- [ ] Endpoints are filtered to show only those matching the selected methods
- [ ] Filter state persists within a session
- [ ] Typecheck passes
- [ ] Verify in browser

### US-006: Developer tries an endpoint (interactive console)
**Description:** As a developer, I want to try an endpoint directly from the API Reference so that I can validate my understanding before writing code.

**Acceptance Criteria:**
- [ ] Each endpoint detail has a "Try it" button
- [ ] Clicking it opens an interactive console with: pre-filled request body, API key input, "Send" button
- [ ] On send, the response is shown: status code, response body, response headers, latency
- [ ] The console uses the user's workspace API key
- [ ] Typecheck passes
- [ ] Verify in browser

### US-007: Developer views authentication details
**Description:** As a developer, I want to see how to authenticate API requests so that I can call the API from my code.

**Acceptance Criteria:**
- [ ] The API Reference has a dedicated "Authentication" section
- [ ] Section explains: API key auth, how to create a key in the Admin section, how to pass it in the `Authorization: Bearer <key>` header
- [ ] Code examples show the auth header in all three languages (cURL, Python, JavaScript)
- [ ] Typecheck passes
- [ ] Verify in browser

### US-008: Developer views error response format
**Description:** As a developer, I want to see the standard error response format so that I can handle errors in my code.

**Acceptance Criteria:**
- [ ] The API Reference has a dedicated "Errors" section
- [ ] Section lists the standard error codes (400, 401, 403, 404, 409, 429, 500) with example responses
- [ ] Section shows the standard error body shape: `{ error: { code, message, details } }`
- [ ] Each example includes a realistic error scenario
- [ ] Typecheck passes
- [ ] Verify in browser

### US-009: Developer views rate limiting details
**Description:** As a developer, I want to see the rate limiting policy so that I can design my integration to stay within limits.

**Acceptance Criteria:**
- [ ] The API Reference has a dedicated "Rate Limiting" section
- [ ] Section explains: per-key rate limit, response headers (`X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`)
- [ ] Section shows what happens when limit is exceeded (429 response with `Retry-After` header)
- [ ] Typecheck passes
- [ ] Verify in browser

### US-010: Developer copies code examples
**Description:** As a developer, I want to copy code examples to clipboard so that I can paste them into my own code.

**Acceptance Criteria:**
- [ ] Each code example has a "Copy" button
- [ ] Clicking copies the code to clipboard
- [ ] A toast confirms the copy
- [ ] Code examples are syntax-highlighted
- [ ] Typecheck passes
- [ ] Verify in browser

### US-011: Developer switches between code languages
**Description:** As a developer, I want to switch between code languages (cURL, Python, JavaScript) so that I can see examples in my preferred language.

**Acceptance Criteria:**
- [ ] Each endpoint detail has a language selector: cURL / Python / JavaScript
- [ ] Default: cURL
- [ ] User's choice persists within a session
- [ ] All code examples are kept in sync (same endpoint, different syntax)
- [ ] Typecheck passes
- [ ] Verify in browser

---

## 4. Functional Requirements

### Page structure
- FR-1: The API Reference must be accessible as a top-level tab in the Knowledge Studio navigation
- FR-2: Endpoints must be organised into capability groups: Datasets, Pipelines, Retrieval & Chat, Evaluation, Observability, Graph, Internal KB, Admin
- FR-3: Each group must support expand/collapse
- FR-4: HTTP methods must be colour-coded throughout

### Endpoint detail
- FR-5: Each endpoint must show: HTTP method, path, description, authentication requirements, query parameters, request body schema, response schema, error responses
- FR-6: Each endpoint must have code examples in at least: cURL, Python, JavaScript
- FR-7: Each code example must have a Copy button

### Search & filter
- FR-8: A search input must search across: endpoint path, description, parameter names, tags
- FR-9: The search must be case-insensitive with debounced updates
- FR-10: HTTP method filter chips must allow filtering by one or more methods

### Interactive console
- FR-11: Each endpoint must have a "Try it" interactive console
- FR-12: The console must pre-fill the request body
- FR-13: The console must show the response: status code, body, headers, latency
- FR-14: The console must use the user's workspace API key

### Authentication
- FR-15: The API Reference must have a dedicated Authentication section
- FR-16: The section must explain: API key creation, the `Authorization: Bearer <key>` header format
- FR-17: Code examples must show the auth header in all three languages

### Errors
- FR-18: The API Reference must have a dedicated Errors section
- FR-19: The section must list standard HTTP error codes with example responses
- FR-20: The section must show the standard error body shape

### Rate limiting
- FR-21: The API Reference must have a dedicated Rate Limiting section
- FR-22: The section must explain: per-key rate limits, response headers, 429 behaviour, `Retry-After` header

### Code examples
- FR-23: Code examples must be available in cURL, Python, JavaScript
- FR-24: User must be able to switch between languages per endpoint
- FR-25: Language choice must persist within a session
- FR-26: All code examples must be syntax-highlighted
- FR-27: Each code example must have a Copy button

---

## 5. Out of Scope

- Surfacing APIs contextually on each UI screen (the API Reference is a single central page only)
- OpenAPI spec download
- SDK generation or download
- Webhook documentation
- API changelog
- API deprecation notices
- Per-tenant API customisation
- API analytics (call volume, latency, etc.)
- API key management UI (lives in the Admin section, separate from the API Reference)

---

## 6. Design Considerations

- **UI components to reuse**: tabs, code-snippet, badge, btn-group, search-bar, info-callout
- **New components needed**: endpoint list item, endpoint detail panel, language selector, interactive console
- **Reference mockup**: `knowledge_studio_wireframes.html` — the existing API Reference screen can serve as a starting point for the central page layout
- **Visual style**: clean, documentation-focused, syntax-highlighted code, dark code blocks on light background
- **Layout**: 2-column on desktop (endpoint list left, detail right); single column on mobile
- **Navigation**: collapsible groups, sticky search at top, "back to top" link
- **Empty state**: when no endpoints match search, show helpful message with suggestion to clear filters

---

## 7. Relationships to Other Features

- **Stands alone** — this PRD is not consumed by other PRDs; it documents the API surface for external developers
- **References the actual API surface** — all endpoints listed in this page must exist in the codebase
- **Consumed by**: external developers integrating Knowledge Studio into their applications

---

## 8. Technical Considerations

### Page rendering
- Server-side rendered for SEO and fast initial load
- Interactive console is client-side JavaScript
- Code examples stored in structured data (JSON) with multi-language variants

### Search
- Client-side fuzzy search (lunr.js or similar) — fast for the expected endpoint count
- Index built from endpoint metadata on page load

### Interactive console
- Uses fetch API to call the actual endpoints
- API key stored in user's session (not exposed in URL)
- Response displayed with syntax highlighting

### Documentation sync
- API metadata lives in a single source of truth (e.g. an OpenAPI spec, hand-curated JSON, or auto-generated from the codebase)
- The API Reference page reads from this source
- When endpoints change, the reference updates automatically

### Code example languages
- cURL: standard, always included
- Python: uses `requests` library
- JavaScript: uses `fetch` API
- (Future: Go, Ruby, Java — not in v1)

### Performance
- Page load: <1s
- Search: <100ms (client-side, no network)
- Interactive console: latency matches actual endpoint latency

---

## 9. Success Metrics

- 90% of developers find the endpoint they need within 30 seconds
- 70% of new users visit the API Reference within their first week
- "Try it" console usage: 50% of developers use it at least once
- Code example copy rate: 30% of developers copy at least one example
- API call volume increases by 40% within 3 months of launching the central API Reference

---

## 10. Open Questions

- [ ] Should the API Reference be public (accessible without login) or require authentication?
- [ ] Should we add a "Download OpenAPI spec" button (auto-generated)?
- [ ] Should we add a "Changelog" section showing recent API changes?
- [ ] Should we add a "Report an issue" link to feedback form?
- [ ] Should we add code examples for additional languages (Go, Ruby, Java) in v2?
- [ ] Should we support API key creation directly from the API Reference page (currently in Admin)?
- [ ] Should we add a "dark mode" toggle for the API Reference?
- [ ] Should the interactive console support saving request templates?
