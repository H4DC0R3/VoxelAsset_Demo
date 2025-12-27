# Voxel Asset Editor

The Voxel Asset Editor is a dedicated editor for creating and modifying voxel assets.

---

## Editor Layout

The editor consists of:
- 3D Viewport
- Tool panels
- Palette editor
- Palette Creator editor

![Voxel Asset Editor Overview](Images/Overview/Editor.png)

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

## Palette Creator

The Palette Creator tab is used to create and edit custom palette slots used by voxel assets.

It provides a visual grid of palette slots and a set of controls to edit color and material-related parameters for each slot.

---

### Palette Grid

The grid displays all available palette slots.

- Each cell represents a palette slot
- Slots are indexed numerically
- Empty slots appear as dark cells
- Selecting a slot activates it for editing

Local palettes and custom palettes are displayed in the same grid.

---

### Color Controls

The color of the selected palette slot can be edited using HSV controls.

- **Hue**  
  Controls the base color hue

- **Saturation**  
  Controls color intensity

- **Value**  
  Controls brightness

- **Actual Color**  
  Displays the final color applied to the slot

The **Save Slot** button applies the current color to the selected palette slot.

---

### Material Parameters

Each palette slot also defines material-related parameters.

- **Emissive**  
  Controls emissive intensity for voxels using this slot

- **Roughness**  
  Controls surface roughness

- **Specular**  
  Controls specular reflection strength

- **Metallic**  
  Controls metallic appearance

- **Opacity**  
  Controls voxel opacity

These values are packed into the palette extras texture and used by the voxel material.

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


## Fixed Palettes and Custom Materials

The voxel system supports both fixed palettes and custom palettes, allowing control over color data and material behavior.

---

### Fixed Palettes (0–3)

Palette slots **0 to 3** are fixed palettes.

- Always available
- Stored directly in the voxel asset
- Intended for fast access and common color usage
- Cannot be reassigned to custom palette ranges

These palettes are ideal for frequently used colors or core asset materials.

---

### Custom Palettes (4–99)

Palette slots **4 to 99** are custom palettes.

- Selected using the **Custom** palette index field
- Editable through the Palette Creator
- Designed for extended or specialized color sets
- Suitable for larger or more complex assets

Custom palettes allow a broader range of color and material variations without affecting fixed palettes.

---

### Paint Palette Selection

The **Paint Palette** selector determines which palette range is active while painting voxels.

- Selecting **0–3** uses fixed palettes
- Selecting **Custom** activates the custom palette range
- The custom index defines which custom palette group is currently active

---

### Custom Materials

Voxel assets can use a custom base material instead of the default voxel material.

- The **Material** field allows selecting a custom material
- When a custom material is assigned, a material instance is created automatically
- Palette color and extras textures are passed to the material

The custom material must support the following parameters:

- Palette Color Texture
- Palette Extras Texture
- Palette Index
- Debug options (optional)

---

### Usage Notes

- Changing the active paint palette affects all voxels
- Custom materials must be compatible with the palette texture layout

--- 

## Debug Visualization

- Grid visualization
- Ruler visualization




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


## JSON Import / Export

- Export voxel data to JSON
- Import voxel data from JSON
- Optional palette import
- Optional odd grid alignment

---

## MagicaVoxel (.vox) Compatibility

The plugin provides basic compatibility with **MagicaVoxel (.vox)** files, allowing voxel data to be imported and exported for interoperability.

---

### Supported Operations

- **Export VOX**  
  Exports the current voxel asset geometry to a `.vox` file.

- **Import VOX**  
  Imports voxel geometry from a `.vox` file into the current voxel asset.

Only voxel occupancy and palette indices are transferred.

---

### Palette Index Mapping

- The **Palette Index (0–99)** field defines which internal palette range will be used during import or export.
- This allows mapping MagicaVoxel palettes into specific palette slots of the voxel asset.
- Existing palette slots outside the selected range are not modified.

---

### Limitations

- Only voxel data and palette indices are supported
- Material parameters (emissive, roughness, metallic, etc.) are not stored in `.vox` files
- Animations are not supported by the `.vox` format
- Advanced editor features such as LOD, streaming, and runtime metadata are not exported

---

### Usage Notes

- Importing a `.vox` file replaces the current voxel geometry
- Palette colors must be adjusted manually after import if needed
- The feature is intended for **data exchange**, not full asset round-tripping

---


### Usage Notes

- Palette changes are applied immediately after saving the slot
- Voxels using the modified slot update visually
- Palette slots can be reused across the entire voxel asset
- Material behavior depends on the active voxel material setup


---

## Undo / Redo

- Manual transaction system
- Each tool action is recorded
- Maximum history size is limited for performance

---
