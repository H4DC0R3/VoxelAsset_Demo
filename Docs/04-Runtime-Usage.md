# Runtime Usage

Voxel assets are rendered and managed at runtime using the Voxel Component.

---

## Voxel Actor

Placing a Voxel Asset in the Level creates a Voxel Actor that contains:
- A Voxel Component
- Optional static mesh proxy component

---

## Rendering Modes

- **Procedural Mesh**  
  Generated at runtime, supports destruction and animation

- **Baked Static Mesh Proxy**  
  Prebuilt mesh used for performance

The system can switch automatically based on settings.

---

## Rebuild Types

- Full rebuild
- Partial region rebuild
- Visual-only rebuild

---

## Coordinate Conversion

- World to voxel coordinates
- Voxel to world coordinates

---

## Collision

- Optional collision generation
- Aggregated simple collision per chunk
- Collision updates after rebuilds

---

## Runtime Safety

The system ensures:
- Original assets are not modified
- Runtime-only copies are created when needed
