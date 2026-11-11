# Studio-Standard Modular Training Bay

**Unreal Engine content pack - public documentation**

Publisher: **Haishan** · Author: Christon Chan · Support: [Chanjc03@gmail.com](mailto:Chanjc03@gmail.com)

A modular kit of 80 static meshes for building enclosed industrial training bays, workshops and service interiors in Unreal Engine: walls, floors, ceilings, columns, beams, door and window frames, catwalks, stairs, railings, trims, pipes and props, with custom collision, lightmap UVs, two master materials, a demo map and an overview map.

Contents of this page: [Requirements](#requirements) · [Installation](#installation) · [Quick start](#quick-start) · [What is included](#what-is-included) · [Grid and pivot rules](#grid-and-pivot-rules) · [Collision and lightmaps](#collision-and-lightmaps) · [Materials](#materials) · [Lighting](#lighting) · [Engine version support](#engine-version-support) · [Known limitations](#known-limitations) · [Mesh list](#mesh-list) · [License and provenance](#license-and-provenance) · [AI disclosure](#ai-disclosure) · [Support](#support)

## Requirements

- Unreal Engine 5.0, 5.1, 5.2, 5.3, 5.4, 5.5, 5.6, 5.7 or 5.8 (each version opened and checked).
- Content only: no plugins, no code, no Blueprints. Works in Blueprint and C++ projects.
- Development platform tested: Windows. The content is not platform specific.
- Lumen (the UE5 default) is recommended for the demo maps.

## Installation

1. Open the product page on Fab and add the pack to your library.
2. In the Epic Games Launcher (Unreal Engine > Library > Fab Library) or the Fab plugin inside the editor, choose **Add to Project** and pick a project with a supported engine version.
3. The pack is added to your project as `Content/ModularTrainingBay` (`/Game/ModularTrainingBay`). No restart or plugin setup is needed.
4. Recommended editor settings while building: grid snap 100 (50 for trims), rotation snap 90 degrees.

## Quick start

1. Open `/Game/ModularTrainingBay/Maps/L_TB_Overview` to browse every mesh, laid out by category with labels.
2. Open `/Game/ModularTrainingBay/Maps/L_TB_Demo` to see an enclosed, fully roofed training bay assembled only from the kit, with firing lanes, a mezzanine catwalk and stairs, lockers, consoles, pipe runs and three adjoining rooms (corridor, equipment room, control room).
3. Drag meshes from `/Game/ModularTrainingBay/Meshes` into your level and snap them using the rules below. Swap looks by assigning another `MI_TB_*` instance or a child instance of `M_TB_Master`.

## What is included

- 80 Static Meshes under /Game/ModularTrainingBay/Meshes (Openings 7, Pipes 6, Props 12, Structure/Beams 4, Structure/Ceilings 3, Structure/Columns 3, Structure/Floors 5, Structure/Walls 20, Trims 7, Utilities 2, Walkways 11).
- 2 master materials (M_TB_Master opaque, M_TB_Glass_Master translucent) and 14 material instances (MI_TB_*).
- 36 textures (12 sets x BaseColor _D, packed _ARM, DirectX normal _N), 1024x1024.
- 2 maps: L_TB_Demo (an enclosed, fully roofed training bay with three adjoining rooms seen through the doors and windows) and L_TB_Overview (every mesh laid out by category with labels).

### Folder structure

```
/Game/ModularTrainingBay/
  Docs/       README.txt (this documentation as plain text)
  Maps/       L_TB_Demo, L_TB_Overview
  Materials/  M_TB_Master, M_TB_Glass_Master, MI_TB_*
  Meshes/     Structure/(Walls, Floors, Ceilings, Columns, Beams), Openings, Trims, Walkways, Utilities, Pipes, Props
  Textures/   Kit, Props
```

## Grid and pivot rules

- Units: centimeters, Z up. Grid: 100 / 200 / 400 cm. Story height: 400 cm. Set the editor grid snap to 100 (or 50 for trims).
- Walls: 20 cm thick, centered on the grid line, length along X, pivot at bottom center. Place at Z = 0.
- Outer corners: add SM_TB_Wall_CornerPost_20x20_H400 at the grid intersection. T-junctions need no extra piece.
- Floors: 20 cm thick, pivot at bottom center. Place at Z = -20 so the top surface is the walking surface (Z = 0).
- Ceiling decks: pivot at bottom center, place at Z = 380.
- Columns: pivot at bottom center, place on grid intersections. Beams: pivot at the bottom flange center, length along X.
- Door and window frames: copy the host wall transform exactly (same pivot). Window sill 110 cm. Door openings 120x250 and 300x300.
  The roller shutter (108 cm wide) fits the 120 cm door opening.
- Kick plates face local +Y (pivot on the back face); place them against a wall face (wall center line +/- 10 cm).
- Catwalk pieces: pivot at the walking surface center so they line up with the top of the stair at Z = 200.
  SM_TB_Stair_Steel_W100_H200 rises 200 cm in ten 20 cm steps, pivot at the footprint bottom center, top step at local -Y (bottom step at +Y).
- Pipes (10 cm diameter) are modeled vertical (along +Z). Straight pipes and fittings pivot at the center of the bottom mouth (facing -Z).
  The other mouths sit on 10 cm offsets (elbow: +10 X / +10 Z, facing +X; tee: -10 X and +10 X at +10 Z), so pipe runs snap on a 10 cm grid. Rotate pieces to run pipes horizontally.
- UV0 uses world-scale box mapping (concrete 1 tile = 200 cm, steel 100 cm, caution stripes 50 cm), so textures continue across neighboring pieces.

Example: an 8 m x 8 m room is four `SM_TB_Floor_Concrete_400x400` at Z = -20 (centers at 200 / 600 on X and Y), `SM_TB_Wall_*_W400_H400` pieces centered on the grid lines at Z = 0, `SM_TB_Wall_CornerPost_20x20_H400` at the outer corners and four `SM_TB_Ceiling_Deck_400x400` at Z = 380.

## Collision and lightmaps

- Every mesh has custom UCX convex collision (1 to 7 hulls). Use the Player Collision view (Alt+C) in the editor viewport to inspect it.
- `SM_TB_Decal_FloorLane` has a thin collision box; it is set to NoCollision in the demo map.
- Light Map Coordinate Index is 1 on every mesh. UV channel 1 is generated by Unreal from UV 0 (Generate Lightmap UVs), with minimum lightmap resolutions of 64 to 256 depending on size. Lightmap UVs were checked for overlapping and out-of-range texels on all 80 meshes in every supported engine version (none found).
- LOD0 only; Nanite is disabled on all meshes and can be enabled per mesh by the user (the console glass slot is translucent).

## Materials

- `M_TB_Master` (opaque) and `M_TB_Glass_Master` (translucent), 14 material instances `MI_TB_*`.
- ARM texture: R = ambient occlusion, G = roughness, B = metallic. Normal maps are DirectX (green down).
- Parameters: `BaseTint`, `RoughnessScale`; glass: `Color`, `Opacity`, `Roughness`, `Metallic`, `EmissiveColor`, `EmissiveStrength`.
- UV0 uses world-scale box mapping (concrete 1 tile = 200 cm, steel 100 cm, caution stripes 50 cm), so textures continue across neighboring pieces.

## Lighting

- The maps use movable lights only: Directional Light, Sky Light (real-time capture), Sky Atmosphere, Exponential Height Fog and Rect Lights.
- Force No Precomputed Lighting is enabled, so no lighting build is required.
- The maps are set up for Lumen global illumination and reflections (UE5 defaults). Without Lumen the interior looks darker.

## Engine version support

| Engine version | Opened and checked | Result |
|---|---|---|
| 5.0 | Yes | 80 meshes, collision and materials present; both maps load with 0 missing meshes; 0 errors, 0 warnings |
| 5.1 | Yes | same |
| 5.2 | Yes | same |
| 5.3 | Yes | same |
| 5.4 | Yes | same |
| 5.5 | Yes | same |
| 5.6 | Yes | same |
| 5.7 | Yes | same |
| 5.8 | Yes | same |

Versions not listed have not been tested.

## Known limitations

- LOD0 only (no additional LODs). Triangle counts are 12 to 6,372 per mesh, 38,958 in total.
- Nanite is not enabled by default.
- Textures are 1024x1024.
- The demo maps rely on dynamic lighting (Lumen recommended).
- This is an environment kit, not a gameplay template: no Blueprints, no code, no characters, no animations, no audio.

## Mesh list

| Mesh | Folder | Size (cm) | Triangles | Collision hulls | Lightmap res | Source |
|---|---|---|---|---|---|---|
| SM_TB_DoorFrame_Steel_W120_H250 | Openings | 136x24x259 | 291 | 6 | 128 | Original geometry |
| SM_TB_DoorFrame_Steel_W300_H300 | Openings | 316x24x309 | 291 | 6 | 128 | Original geometry |
| SM_TB_DoorFrame_Steel_W80_H210 | Openings | 92x24x217 | 291 | 6 | 128 | Original geometry |
| SM_TB_Door_Rollershutter_W108_H240 | Openings | 108x30x240 | 552 | 1 | 128 | Derived from Poly Haven 'rollershutter_door' (CC0) |
| SM_TB_WindowFrame_Steel_Obs_W80_H200 | Openings | 90x24x212 | 507 | 1 | 128 | Original geometry |
| SM_TB_WindowFrame_Steel_W140_H100 | Openings | 152x24x114 | 551 | 1 | 64 | Original geometry |
| SM_TB_WindowFrame_Steel_W300_H100 | Openings | 312x24x114 | 551 | 1 | 128 | Original geometry |
| SM_TB_Pipe_D10_Bracket_A | Pipes | 11.1x15.6x3.2 | 278 | 1 | 32 | Derived from Poly Haven 'modular_pipes' (CC0) |
| SM_TB_Pipe_D10_Elbow90 | Pipes | 15.3x10.5x15.3 | 864 | 2 | 32 | Derived from Poly Haven 'modular_pipes' (CC0) |
| SM_TB_Pipe_D10_L100 | Pipes | 10x10x100 | 144 | 1 | 64 | Derived from Poly Haven 'modular_pipes' (CC0) |
| SM_TB_Pipe_D10_L200 | Pipes | 10x10x200 | 288 | 1 | 128 | Derived from Poly Haven 'modular_pipes' (CC0) |
| SM_TB_Pipe_D10_Tee | Pipes | 20x10x15 | 300 | 3 | 32 | Derived from Poly Haven 'modular_pipes' (CC0) |
| SM_TB_Pipe_D10_Valve_A | Pipes | 15.1x25.3x40 | 4910 | 1 | 32 | Derived from Poly Haven 'modular_pipes' (CC0) |
| SM_TB_Decal_FloorLane | Props | 200x20x0.3 | 12 | 1 | 128 | Original geometry |
| SM_TB_Prop_Barrel_A | Props | 63.4x63.9x93 | 1473 | 1 | 64 | Derived from Poly Haven 'barrel_03' (CC0) |
| SM_TB_Prop_Bench | Props | 150x40x45 | 408 | 1 | 64 | Original geometry |
| SM_TB_Prop_Console | Props | 126x60x108.3 | 512 | 2 | 64 | Original geometry |
| SM_TB_Prop_ElectricalCabinet_A | Props | 52x43.2x112 | 4404 | 1 | 64 | Derived from Poly Haven 'utility_box_01' (CC0) |
| SM_TB_Prop_GearRack | Props | 102x43x181 | 490 | 2 | 128 | Original geometry |
| SM_TB_Prop_Locker | Props | 94x56x190 | 916 | 2 | 128 | Original geometry |
| SM_TB_Prop_MetalRack_A | Props | 91.5x60x190 | 6372 | 1 | 128 | Derived from Poly Haven 'worn_metal_rack' (CC0) |
| SM_TB_Prop_NumberPlate | Props | 30x2.4x20 | 455 | 1 | 32 | Original geometry |
| SM_TB_Prop_Stool | Props | 32x32x46 | 460 | 1 | 32 | Original geometry |
| SM_TB_Prop_TargetPlate | Props | 56x6x80 | 250 | 1 | 64 | Original geometry |
| SM_TB_Prop_TargetStand | Props | 70x44x160 | 400 | 3 | 64 | Original geometry |
| SM_TB_Beam_SteelChannel_L200 | Structure/Beams | 200x20x30 | 60 | 1 | 128 | Original geometry |
| SM_TB_Beam_SteelI_L100 | Structure/Beams | 100x20x30 | 60 | 1 | 64 | Original geometry |
| SM_TB_Beam_SteelI_L200 | Structure/Beams | 200x20x30 | 60 | 1 | 128 | Original geometry |
| SM_TB_Beam_SteelI_L400 | Structure/Beams | 400x20x30 | 60 | 1 | 256 | Original geometry |
| SM_TB_Ceiling_Deck_100x100 | Structure/Ceilings | 100x100x20 | 68 | 1 | 64 | Original geometry |
| SM_TB_Ceiling_Deck_200x200 | Structure/Ceilings | 200x200x20 | 116 | 1 | 128 | Original geometry |
| SM_TB_Ceiling_Deck_400x400 | Structure/Ceilings | 400x400x20 | 212 | 1 | 256 | Original geometry |
| SM_TB_Column_Concrete_40x40_H400 | Structure/Columns | 43.1x43.1x400 | 396 | 1 | 256 | Original geometry |
| SM_TB_Column_Concrete_T_H400 | Structure/Columns | 47.1x34.2x400 | 440 | 2 | 256 | Original geometry |
| SM_TB_Column_SteelH_30_H400 | Structure/Columns | 50x50x400 | 108 | 2 | 256 | Original geometry |
| SM_TB_Floor_Concrete_100x100 | Structure/Floors | 100x100x20.5 | 36 | 1 | 64 | Original geometry |
| SM_TB_Floor_Concrete_200x200 | Structure/Floors | 200x200x20.5 | 36 | 1 | 128 | Original geometry |
| SM_TB_Floor_Concrete_400x400 | Structure/Floors | 400x400x20.5 | 36 | 1 | 256 | Original geometry |
| SM_TB_Floor_Concrete_SafetyEdge_200x200 | Structure/Floors | 200x200x20 | 20 | 1 | 128 | Original geometry |
| SM_TB_Floor_DiamondPlate_200x200 | Structure/Floors | 200x200x20 | 20 | 1 | 128 | Original geometry |
| SM_TB_Wall_Cladding_W200_H400 | Structure/Walls | 200x23.1x400 | 324 | 1 | 256 | Original geometry |
| SM_TB_Wall_Cladding_W400_H400 | Structure/Walls | 400x23.1x400 | 516 | 1 | 256 | Original geometry |
| SM_TB_Wall_ConcretePanel_W400_H400 | Structure/Walls | 400x23.1x400 | 276 | 1 | 256 | Original geometry |
| SM_TB_Wall_Concrete_DoorLarge_W400_H400 | Structure/Walls | 400x23.1x400 | 204 | 3 | 256 | Original geometry |
| SM_TB_Wall_Concrete_Door_W100_H400 | Structure/Walls | 100x23.1x400 | 156 | 3 | 256 | Original geometry |
| SM_TB_Wall_Concrete_Door_W200_H400 | Structure/Walls | 200x23.1x400 | 204 | 3 | 256 | Original geometry |
| SM_TB_Wall_Concrete_ObsWindow_W200_H400 | Structure/Walls | 200x23.1x400 | 192 | 4 | 256 | Original geometry |
| SM_TB_Wall_Concrete_W100_H200 | Structure/Walls | 100x23.1x200 | 228 | 1 | 128 | Original geometry |
| SM_TB_Wall_Concrete_W100_H400 | Structure/Walls | 100x23.1x400 | 228 | 1 | 256 | Original geometry |
| SM_TB_Wall_Concrete_W200_H200 | Structure/Walls | 200x23.1x200 | 252 | 1 | 128 | Original geometry |
| SM_TB_Wall_Concrete_W200_H400 | Structure/Walls | 200x23.1x400 | 252 | 1 | 256 | Original geometry |
| SM_TB_Wall_Concrete_W400_H200 | Structure/Walls | 400x23.1x200 | 252 | 1 | 256 | Original geometry |
| SM_TB_Wall_Concrete_W400_H400 | Structure/Walls | 400x23.1x400 | 252 | 1 | 256 | Original geometry |
| SM_TB_Wall_Concrete_Window_W200_H400 | Structure/Walls | 200x23.1x400 | 192 | 4 | 256 | Original geometry |
| SM_TB_Wall_Concrete_Window_W400_H400 | Structure/Walls | 400x23.1x400 | 240 | 4 | 256 | Original geometry |
| SM_TB_Wall_CornerPost_20x20_H400 | Structure/Walls | 20x23.1x400 | 84 | 1 | 256 | Original geometry |
| SM_TB_Wall_EndCap_20_H200 | Structure/Walls | 6.9x22.8x200 | 308 | 1 | 128 | Original geometry |
| SM_TB_Wall_EndCap_20_H400 | Structure/Walls | 6.9x22.8x400 | 308 | 1 | 256 | Original geometry |
| SM_TB_Wall_InnerCorner_L100_H400 | Structure/Walls | 111.2x111.2x400 | 216 | 2 | 256 | Original geometry |
| SM_TB_Wall_Low_W200_H100 | Structure/Walls | 200x23.1x100 | 752 | 1 | 128 | Original geometry |
| SM_TB_Trim_CornerGuard_H120 | Trims | 13x13.6x120 | 68 | 2 | 64 | Original geometry |
| SM_TB_Trim_FloorJoint_L200 | Trims | 200x6x0.9 | 24 | 1 | 128 | Original geometry |
| SM_TB_Trim_KickPlate_Caution_L100 | Trims | 100x1.5x15 | 20 | 1 | 64 | Original geometry |
| SM_TB_Trim_KickPlate_Caution_L200 | Trims | 200x1.5x15 | 20 | 1 | 128 | Original geometry |
| SM_TB_Trim_KickPlate_Caution_L400 | Trims | 400x1.5x15 | 20 | 1 | 256 | Original geometry |
| SM_TB_Trim_KickPlate_CornerIn | Trims | 15.6x15.6x15 | 68 | 2 | 32 | Original geometry |
| SM_TB_Trim_KickPlate_CornerOut | Trims | 16.5x16.5x15 | 68 | 2 | 32 | Original geometry |
| SM_TB_Utility_CableTray_L200 | Utilities | 200x40x10 | 528 | 1 | 128 | Original geometry |
| SM_TB_Utility_LightChannel_L200 | Utilities | 200x20x8 | 112 | 1 | 128 | Original geometry |
| SM_TB_Catwalk_Corner_L_W100 | Walkways | 151x150.7x123 | 805 | 6 | 64 | Original geometry |
| SM_TB_Catwalk_End_W100 | Walkways | 52.5x101x123 | 556 | 4 | 64 | Original geometry |
| SM_TB_Catwalk_Straight_W100_L200 | Walkways | 200x101x123 | 404 | 3 | 128 | Original geometry |
| SM_TB_Catwalk_T_W100 | Walkways | 200x150.5x123 | 915 | 7 | 128 | Original geometry |
| SM_TB_Railing_End_L100 | Walkways | 100x5x105 | 152 | 1 | 64 | Original geometry |
| SM_TB_Railing_Post_H100 | Walkways | 12x12x105 | 124 | 1 | 64 | Original geometry |
| SM_TB_Railing_Steel_L200 | Walkways | 200x5x105 | 132 | 1 | 128 | Original geometry |
| SM_TB_Ramp_Concrete_W200_L400_H100 | Walkways | 200x404x128 | 96 | 1 | 256 | Original geometry |
| SM_TB_Stair_Landing_200x200 | Walkways | 200x200x18 | 308 | 1 | 128 | Original geometry |
| SM_TB_Stair_Landing_Corner_200x200 | Walkways | 200x200x18.2 | 568 | 2 | 128 | Original geometry |
| SM_TB_Stair_Steel_W100_H200 | Walkways | 102x304.7x301.5 | 836 | 3 | 128 | Original geometry |

## License and provenance

The pack is licensed to buyers under the Fab End User License Agreement and the license tier purchased on Fab.

**Original content.** 70 of the 80 meshes are original geometry made for this pack, together with their modular layout, collision, naming and documentation.

**Public domain (CC0) content.** The pack also contains public domain content released under CC0 1.0 Universal (https://creativecommons.org/publicdomain/zero/1.0/). CC0 content is free for anyone to use; it is **not** original work of the publisher and is not presented as such. All 12 texture sets are derived from CC0 textures, and 10 meshes are derived from CC0 models. Five prop texture sets (PipeMetal, RollerShutter, MetalRack, Barrel, ElectricalCabinet) were reworked: only the source UV layout and form detail (normal and ambient occlusion) were kept, with new paint colors, edge wear, scratches, grime, roughness and metallic maps, a micro-normal layer and stencil markings. They remain CC0-derived. Source licenses: Poly Haven https://polyhaven.com/license , ambientCG https://docs.ambientcg.com/license/ .

### Meshes derived from CC0 models (10 of 80 meshes)

Changes made: isolated from the source scene, transforms applied, re-pivoted to snap points (pipe fittings pivot at the pipe mouth on a 10 cm grid), renamed, custom UCX collision added, lightmap UVs generated in Unreal (channel 1, checked for no overlaps), materials remapped to this pack's material instances.

- SM_TB_Pipe_D10_L100, _L200, _Elbow90, _Tee, _Valve_A, _Bracket_A: Poly Haven "modular_pipes" by Kuutti Siitonen, CC0 1.0 - https://polyhaven.com/a/modular_pipes
- SM_TB_Door_Rollershutter_W108_H240: Poly Haven "rollershutter_door" by MP, CC0 1.0 - https://polyhaven.com/a/rollershutter_door
- SM_TB_Prop_MetalRack_A: Poly Haven "worn_metal_rack" by Luca B, CC0 1.0 - https://polyhaven.com/a/worn_metal_rack
- SM_TB_Prop_Barrel_A: Poly Haven "barrel_03" by Serhii Khromov, CC0 1.0 - https://polyhaven.com/a/barrel_03
- SM_TB_Prop_ElectricalCabinet_A: Poly Haven "utility_box_01" by James Ray Cock, CC0 1.0 - https://polyhaven.com/a/utility_box_01

### Textures derived from CC0 textures (all 12 texture sets / 36 textures)

- T_TB_ConcreteWall_D/_ARM/_N: Poly Haven "garage_floor" by Jenelle van Heerden, CC0 1.0 - https://polyhaven.com/a/garage_floor (Resized to 1K, desaturated, cooled, contrast adjusted, ARM packed, DirectX normal)
- T_TB_ConcreteFloor_D/_ARM/_N: Poly Haven "hangar_concrete_floor" by Dimitrios Savva, CC0 1.0 - https://polyhaven.com/a/hangar_concrete_floor (Resized to 1K, brightened, desaturated, ARM packed, DirectX normal)
- T_TB_SteelOxidized_D/_ARM/_N: ambientCG "MetalPlates013" by ambientCG, CC0 1.0 - https://ambientcg.com/a/MetalPlates013 (Resized to 1K, AO/Roughness/Metallic packed to ARM, slight color shift)
- T_TB_SteelCladding_D/_ARM/_N: ambientCG "CorrugatedSteel007A" by ambientCG, CC0 1.0 - https://ambientcg.com/a/CorrugatedSteel007A (Resized to 1K, recolored to galvanized grey, ARM packed)
- T_TB_DiamondPlate_D/_ARM/_N: ambientCG "DiamondPlate008A" by ambientCG, CC0 1.0 - https://ambientcg.com/a/DiamondPlate008A (Resized to 1K, ARM packed)
- T_TB_SafetyYellow_D/_ARM/_N: ambientCG "PaintedMetal009" by ambientCG, CC0 1.0 - https://ambientcg.com/a/PaintedMetal009 (Resized to 1K, ARM packed)
- T_TB_CautionStripe_D/_ARM/_N: Poly Haven "metal_plate_02" by Rob Tuytel, CC0 1.0 - https://polyhaven.com/a/metal_plate_02 (Stripe pattern is original (script-generated); CC0 texture used only for grime/normal modulation)
- T_TB_PipeMetal_D/_ARM/_N: Poly Haven "modular_pipes" by Kuutti Siitonen, CC0 1.0 - https://polyhaven.com/a/modular_pipes (Reworked set derived from the CC0 1K maps: kept only the source UV layout and form detail (normal and ambient occlusion); new process-green paint with safety-yellow valve wheels and white pipe bands every 50 cm, new edge wear, scratches, height-based grime, newly authored roughness and metallic maps and an added micro-normal layer; normal converted to DirectX)
- T_TB_RollerShutter_D/_ARM/_N: Poly Haven "rollershutter_door" by MP, CC0 1.0 - https://polyhaven.com/a/rollershutter_door (Reworked set derived from the CC0 1K maps: kept only the source UV layout and form detail (normal and ambient occlusion); new slate blue-grey paint, yellow/black hazard band at the bottom, "BAY 02 / KEEP CLEAR" stencil, vertical rain streaks, new edge wear, scratches, height-based grime, newly authored roughness and metallic maps and an added micro-normal layer; normal converted to DirectX)
- T_TB_MetalRack_D/_ARM/_N: Poly Haven "worn_metal_rack" by Luca B, CC0 1.0 - https://polyhaven.com/a/worn_metal_rack (Reworked set derived from the CC0 1K maps: kept only the source UV layout and form detail (normal and ambient occlusion); new orange uprights and dark blue-grey shelves, "MAX 150 KG / PER SHELF" stencils on the shelf fronts, new edge wear, scratches, height-based grime, newly authored roughness and metallic maps and an added micro-normal layer; normal converted to DirectX)
- T_TB_Barrel_D/_ARM/_N: Poly Haven "barrel_03" by Serhii Khromov, CC0 1.0 - https://polyhaven.com/a/barrel_03 (Reworked set derived from the CC0 1K maps: kept only the source UV layout and form detail (normal and ambient occlusion); new amber drum body with graphite lids, white label band with "TB-200L / NON-POTABLE" stencil, new edge wear, scratches, height-based grime, newly authored roughness and metallic maps and an added micro-normal layer; normal converted to DirectX)
- T_TB_ElectricalCabinet_D/_ARM/_N: Poly Haven "utility_box_01" by James Ray Cock, CC0 1.0 - https://polyhaven.com/a/utility_box_01 (Reworked set derived from the CC0 1K maps: kept only the source UV layout and form detail (normal and ambient occlusion); new olive-green enclosure, red accents replacing the yellow warning blocks, "TB-EP 04 / 400 V" stencil on the doors, new edge wear, scratches, height-based grime, newly authored roughness and metallic maps and an added micro-normal layer; normal converted to DirectX)


**Font.** No font files are included. The stencil markings on four prop texture sets were rasterized from the Barlow Condensed font, used under the SIL Open Font License 1.1 (https://openfontlicense.org/).

No other third-party software, audio, code or plugins are included. Engine content (Epic) is referenced only by the demo maps' lighting actors.

## AI disclosure

This product is marked **Created with AI** on Fab. The 70 original meshes, the layout of the two maps and the new paint, wear and stencil layers of the five reworked prop texture sets were generated by procedural Blender, Unreal and Python scripts written with an AI assistant. No generative image, 3D-mesh or video models were used. All store images and the product video are real Unreal Engine renders of the included maps.

## Support

- Email: [Chanjc03@gmail.com](mailto:Chanjc03@gmail.com) (publisher: Haishan). Please include your engine version and a screenshot if something looks wrong.
- The same documentation is included in the pack at `/Game/ModularTrainingBay/Docs/README.txt`.
