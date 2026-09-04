# Rust Rules
- Minimize unsafe and isolate/document every unsafe boundary.
- Treat MMIO and FFI as trust boundaries.
- Review Send/Sync and interrupt-sharing assumptions.
- Prefer checked arithmetic for external values.
- Use cargo fmt, cargo clippy, and configured security/dependency checks.
