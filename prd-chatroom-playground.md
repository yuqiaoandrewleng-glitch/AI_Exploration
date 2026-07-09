# PRD: Chatroom & Playground Improvements

## 1. Introduction / Overview

The Playground (Chatroom) is where users test their pipelines by asking questions and reviewing answers. This PRD covers two improvements to the Playground experience:

1. **Additional metrics** — surface usage and runtime evaluation signals per message so users can see how their pipelines perform in real time
2. **Citation upgrade** — replace the current "Sources: ..." bottom-line format with **inline footnote citations** that look like `[1]`, `[2]`, etc., are clickable, and jump to the source document

The Playground already shows a basic "Session Info" panel with total latency, tokens, and cost. This PRD formalises the per-message metrics surface and adds the inline footnote citation format.

This PRD is consumed by users running queries in the Playground and by external developers integrating with the retrieval/chat APIs.

---

## 2. Goals

- Make usage metrics (latency, token usage, cost) visible per message
- Make runtime evaluation signals visible per message (placeholder fields for v1)
- Upgrade citations to inline footnote format — clickable, hoverable, with source preview
- Make metrics and citations available in the API responses (not just the UI)
- Keep the Playground fast and responsive even with the new surface

---

## 3. User Stories

### US-001: User sees per-message latency, token usage, and cost
**Description:** As an end user, I want to see the latency, token usage, and cost for each assistant message so that I can understand the performance of my pipeline.

**Acceptance Criteria:**
- [ ] Each assistant message shows a metrics line with: latency (ms), token usage (input/output/total), cost
- [ ] Metrics are displayed inline below the message (collapsible if space is tight)
- [ ] Metrics update after the message completes
- [ ] Numbers are copy-to-clipboard on click
- [ ] Typecheck passes
- [ ] Verify in browser

### US-002: User sees cumulative session metrics
**Description:** As an end user, I want to see the cumulative metrics for the whole chat session so that I can track total usage.

**Acceptance Criteria:**
- [ ] The Playground sidebar / info panel shows: total messages, total tokens, total cost, average latency
- [ ] Updates after each new message
- [ ] User can reset the session (starts fresh metrics)
- [ ] Typecheck passes
- [ ] Verify in browser

### US-003: User can export chat history with metrics
**Description:** As an end user, I want to export the chat history (with metrics) as a CSV so that I can analyse it externally.

**Acceptance Criteria:**
- [ ] "Export" button in the Playground exports a CSV with: message_id, role, content (truncated), latency_ms, tokens, cost, citations, timestamp
- [ ] CSV is downloaded immediately
- [ ] User can copy the CSV to clipboard as a fallback
- [ ] Typecheck passes
- [ ] Verify in browser

### US-004: User sees runtime evaluation signals per message
**Description:** As an end user, I want to see runtime evaluation signals for each assistant message so that I can spot low-quality responses.

**Acceptance Criteria:**
- [ ] Each assistant message shows a small badge with the runtime evaluation score (placeholder field, see Open Questions)
- [ ] The score is a 0-1 number with a visual indicator (e.g. coloured dot: green ≥ 0.7, yellow 0.4-0.7, red < 0.4)
- [ ] User can hover the badge to see the full evaluation breakdown
- [ ] User can click the badge to flag the message for review
- [ ] Typecheck passes
- [ ] Verify in browser

### US-005: System records runtime evaluation for every assistant response
**Description:** As a developer, I want runtime evaluation signals to be computed and stored for every assistant response so that we can analyse quality later.

**Acceptance Criteria:**
- [ ] Every assistant response is evaluated at runtime (placeholder algorithm, see Open Questions)
- [ ] The evaluation result is stored with the message
- [ ] The evaluation result is included in the API response (see FRs)
- [ ] Low-scoring messages are surfaced in the Observability screen
- [ ] Typecheck passes

### US-006: User sees inline footnote citations in the assistant's response
**Description:** As an end user, I want citations in the assistant's response to appear as numbered footnotes inline so that I can quickly identify which sources support which claims.

**Acceptance Criteria:**
- [ ] The assistant's response includes inline citation markers in the format `[1]`, `[2]`, etc.
- [ ] Markers are placed at the end of the sentence or claim they support
- [ ] At the end of the response, a "References" section lists each citation with: `[1] Source title — URL` (or document link)
- [ ] Citations are rendered in both markdown and plain text responses
- [ ] Typecheck passes
- [ ] Verify in browser

### US-007: User can hover a footnote to see a source preview
**Description:** As an end user, I want to hover a footnote to see a preview of the source so that I can quickly assess relevance without leaving the chat.

**Acceptance Criteria:**
- [ ] Hovering a footnote shows a tooltip with: source title, source type, and a 200-char snippet of the relevant chunk
- [ ] Tooltip appears within 200ms of hover
- [ ] Tooltip is keyboard-accessible (focus shows the same content)
- [ ] Typecheck passes
- [ ] Verify in browser

### US-008: User can click a footnote to open the source
**Description:** As an end user, I want to click a footnote to jump to the source document so that I can read the full context.

**Acceptance Criteria:**
- [ ] Clicking a footnote opens the source in a side panel (or new tab, depending on source type)
- [ ] The side panel shows the full source content
- [ ] The relevant chunk is highlighted in the source
- [ ] Typecheck passes
- [ ] Verify in browser

### US-009: User can compare two messages on metrics
**Description:** As an end user, I want to compare two messages side by side on metrics so that I can see how a configuration change affects performance.

**Acceptance Criteria:**
- [ ] In the Playground, the user can select two messages and click "Compare"
- [ ] A side-by-side view shows: latency, tokens, cost, evaluation score for each
- [ ] Differences are highlighted (better / worse)
- [ ] Typecheck passes
- [ ] Verify in browser

### US-010: Developer receives metrics and citations in API responses
**Description:** As a developer, I want the retrieval and chat APIs to return per-message metrics and inline citations so that I can integrate them into my application.

**Acceptance Criteria:**
- [ ] `POST /api/v1/retrieve` and `POST /api/v1/chat` return: `latency_ms`, `tokens`, `cost`, `runtime_eval`, and `citations` (structured, not just text)
- [ ] The `citations` field is a structured array: `[{ index: 1, source_id, source_title, source_url, snippet, chunk_id }]`
- [ ] The response `answer` field includes the inline footnote markers
- [ ] The response `references` field (or similar) lists each citation with the same structure
- [ ] Typecheck passes

---

## 4. Functional Requirements

### Per-message metrics
- FR-1: Each assistant message must display: latency (ms), token usage (input/output/total), cost
- FR-2: Each assistant message must display a runtime evaluation badge (placeholder field, see Open Questions)
- FR-3: Metrics must update after the message completes
- FR-4: Metrics must be copy-to-clipboard on click
- FR-5: Metrics must be exportable as CSV (chat history with metrics per message)

### Session metrics
- FR-6: The Playground info panel must show: total messages, total tokens, total cost, average latency
- FR-7: Session metrics must update after each new message
- FR-8: User must be able to reset the session (starts fresh metrics)

### Runtime evaluation (v1 placeholder)
- FR-9: The system must compute and store a runtime evaluation score for every assistant response
- FR-10: The runtime evaluation must be included in the API response
- FR-11: Low-scoring messages must be surfaced in the Observability screen

### Inline footnote citations
- FR-12: The assistant's response must include inline citation markers in the format `[1]`, `[2]`, etc.
- FR-13: Citation markers must be placed at the end of the sentence or claim they support
- FR-14: A "References" section must list each citation at the end of the response with: `[index] Source title — URL`
- FR-15: Citations must render correctly in both markdown and plain text responses
- FR-16: Footnotes must be hoverable (show source preview tooltip)
- FR-17: Footnotes must be clickable (open source in side panel)
- FR-18: The relevant chunk in the source must be highlighted when opened

### API exposure
- FR-19: `POST /api/v1/retrieve` and `POST /api/v1/chat` must return: `latency_ms`, `tokens`, `cost`, `runtime_eval`, and `citations` (structured array)
- FR-20: The `citations` field must be structured: `[{ index, source_id, source_title, source_url, snippet, chunk_id }]`
- FR-21: The response `answer` field must include the inline footnote markers
- FR-22: The response `references` field (or similar) must list each citation with the same structure

### Comparison
- FR-23: User must be able to select two messages and click "Compare" to see side-by-side metrics
- FR-24: The compare view must show: latency, tokens, cost, evaluation score for each message
- FR-25: Differences must be highlighted (better / worse indicator)

---

## 5. Out of Scope

- Online / runtime evaluation algorithm details (placeholder for v1; algorithm defined by separate workstream)
- Token-level cost breakdown by LLM provider (just total cost in v1)
- Real-time cost budget enforcement (handled by separate workstream)
- Custom evaluation models (only the default placeholder in v1)
- Citation style customisation (only the `[1]`, `[2]` format in v1)
- Inline citations in user messages (only assistant messages)
- Citation export to BibTeX / other academic formats
- Citation graph (which sources cite which other sources)
- Per-message cost projection before sending (just actual cost after)

---

## 6. Design Considerations

- **UI components to reuse**: badges, info-callouts, btn-group, table-row (for the compare view)
- **New components needed**: footnote marker, footnote hover tooltip, source side panel, metrics line, runtime evaluation badge
- **Reference mockup**: `knowledge_studio_wireframes.html` — Playground screen, current "Sources: ..." line will be replaced
- **Visual style**: footnote markers are small, superscript-style numbers in a subtle blue/grey colour; references section uses the same numbered list format
- **Layout**: metrics line appears directly below the assistant's message (compact, single line); expandable for full breakdown
- **Empty state**: when runtime eval is unavailable, hide the badge silently (don't show "N/A")
- **Performance**: hover tooltip should appear within 200ms; side panel should open within 500ms

---

## 7. Relationships to Other Features

- **Consumes**:
  - `prd-agentic-pipeline.md` — agentic pipelines generate multiple intermediate steps; metrics must be aggregated across steps
  - `prd-dataset-auto-refresh.md` — refresh health affects pipeline quality over time
  - `prd-graph-rag.md` and `prd-llmwiki-vector-free.md` — the citation sources come from the underlying retrieval strategies
- **Surfaced in**: `prd-dataset-general-info.md` — the Overview tab shows pipeline-level metrics that include chat session data
- **API support**: `prd-apis.md` (central API Reference) — the new metrics and citation fields must be documented in the central API Reference
- **Monitored via**: `prd-observability.md` (out of scope for this batch) — runtime eval signals feed into observability dashboards

---

## 8. Technical Considerations

### Data model
- **Message**: `{ id, session_id, role, content, latency_ms, tokens: { input, output, total }, cost, runtime_eval: { score, breakdown }, citations: [{ index, source_id, source_title, source_url, snippet, chunk_id }], created_at }`
- **Session**: `{ id, pipeline_id, user_id, started_at, total_messages, total_tokens, total_cost, avg_latency_ms }`

### Inline citation format
- Generation prompt instructs the LLM to include `[1]`, `[2]` markers at the end of cited claims
- Post-processing: parse the response to extract citation positions and build the structured `citations` array
- If the LLM fails to include markers, the system falls back to the previous "Sources: ..." line at the bottom (degraded mode, not silent)

### Runtime evaluation
- v1: placeholder algorithm (a simple signal, e.g. average retrieval score, or a basic LLM-as-judge call) — to be replaced by the real algorithm defined in a separate workstream
- The score is computed after the response is generated
- Stored with the message for later analysis

### Side panel for source viewing
- Lazy-load the full source content on first open (don't load all sources upfront)
- Highlight the relevant chunk by scrolling to it and applying a temporary highlight

### Performance
- Metrics display: <50ms render after message completes
- Hover tooltip: <200ms
- Side panel open: <500ms
- CSV export: <1s for sessions with <100 messages

### API contracts
- `POST /api/v1/retrieve` response extended to include: `latency_ms`, `tokens`, `cost`, `runtime_eval`, `citations`, `references`
- `POST /api/v1/chat` response extended with the same fields
- Backwards compatibility: existing fields are unchanged; new fields are additive

---

## 9. Success Metrics

- 80% of Playground users interact with at least one footnote citation per session
- Average hover-to-source-preview latency < 200ms
- 70% of users find runtime evaluation signals useful (proxy: badge click rate)
- CSV export usage: 20% of users export at least once
- API consumers adopting the new fields: 50% within 3 months of launch
- No regression in Playground response time (P95 < 1.5s for static, < 4s for agentic)

---

## 10. Open Questions

- [ ] (v1 placeholder) Runtime evaluation algorithm — what signals make up the score? (e.g. retrieval confidence, response groundedness, answer relevance)
- [ ] (v1 placeholder) Runtime evaluation score thresholds for green/yellow/red
- [ ] (v1 placeholder) Should the runtime evaluation also produce per-citation scores (which citation was most useful)?
- [ ] (v1 placeholder) Should the user be able to manually rate a response (thumbs up/down) — separate from runtime eval?
- [ ] Citation footnote style: is `[1]`, `[2]` the right format, or should we use `(Source: ...)` style or superscript?
- [ ] Should the runtime evaluation be visible to all users, or only to workspace admins by default?
- [ ] Should we allow users to disable runtime evaluation per pipeline (some users may not want the extra latency)?
- [ ] Should the side panel support multiple tabs (one per source) when a message cites many sources?
- [ ] Should we add a "copy citation" feature (copy the citation as plain text or BibTeX)?
- [ ] Should the metrics line be hidden by default to keep messages clean, with a toggle to show?
