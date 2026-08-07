# 🔍 Review Focus Directives

When evaluating this prototype codebase, please focus on:
1. **Schema Compliance:** Verify that all active streams strictly adhere to the `v3.0.0-Truth` specification defined in `schema/canonical_telemetry_v3.json`.
2. **Determinism:** Confirm that the control loop maintains execution under the mandatory $5.0\,\text{ms}$ latency deadline.
3. **Ecosystem Decoupling:** Confirm that Phase 7 interfaces define clean JSON Schema attachment boundaries without requiring functional integrations in the core execution path.