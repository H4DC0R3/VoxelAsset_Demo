# Streaming Subsystem

The streaming subsystem manages voxel rebuild scheduling globally.

---

## Purpose

- Prevent CPU spikes
- Prioritize nearby voxel components
- Queue rebuild operations efficiently

---

## Critical Radius

Components inside the critical radius:
- Are rebuilt first
- Trigger critical-ready events

---

## Delegates

- On initial critical ready
- On global queue drained
- On component geometry update finished
- On component visual update finished

---

## Rebuild Reasons

- Initial load
- Save/load
- Destruction
- Damage
- Visual-only update
