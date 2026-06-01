# Blueprint API

The plugin exposes Blueprint APIs for runtime voxel interaction through the Voxel Component and Voxel Runtime Blueprint Function Library.

Blueprints should interact with voxel data through these APIs. The internal runtime mesh components, sparse template cache, and generated StaticMesh/Nanite chunks are implementation details.

![Voxel Blueprint API](Images/BP/BP_Component.png)

## Voxel Component Functions

Voxel Component exposes:

- RequestFullVoxelVisualRefresh
- RequestFullVoxelVisualRefreshAsync
- RequestVoxelRegionRefresh
- WorldToVoxel
- VoxelToWorld

Use refresh functions only when a custom system changes voxel state outside the normal runtime delete, damage, or save/load APIs.

## Deletion Functions

Deletion functions remove voxels instantly without applying damage.

Supported operations:

- DeleteVoxel_Single
- DeleteVoxel_Box
- DeleteVoxel_Sphere
- DeleteVoxel_BoxOnComponent
- DeleteVoxel_SphereOnComponent
- DestroyVoxelComponentCompletely

World-scoped functions search for intersecting Voxel Components. Component-specific functions apply the operation only to the component passed in.

Deletion functions return deleted voxel data and affected-neighbor data.

![Voxel Blueprint API](Images/BP/BP_Delete.png)

## Damage Functions

Damage functions apply damage to voxel health before removal.

Supported operations:

- ApplyDamageToVoxels_Unique
- ApplyDamageToVoxels_UniqueOnComponent
- ApplyDamageToVoxels_Box
- ApplyDamageToVoxels_BoxOnComponent
- ApplyDamageToVoxels_Sphere
- ApplyDamageToVoxels_SphereOnComponent

Damage functions return destroyed voxel data and damaged-but-still-occupied voxel data.

![Voxel Blueprint API](Images/BP/BP_Damage.png)

## Save and Load Functions

Save/load functions capture and restore sparse runtime state.

Supported operations:

- SaveVoxelComponent
- SaveAllVoxelComponents
- LoadVoxelComponentFromData
- LoadAllVoxelComponentsFromData
- SaveAllVoxelComponentsAsync
- LoadAllVoxelComponentsFromDataAsync

Save data is delta-based. It stores removed cells, damaged cells, remaining health, and whether a component was completely destroyed.

Async save/load functions spread work across frames and expose per-tick count controls:

- ActorsPerTick for async save.
- ComponentsPerTick for async load.

![Voxel Blueprint API](Images/BP/BP_Save.png)

## Returned Delete/Damage Data

FRuntimeVoxelDeleteData contains:

- WorldPositions
- Colors
- Extras
- VoxelScales
- AffectedComponents

Use this data for hit reactions, particle spawning, dropped fragments, scoring, analytics, or gameplay effects.

## Returned Save Data

FRuntimeVoxelSaveData contains:

- ActorName
- ComponentName
- Size
- RemovedCells
- DamagedCells
- RemainingHealth
- bComponentCompletelyDestroyed
- SparseFormatVersion

The current sparse save format version is 2.

## Helper Functions

Utility functions include:

- MergeRuntimeVoxelDeleteDataArray
- GetCenterOfPositions
- GetRandomPercentageOfPositions
- GetVoxelPerformanceSnapshot
- ForLoopWithDelay
- ForEachLoopWithDelay

These helpers simplify post-processing, staggered Blueprint loops, and performance/debug UI.

![Voxel Blueprint API](Images/BP/BP_Extra.png)

## Usage Notes

- Do not modify generated runtime mesh components directly in Blueprint.
- Use component-specific functions when the target Voxel Component is already known.
- Use world-scoped functions when an explosion, projectile, or trace can affect multiple voxel actors.
- Use save/load APIs instead of serializing full Voxel Assets.
- Large runtime edits should be paired with conservative Runtime Sparse Pipeline budgets.
