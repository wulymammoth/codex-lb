## Why

Local OpenCode sessions against `codex-lb` could appear to hang with no client-visible error even when operators forced upstream streaming transport to `http`. Request logs showed `stream_idle_timeout`, `stream_incomplete`, and `Upstream websocket closed before response.completed (close_code=1000)` for the same requests that were logged with `transport="http"`.

The root cause was that the HTTP Responses bridge path still created and maintained an internal upstream websocket session for continuity. That made the dashboard `upstream_stream_transport=http` setting incomplete: the direct stream path respected it, but the HTTP bridge entrypoint did not.

## What Changes

- Make the HTTP Responses bridge entrypoint bypass the websocket-backed bridge whenever dashboard settings explicitly force upstream streaming transport to `http`.
- Add a regression test covering the forced-HTTP bridge bypass.
- Document the production symptom, the request-log evidence, the temporary local workaround, and the final code fix in Responses API context docs.

## Impact

- Code: `app/modules/proxy/service.py`
- Tests: `tests/unit/test_proxy_http_bridge.py`
- Specs/context: `openspec/specs/responses-api-compat/spec.md`, `openspec/specs/responses-api-compat/context.md`
