# VFX and Particle System

The plugin ships a global voxel destruction VFX system built on Niagara. A single spawner actor placed in the level listens for every voxel edit and emits particles that match the voxels that were destroyed or damaged — same positions, same colors, same sizes.

## Global Particle Spawner

Destruction VFX is managed by one actor: **BP_VE_ParticleSpawner**.

Place a single instance in the level. It binds to the Voxel Physics Subsystem On Voxel Edited event and handles particles for the whole world from there, so individual destruction nodes and voxel actors do not each spawn their own effect.

Steps:

1. Drag BP_VE_ParticleSpawner from the plugin content (Resources/Blueprints) into the level.
2. Leave one instance per level. It manages destruction VFX globally.
3. Play. Destroying or damaging any voxel object emits particles automatically.

Because it drives everything from On Voxel Edited, it also reacts to structural collapse, not just direct hits — the same event carries destroy, damage, and both collapse causes.

## How It Works

On Voxel Edited hands the spawner the per-voxel payload for each edit: world positions, colors, and extras for the destroyed (and damaged) voxels. The spawner feeds that payload into a Niagara system that spawns one particle per voxel at the right place, tinted with the voxel's color and sized to the voxel.

The system is data-driven, so it scales from a single chipped voxel to a full wall coming down without any per-object setup.

## Included Content

The VFX content lives in the plugin under Resources:

- **BP_VE_ParticleSpawner** (Resources/Blueprints) — the global spawner actor.
- **NS_VE_Destruction_Example** (Resources/Particles) — the destruction Niagara system.
- **NM_VE_SpawnCount**, **NM_VE_SpawnLocationAndColor**, **NM_VE_VoxelSize** (Resources/Particles/Modules) — Niagara modules that map the voxel payload to spawn count, per-particle location/color, and particle size.
- **M_VoxelParticle** (Resources/Materials) — the particle material.

## Customizing

- Use NS_VE_Destruction_Example as a starting point and duplicate it for your own look.
- The modules read the voxel payload, so keep them in a custom system if you want positions, colors, and sizes to track the destroyed voxels.
- For fully custom VFX, bind On Voxel Edited yourself and drive any effect from the destroyed/damaged payload instead of using the spawner. See [Physics and Destruction](11-Physics-and-Destruction.md).

## Usage Notes

- One BP_VE_ParticleSpawner per level is enough; it is global.
- No wiring is needed on individual voxel actors or destruction nodes.
- On networked clients, VFX still fires: On Voxel Edited is raised on every machine after the edit is applied. See [Networking and Replication](12-Networking-Replication.md).
