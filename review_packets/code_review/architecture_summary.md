# 🏛️ Architecture Summary

## Software & Hardware Integration Layer
This system converges mechanical parameters, subsystem layouts, and observability pipelines into a single deterministic runtime.

1. **Hardware Abstraction Layer (HAL):** Exposes direct hardware registers to high-level controllers via standard APIs (`read_sensors()`, `dispatch_actuator_commands()`).
2. **Schema Enforcement (`v3.0.0-Truth`):** Every telemetry packet is validated through `validate_contract()` before streaming over local ports `5555/5556`.
3. **Decoupled Architecture:** Core loops execute independently of external networks. Future attachments (TANTRA, Replay, MDU, Testing) connect exclusively through Phase 7 attachment contracts.