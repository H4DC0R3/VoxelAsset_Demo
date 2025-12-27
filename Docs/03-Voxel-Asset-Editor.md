# Voxel Asset Editor

The Voxel Asset Editor is a dedicated editor for creating and modifying voxel assets.

---

## Editor Layout

The editor consists of:
- 3D Viewport
- Tool panels
- Palette editor
- Palette Creator editor
- Performance panel

---

## Edit Tools

Edit tools modify voxel geometry:
- Single
- Cube
- Wall
- Sphere
- Circle (2D)
- Line

Mirror options are available on X, Y, and Z axes.

---

## Paint Tools

Paint tools assign palette indices:
- Single
- Wall
- Fill
- Eyedropper
- Noise-based painting
- Line and shape painting

---

## Palette System

- Up to 256 palette slots
- Slots 0–3 are local palettes
- Slots 4–99 are custom palettes
- Each slot stores:
  - Color
  - Extra data (packed RGBA)

---

## Transform and Shape Operations

- Translate
- Rotate
- Center
- Flip
- Grow / Shrink
- Smooth
- Shell
- Remove floating islands

---

## Undo / Redo

- Manual transaction system
- Each tool action is recorded
- Maximum history size is limited for performance

---

## JSON Import / Export

- Export voxel data to JSON
- Import voxel data from JSON
- Optional palette import
- Optional odd grid alignment

---

## Debug Visualization

- Grid visualization
- Ruler visualization
- Debug voxel size and offsets
