# Generated Content Paths

The plugin generates and stores assets automatically.

---

## Material Instances

Location:
- /Game/VoxelEditor/Materials/

Each Voxel Asset creates a dedicated material instance.

---

## Baked Mesh Proxies

Location:
- /Game/VoxelEditor/BakedMesh/

Used for runtime static mesh rendering.

---

## Palette Textures

Palette textures are loaded from plugin resources:
- Palette color textures
- Palette extras textures

## Custom Palette Textures

Location:
- /Game/VoxelEditor/Textures/

User-created palettes are saved in the project.

---

## Version Control Notes

- Generated assets should be committed if reused
- Runtime-only assets are not persisted
