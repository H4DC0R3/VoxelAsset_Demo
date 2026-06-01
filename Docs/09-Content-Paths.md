# Generated Content Paths

The plugin generates and stores supporting assets automatically.

## Material Instances

Location:

- /Game/VoxelEditor/Materials/

Each Voxel Asset creates or updates a dedicated material instance. The material instance receives palette color and extras textures, the active palette index, and optional debug parameters.

## Baked Mesh Proxies

Location:

- /Game/VoxelEditor/BakedMesh/

Voxel Assets generate baked StaticMesh proxies named with the SM_ prefix.

The baked proxy is used for:

- Editor preview anchors
- Thumbnail generation
- Runtime shared visual/collision source data
- Nanite baked anchors when Nanite runtime rendering is enabled

The generated proxy follows the current runtime rendering mode:

- Nanite enabled: the StaticMesh proxy has Nanite enabled.
- Nanite disabled: the StaticMesh proxy is built as a standard StaticMesh with the configured collision generation mode.

Runtime Nanite collision is controlled by Runtime Nanite Collision Mode. Default Collision Generation is the runtime collision path for the Standard StaticMesh backend.

## Palette Textures

Plugin resources include default palette textures and material resources under the plugin content directory.

Voxel Assets can also store generated palette textures for their own palette state.

## Custom Palette Textures

Location:

- /Game/VoxelEditor/Textures/

Custom palettes are saved as project content so assets can reuse them and source control can track them.

## Runtime Generated Objects

Runtime shared templates, local override chunks, and transient Nanite meshes are created at runtime and are not persisted as project assets.

Runtime save/load data should be stored through gameplay save systems using FRuntimeVoxelSaveData, not by saving generated runtime objects.

## Version Control Notes

- Commit generated material instances, palette textures, and baked mesh proxies when they are required by shipped content.
- Commit Voxel Assets after changing voxel geometry or palette data.
- Save all affected assets after changing runtime rendering or collision settings.
- Do not commit runtime-only transient objects.
