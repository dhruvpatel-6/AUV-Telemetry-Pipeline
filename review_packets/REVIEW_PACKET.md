# 🏆 Prototype Architecture Technical Overview & Handover Packet
**Platform Iteration:** `v1.0.0-Prototype` | **Governance Schema:** `v3.0.0-Truth`
**Project:** Quadruped Functional Prototype Integration

---

## 🏛️ System Architecture & Convergence 
This canonical architecture unifies mechanical structural parameters (Arya), physical layout/subsystem design (Manya), and observability/analytics pipelines (Dhruv):

```text
===================================================================================================
                                      OPERATOR INTERFACE (UI)
===================================================================================================
      [Streamlit Control Cockpit (Port 8501)] ◄──► [Local Run Orchestrator (run_pipeline.ps1)]
                         │                                         ▲
                         │ (User Commands)                         │ (Real-Time UI Logs)
                         ▼                                         │
===================================================================================================
                                         EXECUTION RUNTIME
===================================================================================================
      [C++ Real-Time Kernel RT-Preempt] ◄─────────────────────────┼───┐
                         │                                         │   │
                         │ (Deterministic Control Loop, 5ms Loop)   │   │
                         ▼                                         │   │
===================================================================================================
                                              COMPUTE
===================================================================================================
      [Jetson Orin Nano Single Board Computer (SBC)]               │   │
             ├── Runs inverse kinematics (IK) matrices             │   │
             └── Runs `analytics_worker.py` (Ecosystem Consumer) ──┘   │
                         │                                             │
                         ▼ (Ecosystem Gateway Bus - Port 5555/5556)    │ (TCP/IP Telemetry Stream)
===================================================================================================
                                             TELEMETRY
===================================================================================================
      [Dhruv: Telemetry Core Engine (v3.0.0-Truth)] ───────────────────┘
             ├── Verification: `validate_contract` & Schema Guards
             └── Target: Logs to `quadruped_session_recording.jsonl`
                         ▲
                         │ (Real-Time State Frames)
===================================================================================================
                                              SAFETY
===================================================================================================
      [Optocoupled Safety Switch (Upper Spine-Mounted)] ───────────────┐
             ├── Software-Triggered Emergency Stop Path                │
             └── Hard-wired Motor Power Cut (Estop Active > 15ms)      │ (E-Stop Trigger Override)
                         │                                             │
                         ▼                                             ▼
===================================================================================================
                                               POWER
===================================================================================================
      [6S High-Drain LiPo Battery (22.2V Nominal)] ────────────────────┼────────────────┐
             ├── Direct High-Power Copper Bus Line (22.2V RAW) ────────┼────────┐       │
             └── Dual-Channel Buck Regulator ──────────────────────────┘        │       │
                   ├── 5V @ 5A Rail ──► [Compute Layer / SBC Stack]             │       │
                   └── 3.3V Rail   ──► [Sensory Interface Layer]                │       │
                                                                                │       │
===================================================================================================
                                        COMMUNICATIONS BUS                     │       │
===================================================================================================
      [CAN-Bus Protocol Architecture (1 Mbps)] ◄────────────────────────────────┘       │
      [SPI / I2C Bus Interconnects] ◄───────────────────────────────────────────────────┼──┐
                                                                                        │  │
===================================================================================================
                                     MOTOR DRIVERS & ACTUATORS                          │  │
===================================================================================================
      [12x BLDC Driver Boards (Daisy-chained on CAN)] ◄─────────────────────────────────┘  │
             └── Drives [12x BLDC Direct Actuators (3 per leg, CNC Joint Linked)]           │
                         │                                                                 │
                         ▼ (Continuous Dynamic Mechanical Loads)                           │
===================================================================================================
                                              SENSORS                                      │
===================================================================================================
      [12x Joint Strain Gauges] ──► Measured via Local Torque Matrix (Nm)                 │
      [12x Local Actuator Thermistors] ──► Handled over local I2C Bus                      │
      [Central 9-Axis IMU (BNO055)] ──► Orientation vector mapped via SPI ─────────────────┘
      [4x Foot Contact FSRs] ──► Mapped via Digital High/Low State Logic
===================================================================================================