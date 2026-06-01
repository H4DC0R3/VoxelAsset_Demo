# Nanite Runtime Pipeline

VE 1.2.0 adds an optional Nanite runtime rendering backend for the sparse voxel runtime pipeline.

## Enabling Nanite Runtime Rendering

Open Project Settings > Voxel Editor > Runtime Rendering and enable:

- Use Nanite Runtime Rendering (Restart Required)

After changing this option:

1. Restart the Unreal Editor.
2. Let the plugin apply the pending runtime rendering rebuild.
3. Save all rebuilt Voxel Assets and generated baked proxies.

The demo project is currently configured with Nanite runtime rendering enabled.

## What Nanite Changes

When Nanite runtime rendering is enabled:

- Shared runtime visual clusters are built as Nanite StaticMeshes.
- Baked editor anchors are rebuilt with Nanite enabled.
- Standard StaticMesh distance hiding is bypassed.
- Runtime Nanite Collision Mode controls collision.
- Default Collision Generation is ignored by runtime Nanite collision.
- Local override chunks are still generated after runtime edits.

Nanite changes the visual backend. It does not replace the sparse runtime data model.

## Sparse Template Flow

The runtime flow is:

1. The Voxel Asset provides sparse occupancy, palette indices, health, material, and baked proxy data.
2. The runtime template cache builds shared visual/collision clusters for pristine instances.
3. Voxel Components attach shared clusters.
4. Delete, damage, or save/load operations mark a component as locally diverged.
5. Only affected edit chunks and visual clusters are rebuilt as local overrides.
6. Unedited clusters continue using shared data.

This keeps repeated instances cheap while allowing localized runtime destruction.

## Collision in Nanite Mode

Nanite rendering uses a dedicated collision setting:

- None
- Fast Chaos TriMesh

Fast Chaos TriMesh builds collision directly from voxel chunk triangles. Use it when runtime traces, blocking, or physics interaction need to match the generated voxel surface.

Default Collision Generation is the runtime collision path for the Standard StaticMesh backend. Nanite runtime collision is controlled by Runtime Nanite Collision Mode.

## Streaming Behavior

Nanite mode bypasses Standard StaticMesh distance visibility streaming.

This means:

- Runtime Critical Radius does not hide Nanite visuals.
- Runtime Visual Hysteresis, Runtime Freeze Seconds, and Recently Rendered streaming options are Standard StaticMesh-only.
- Nanite handles visual streaming.
- The Voxel Streaming Subsystem still records runtime performance state.

## Budgeting Runtime Edits

Nanite mode still uses Runtime Sparse Pipeline budgets for local override work:

- Runtime Sparse Edit Chunk Size
- Runtime Sparse Visual Cluster Size
- Runtime Override Build Budget Ms
- Runtime Override Build Chunks Per Frame
- Runtime Override Max Active Builds Per Frame

Tune these settings for destruction-heavy scenes. Nanite reduces visual rendering cost for dense geometry, but local override generation still consumes CPU time.

## When to Use Nanite

Use Nanite runtime rendering for:

- Dense voxel assets
- Large static or mostly-static environments
- Scenes where visual triangle count is the main cost
- Assets that still need localized runtime destruction

Use Standard StaticMesh rendering instead when:

- Distance-based hide/unhide behavior is required.
- You need Simple Boxes or Approximate Surface collision generation.
- You want freeze behavior based on Recently Rendered checks.
