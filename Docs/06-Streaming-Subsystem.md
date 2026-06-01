# Streaming Subsystem

The Voxel Streaming Subsystem tracks registered Voxel Components and updates runtime streaming/performance state in game worlds and PIE.

## Runtime Scope

In VE 1.2.0 the subsystem is used differently depending on the runtime rendering backend.

With Nanite runtime rendering enabled:

- Standard distance visibility streaming is bypassed.
- Components previously hidden or frozen by Standard StaticMesh streaming are restored to a visible runtime state.
- Nanite manages visual streaming.
- Sparse edit and override work is processed by each Voxel Component.

With Nanite runtime rendering disabled:

- The subsystem evaluates Standard StaticMesh distance visibility.
- Runtime Critical Radius controls the active radius around player pawns.
- Runtime Visual Hysteresis prevents rapid load/unload toggling.
- Recently rendered checks can freeze or downgrade off-screen components.

## Stream States

Voxel Components can report the following runtime stream states:

- Hidden By Standard Streaming
- Shared Sparse Visible
- Local Override Visible
- Local Override Frozen

Pristine components can use Shared Sparse Visible. Components with local sparse edits use Local Override Visible or Local Override Frozen.

## Evaluation Rules

The subsystem runs at Runtime Stream Update Interval.

On each evaluation pass:

1. Client worlds skip authoritative state decisions.
2. Nanite mode restores visible states and exits without distance culling.
3. Standard StaticMesh mode gathers player pawn locations.
4. Each registered component is compared against the closest player.
5. Components outside Runtime Critical Radius can become hidden.
6. Components inside the radius attach shared visuals or local override visuals.
7. Edited components can freeze after Runtime Freeze Seconds when no recent edits are detected.

The subsystem also records performance counters for visible shared components, local overrides, frozen components, hidden components, active local builds, and streaming evaluation time.

## Component Registration

Voxel Components register with the subsystem when active in a game world or PIE session and unregister when removed. Registration is internal to the component lifecycle.

Runtime sparse override work is not queued by the subsystem. Each component owns its own pending override queue and uses the Runtime Sparse Pipeline budget settings.

## Networking

Streaming decisions are server authoritative.

Client worlds do not evaluate desired stream states directly. Components replicate their runtime stream state from the server.

## Usage Notes

- Standard StaticMesh streaming settings do not cull Nanite visuals.
- Runtime Critical Radius is only a visibility radius for the Standard StaticMesh backend.
- Runtime Stream Update Interval still affects subsystem performance sampling cadence.
- Use GetVoxelPerformanceSnapshot to inspect stream state counts from Blueprint.
