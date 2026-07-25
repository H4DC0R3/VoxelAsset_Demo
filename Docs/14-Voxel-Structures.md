# Voxel Structures (Multi-Asset Buildings)

A building is rarely one asset. A five-storey block is usually one asset per floor, or a set of modular props assembled in the level. Left alone, the structural solver treats each of those as an independent object that holds itself up — so destroying the ground floor changes nothing above it.

A **Voxel Structure** links several voxel actors so the solver understands they hold each other up.

> **Work in progress.** Structures extend Collapse mode, which is still being performance-evaluated on large objects. See [Physics and Destruction](11-Physics-and-Destruction.md).

## The Problem

Every voxel object is anchored by one face of its own bounding box (the Anchor Face). For a single object standing on the floor that is exactly right. For floor 3 of a building it is not: floor 3's own underside is its anchor, so it believes the world is holding it — in mid-air, with nothing below it.

Cut the ground floor away and floor 3 never moves, because nothing ever told it that what it was standing on is gone.

## Setup

Place a **Voxel Structure** actor in the level and list the members.

- **Members** — the voxel actors that make up this building. Order does not matter: which piece rests on which is worked out from where the pieces actually are, so re-ordering the list changes nothing.
- **Is Foundation** (per member) — this piece is held by the world itself, whatever is or is not under it. Leave it off for an ordinary stack; whatever ends up at the bottom is treated as grounded anyway. Turn it on for pieces anchored to something outside the building — a floor bolted into a cliff, the far end of a bridge — because grounding is resolved by following supports downwards, and those walks have to end somewhere.
- **Contact Tolerance Voxels** — how close two members have to be, in voxels, to count as touching. Modular pieces rarely meet at exactly zero distance. Raise it if pieces are loosely placed; lower it if two members that should be independent keep getting linked.
- **Contact Survival Fraction** — how much of a joint has to survive before the piece above stops counting as held. Deliberately low (0.15 by default): leaving one column of a floor standing *does* still hold the floor above, and that column is then weighed carrying all of it, so the per-voxel solver breaks it on its own a moment later. Raising this takes that decision away from the solver.
- **Debug Draw Support Links** — draws the derived "A holds B up" arrows during play. Green where the joint is intact, red where it has been destroyed.
- **Solve On Begin Play** — weigh the whole building as soon as it is assembled instead of waiting for something to hit it. Off by default: a building that is under-built for its material would come down the instant you press play, before anyone touched it. Turn it on to find out whether a structure stands up under its own weight at all.

Every member must have **Can Physics** enabled and its Physics Mode set to **Collapse**. Members in other modes are skipped with a warning in the log — the other modes are per-object by design and are left alone.

## How It Works

The members are **not** merged into one solve. There is no common cell grid to merge them into (members may sit at any transform relative to each other) and a whole building is far more voxels than the per-voxel solver is sized for.

Instead each member goes on solving itself, on its own worker thread, exactly as it always did. The structure supplies the two things it was missing:

- **What is actually holding it** — the members underneath, at the patches where they touch, instead of a face of its own bounding box.
- **What is standing on it** — the weight of everything above, pressed into the cells that carry it.

That is enough for the behaviour. The ground floor is now weighed carrying four floors, so cutting a column out of it overloads what is left and the joint gives way. Losing the ground floor then takes the anchor out from under floor 2, which comes down onto floor 1, and so on.

The cascade runs a member per round rather than all at once, which is what a building coming down looks like anyway.

### Support is derived, not authored

Which member rests on which is worked out from the members' world bounds and world gravity when play begins:

1. One member's centre must be above the other's along world up.
2. The top of the lower one must meet the bottom of the upper one within Contact Tolerance Voxels.
3. They must overlap side to side.

This is measured against the voxels each member actually has, not its grid, so an asset with a lot of empty space above its contents still stacks correctly. It also works at any rotation, because it asks the question in world space.

### When a member comes loose

A member that loses its support leaves as **one rigid body**: everything still standing in it becomes a single fragment and the component is emptied. It is not routed through island detection — the whole member came loose and the answer is already known, and a floor's worth of voxels would blow the island search budget and be left attached for it.

## Blueprint API

The structure can be reconfigured at runtime — a building assembled by gameplay, or a piece added after the level loads.

- **Rebuild Structure** — re-derives the support graph from where the members are now. Called automatically once when play begins; call it again if a member is moved or added while the game is running.
- **Add Structure Member** (Actor, Is Foundation) — adds an actor to the structure and rebuilds.
- **Remove Structure Member** (Actor) — removes an actor and rebuilds.
- **Get Grounded Member Count** — how many members still reach the ground. Zero means the whole building has come loose.

## Merging a Structure Into One Asset

A structure is several actors at runtime. For a piece of set dressing that no longer needs to come apart floor by floor — or as a starting point for hand-editing — the whole thing can be baked into a single voxel asset.

Select the Voxel Structure actor and press **Merge Into New Voxel Asset** in the Details panel.

It asks where to save, bakes every member into one new asset, and saves it. **Nothing in the level is touched** — the building stays exactly as it is, and a new asset appears in the content browser.

What it does:

- **One grid out of many.** Members sit at arbitrary transforms with no grid in common, so the merge picks a single world-aligned grid covering all of them and rasterises each member's voxels into it. A member rotated at an odd angle still comes out solid. Everything outside the members is left empty.
- **One palette out of many.** A voxel stores a palette *index*, and the same index means different colours in two assets that use different palettes. When every member already shares a palette the indices are comparable and are copied through untouched. When they do not, the colours actually used are collected, de-duplicated, and given a new palette of their own, saved beside the merged asset, with every index remapped to match.
- **Voxel health** is preserved, and the physics defaults (Can Physics, Anchor Face, Structural Profile) come from the first member.

Notes:

- If the members do not all use the same voxel size, the **smallest** is used and coarser members are resampled — the other way round would throw away detail with no way to get it back. The summary says when this happened.
- A voxel indexes one byte, so a merge cannot keep more than 256 distinct colours. Past that, extra colours are mapped to the closest kept one and the summary warns.
- Where members overlap, the first one written wins, so the result does not depend on the order of the member list.

## Usage Notes

- Every member needs Physics Mode **Collapse**; the structure is ignored in Simple, Topple, and Shatter.
- If the building never reacts, turn on Debug Draw Support Links. A five-floor stack should show four links. No arrows at all means the members are further apart than Contact Tolerance Voxels, or they do not overlap side to side — the log says which.
- Structures are resolved on every machine independently and deterministically, so they need no replication of their own. See [Networking and Replication](12-Networking-Replication.md).
- The merge tool is an authoring tool: it reads what the members were built as, not what is left of them after something shot at them.
