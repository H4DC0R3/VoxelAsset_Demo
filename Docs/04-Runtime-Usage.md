# Runtime Usage

Voxel assets are rendered and managed at runtime through the Voxel Component.

## Voxel Actor

Placing a Voxel Asset in the level creates a Voxel Actor that contains a Voxel Component.

The component is responsible for:

- Applying the shared sparse template.
- Owning local sparse overrides after runtime edits.
- Creating shared and override visual components.
- Updating collision.
- Handling runtime destruction, damage, and save/load.

## Runtime Pipeline

VE 1.2.0 uses a sparse runtime pipeline.

The source Voxel Asset provides:

- Sparse occupancy data
- Palette indices
- Voxel health
- Palette textures
- Baked StaticMesh proxy data

At runtime:

- A shared template is created per asset, voxel size, edit chunk size, and visual cluster size.
- Pristine instances attach shared visual/collision clusters.
- Runtime edits only create local override chunks for the affected component instance.
- Local override work is spread across frames using the Runtime Sparse Pipeline budget settings.
- The source asset is not modified by runtime gameplay operations.

## Rendering Backends

The runtime backend is selected by Use Nanite Runtime Rendering in Project Settings.

### Nanite Runtime Rendering

Nanite runtime rendering builds shared visual clusters and baked anchors as Nanite StaticMeshes.

Use this backend for large visual scenes, high triangle counts, and mostly static voxel environments with occasional localized destruction.

Nanite runtime rendering:

- Uses Nanite for visual streaming.
- Bypasses Standard StaticMesh distance visibility streaming.
- Uses Runtime Nanite Collision Mode for collision.
- Supports local override chunks after destruction or damage.

### Standard StaticMesh Rendering

Standard StaticMesh rendering uses regular StaticMesh components for shared visuals and local overrides.

Use this backend when the project needs explicit distance-based visibility streaming, freezing, or collision modes controlled by Default Collision Generation.

Standard StaticMesh rendering:

- Uses Runtime Critical Radius for distance visibility.
- Can freeze edited local override components after inactivity.
- Uses Default Collision Generation.
- Supports WasRecentlyRendered-based streaming decisions when enabled.

## Runtime Edits

Runtime edits operate on occupied sparse cells.

Supported edit types:

- Delete single voxel
- Delete sphere
- Delete box
- Destroy a whole Voxel Component
- Damage single voxel
- Damage sphere
- Damage box

Delete operations remove voxels immediately. Damage operations reduce per-voxel health and destroy cells when health reaches zero.

Each edit returns output data that can be used for effects, gameplay feedback, scoring, or save systems.

## Rebuild Behavior

Runtime edits do not rebuild the entire asset.

When an edit touches a region:

1. The component marks itself as diverged from the source asset.
2. Removed cells or health overrides are stored locally.
3. Affected edit chunks are queued.
4. Visual clusters touched by those edit chunks are rebuilt as local overrides.
5. Shared pristine clusters stay attached wherever possible.

The Runtime Override Build Budget, Chunks Per Frame, and Max Active Builds Per Frame settings control how much override work can be done each frame.

## Coordinate Conversion

Voxel Component exposes Blueprint-callable helpers:

- WorldToVoxel
- VoxelToWorld

Use these helpers when converting gameplay hit locations, traces, or custom logic into voxel coordinates.

## Collision

Collision is optional per Voxel Component through Generate Collision.

Collision behavior depends on the runtime backend:

- Nanite backend: Runtime Nanite Collision Mode controls collision. Fast Chaos TriMesh builds collision directly from voxel chunk triangles.
- Standard StaticMesh backend: Default Collision Generation controls Simple Boxes, Complex as Simple, or Approximate Surface collision.

Collision updates are generated only for the relevant shared or override geometry.

## Save/Load Runtime State

Runtime save data stores sparse deltas, not full voxel grids.

Saved data includes:

- Actor name
- Component name
- Grid size for compatibility checks
- Removed cells
- Damaged cells
- Remaining health values
- Complete-component destruction flag
- Sparse format version

Save/load transactions temporarily prioritize persistence work over gameplay edits and pending visual/collision override work.

## Runtime Safety

The runtime system ensures:

- Source Voxel Assets are not mutated by gameplay edits.
- Runtime save data stores only per-instance sparse overrides.
- Pristine instances can continue sharing the same template.
