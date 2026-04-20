## ADDED Requirements
### Requirement: Forced HTTP upstream transport bypasses the websocket-backed HTTP bridge
For HTTP `/v1/responses` and HTTP `/backend-api/codex/responses` requests, the service MUST treat dashboard `upstream_stream_transport="http"` as a hard requirement. When that setting is in effect, the service MUST use the non-bridge HTTP retry stream path and MUST NOT create or reuse the websocket-backed HTTP bridge session path for that request.

#### Scenario: Dashboard forces HTTP upstream transport for HTTP Responses routes
- **WHEN** a streaming HTTP Responses request enters the HTTP bridge-or-retry selector
- **AND** dashboard `upstream_stream_transport` is set to `"http"`
- **THEN** the request MUST execute through the non-bridge `_stream_with_retry` path
- **AND** the request MUST NOT execute through the websocket-backed HTTP bridge path
