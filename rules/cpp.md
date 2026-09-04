# C++ Rules
- Make ownership explicit.
- Prefer RAII where compatible with deterministic constraints.
- Avoid hidden allocation in constrained/critical paths.
- Review exceptions, RTTI, virtual dispatch, and static initialization against target constraints.
- Minimize global mutable state.
- Keep ISR-callable APIs bounded and non-blocking.
- Consider MISRA C++ and CERT C++ where appropriate.
