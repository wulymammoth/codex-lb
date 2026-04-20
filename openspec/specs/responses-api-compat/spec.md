# responses-api-compat Specification

## Purpose

See context docs for background.

## Requirements
### Requirement: Use prompt_cache_key as OpenAI cache affinity
For OpenAI-style `/v1/responses`, `/v1/responses/compact`, and chat-completions requests mapped onto Responses, the service MUST treat a non-empty `prompt_cache_key` as a bounded upstream account affinity key for prompt-cache correctness. This affinity MUST apply even when dashboard `sticky_threads_enabled` is disabled, the service MUST continue forwarding the same `prompt_cache_key` upstream unchanged, and the stored affinity MUST expire after the configured freshness window so older keys can rebalance. The freshness window MUST come from dashboard settings so operators can adjust it without restart.

#### Scenario: dashboard prompt-cache affinity TTL is applied
- **WHEN** an operator updates the dashboard prompt-cache affinity TTL
- **THEN** subsequent OpenAI-style prompt-cache affinity decisions use the new freshness window

### Requirement: Forced HTTP upstream transport bypasses the websocket-backed HTTP bridge
For HTTP `/v1/responses` and HTTP `/backend-api/codex/responses` requests, the service MUST treat dashboard `upstream_stream_transport="http"` as a hard requirement. When that setting is in effect, the service MUST use the non-bridge HTTP retry stream path and MUST NOT create or reuse the websocket-backed HTTP bridge session path for that request.

#### Scenario: dashboard forces HTTP upstream transport for HTTP Responses routes
- **WHEN** a streaming HTTP Responses request enters the HTTP bridge-or-retry selector
- **AND** dashboard `upstream_stream_transport` is set to `"http"`
- **THEN** the request MUST execute through the non-bridge `_stream_with_retry` path
- **AND** the request MUST NOT execute through the websocket-backed HTTP bridge path
