# Voxel Asset Plugin – Documentation

This repository contains the official documentation for the **Voxel Asset Plugin**, a voxel-based asset creation and runtime destruction system for Unreal Engine.

The plugin provides:
- A dedicated Voxel Asset Editor
- Runtime voxel rendering with procedural meshes or baked static mesh proxies
- Destruction, damage, and animation systems
- Streaming and performance-oriented rebuild scheduling
- Full Blueprint API for gameplay integration

This documentation covers editor tools, runtime usage, project settings, Blueprint functions, and performance guidelines.

Source Compatibility

# Current Engine Version - UE 5.7 
# Current Plugin Version - VE 1.0.0

---

## Documentation Index

- [Getting Started](01-Getting-Started.md)
- [Project Settings](02-Project-Settings.md)
- [Voxel Asset Editor](03-Voxel-Asset-Editor.md)
- [Runtime Usage](04-Runtime-Usage.md)
- [Blueprint API](05-Blueprint-API.md)
- [Streaming Subsystem](06-Streaming-Subsystem.md)
- [Animation System](07-Animation.md)
- [Performance](08-Performance.md)
- [Generated Content Paths](09-Content-Paths.md)

---

## Requirements

- Unreal Engine 5.6 or newer
- Editor build with support for custom asset editors
- Blueprint or C++ project

---

## Notes

This documentation focuses on **how the system works internally**, **how to use it correctly**, and **how to avoid common performance pitfalls**.

All examples assume default settings unless stated otherwise.
