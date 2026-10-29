# Agent + MCP in Rust: practical starting point

This guide maps the key crates and files in this repository if you want to understand or build Agent + MCP flows in Rust.

## Libraries and runtime components used here

Core libraries used by this repo's Agent + MCP path:

- **MCP protocol SDK:** `rmcp`
- **Async runtime:** `tokio`, `tokio-util`
- **Serialization:** `serde`, `serde_json`
- **Error handling:** `anyhow`, `thiserror`
- **Observability:** `tracing`
- **HTTP/server stack used around runtime:** `axum`, `reqwest`

Where these are declared:

- Workspace dependency versions and pins:
  - `codex-rs/Cargo.toml`
- MCP runtime crate dependency usage:
  - `codex-rs/codex-mcp/Cargo.toml`
- Extension wiring crates:
  - `codex-rs/ext/mcp/Cargo.toml`
  - `codex-rs/ext/agent/Cargo.toml`

## Recommended read order in this codebase

Read in this order to build a clean mental model:

1. **MCP runtime boundary**
   - `codex-rs/codex-mcp/src/lib.rs`
   - `codex-rs/codex-mcp/src/runtime.rs`
   - `codex-rs/codex-mcp/src/mcp/mod.rs`
2. **How MCP is wired into the app**
   - `codex-rs/ext/mcp/src/lib.rs`
   - `codex-rs/app-server/src/extensions.rs`
3. **Agent runtime entry point**
   - `codex-rs/ext/agent/src/lib.rs`

## Build from scratch in Rust (crate-by-crate progression)

Use this progression when creating a new project:

1. **`proto` crate**
   - Own protocol-facing model types and serde mappings.
2. **`mcp-runtime` crate**
   - Start with a minimal `rmcp + tokio` server/client path.
   - Add lifecycle, startup policy, and cancellation handling.
3. **`agent-runtime` crate**
   - Add agent invocation and run lifecycle (thread/session/turn control).
4. **`config` crate**
   - Add runtime configuration and policy snapshots.
5. **`auth` crate**
   - Add OAuth/token lifecycle, callback handling, refresh strategy.
6. **`tool-catalog` crate**
   - Add tool/resource registration, namespacing, and caching.
7. **`integration` crate (app wiring)**
   - Wire MCP + Agent extensions into your host/server process.

## Minimal implementation phases

Keep implementation phases small and sequential:

1. **Phase 1: transport and handshake only**
   - Boot MCP client/server with `rmcp` and `tokio`.
2. **Phase 2: one tool call path**
   - Implement one end-to-end tool call with typed inputs/outputs.
3. **Phase 3: config + policy snapshotting**
   - Freeze runtime config per run/publication.
4. **Phase 4: auth and token refresh**
   - Add OAuth/login and refresh ownership rules.
5. **Phase 5: catalog, caching, and refresh events**
   - Add dynamic tool/resource refresh and cache generation.
6. **Phase 6: app-server integration**
   - Add request processors and extension registration.

## Fast orientation checklist

Before writing new code, ensure you can answer:

- Which crate owns MCP connection/runtime lifecycle?
- Which crate owns Agent invocation and turn start?
- Where extension registration happens for app startup?
- Which config/auth objects are immutable per runtime publication?
