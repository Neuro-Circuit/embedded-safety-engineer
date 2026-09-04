# Universal Embedded Rules
- Know the hardware contract before changing low-level code.
- Bound time, memory, queue depth, retries, and resource usage where practical.
- Treat interrupts, DMA, peripherals, tasks, and external devices as concurrent actors.
- Make ownership/lifetime explicit.
- Validate before indexing/copying/parsing.
- Define reset/restart behavior.
- Use deterministic state machines for protocol/peripheral logic where appropriate.
- Document hardware and timing assumptions.
