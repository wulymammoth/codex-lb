## 1. HTTP transport enforcement

- [x] 1.1 Bypass the websocket-backed HTTP bridge when dashboard settings explicitly force upstream streaming transport to `http`
- [x] 1.2 Keep existing bridge behavior unchanged for `default`, `auto`, and `websocket`

## 2. Regression coverage

- [x] 2.1 Add a unit regression proving forced `http` transport routes through the non-bridge retry stream path
- [x] 2.2 Run focused bridge transport tests

## 3. Incident documentation

- [x] 3.1 Document the local OpenCode hang symptom, request-log evidence, root cause, workaround, and final fix in Responses API context docs
