# Architecture
Prefer clear boundaries such as application -> service/control -> driver -> hardware
abstraction -> MMIO. Avoid leaking register details through unrelated application modules.
