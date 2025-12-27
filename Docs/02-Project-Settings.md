# Project Settings

The plugin exposes multiple project-level settings that control defaults, runtime behavior, and performance.

All settings are located under **Project Settings → Voxel Editor**.

---

## Voxel Asset Defaults

These settings define defaults when creating new Voxel Assets.

- **Default Grid Size X/Y/Z**  
  Initial voxel grid dimensions

- **Default Voxel Size**  
  World-space size of a single voxel

---

## Runtime Streaming

Controls the global streaming and rebuild scheduler.

- **Enable Runtime Streaming Subsystem**  
  Enables the global voxel rebuild scheduler

- **Runtime Critical Radius**  
  Radius around the player where voxel components are considered critical

- **Runtime Max Concurrent Component Builds**  
  Limits how many voxel components can rebuild simultaneously

- **Runtime Chunk Size**  
  Size of voxel chunks used during procedural mesh generation

- **Prefer Baked Mesh Proxy in Runtime**  
  Uses baked static meshes when available instead of procedural meshes

---

## JSON Import / Export

- **Last Voxel JSON Directory**  
  Remembers the last directory used for JSON import/export

---

## Plugin Performance Settings

Located under **Voxel Plugin Settings**.

- **Chunks Per Frame**  
  Limits how many chunks are committed per frame

- **Async Chunk Batch Size**  
  Controls how many chunks are processed per async task

---

## Guidelines

- Larger chunk sizes reduce draw calls but increase rebuild cost
- Limiting concurrent builds prevents CPU spikes
- Baked mesh proxies are recommended for large static assets
