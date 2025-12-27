# Blueprint API

The plugin exposes a comprehensive Blueprint API for runtime voxel interaction.

---

## Deletion Functions

- Delete single voxel
- Delete box region
- Delete sphere region
- Component-specific variants

---

## Damage Functions

- Apply damage to voxels
- Damage over box or sphere
- Automatic destruction when health reaches zero

---

## Returned Data

Deletion and damage functions return:
- World positions
- Palette colors
- Extra data
- Voxel scales
- Affected components

---

## Helper Functions

- Merge runtime voxel delete data
- Compute center of voxel positions

---

## Performance

- Query runtime performance snapshot
