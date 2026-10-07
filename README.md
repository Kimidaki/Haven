# HavenStudio - MGS4 Stage Editor

A utility that allows you to edit stages for all games running on the MGS4 engine.

**Supported Games**

- Metal Gear Solid 4
- Metal Gear Online 2
- Metal Gear Arcade

<img width="2136" height="1158" alt="image" src="https://github.com/user-attachments/assets/ee34b060-ecbb-406d-ad2b-f1d978d059a5" />

## OctoCamo editing (contribution branch)

This branch adds tools for inspecting and editing the surface data used when a character's OctoCamo contacts a stage. Haven reads the loaded stage's GEOM collision and OctoCamo table; the tools are not tied to a particular map. They are available only when the stage contains the required data.

1. Open a stage folder with **File > Open Folder**, then open its map. Keep a backup of the original stage files.
2. Choose **OctoCamo > Enable OctoCamo view** to show contact collision. Select a face to inspect its material/pattern and separate cloth colour. **Muscle texture** switches between a raw diffuse-pattern projection and the cloth-colour preview; neither is an exact render of the suit in-game.
3. For pattern images and stage-wide remapping, choose **OctoCamo > Load camo previews...** and select `slot_oct_list_online.slot` from the game installation. Haven remembers the selected folder. Then use **OctoCamo > Remap OctoCamo materials...** to assign a pattern to a GEOM material across the stage, including previously unassigned materials.
4. To edit individual or overlapping faces, click a face or use **Shift + left-drag** (or **OctoCamo > Box-select faces**) to list faces inside a box. Filter the list, tick individual faces or **Select all filtered**, and choose **Edit selected camo...** for a batch change. Focused and ticked faces remain outlined through the preview.
5. Choose **Save Map** to write pending polygon edits to the plaintext GEOM and stage-wide remaps to the OctoCamo table, as applicable.

Magenta indicates a missing mapping or preview. If a stage table contains conflicting duplicate rows for one material, Haven leaves that material unmapped and does not rewrite either row: the game's precedence is not yet established. Other materials remain available.

**Game files are not installed or encrypted by Save Map.** Existing `.enc` copies are not refreshed. Re-encrypt edited files with the correct key for *that stage*, validate the output, and test it in-game before replacing a known-good installation.

<!-- Add screenshots: OctoCamo view and inspector; overlapping-face box selection; stage-wide remapping dialog. -->

## Additional map-editor workflows (this branch)

The map editor also discovers supporting files from the opened stage rather than assuming one map's filenames or camera layout. These views have data to show only when Haven can decode the corresponding stage resource; an empty view on one stage does not mean every stage lacks that feature.

| Workflow | Where to find it | What an edit saves |
| --- | --- | --- |
| Placements and effects | **Placements**, **Effects**, **Add Object**, **Add Effect**; select an item for its Inspector | Writable GCX placement/script data or GEOM effects, according to the selected object's source. **Placement collision** separately previews referenced GEOM collision at the placed object's transform. |
| Spectator cameras | **View > Cameras**, then **Spectator cameras** in the outline | Camera position and target in the loaded GCX. The Inspector can **Use current view** or **Look through camera**. Only supported camera tables are shown. |
| Vegetation | **View > Vegetation**, then a PDL group or instance in the outline | Group movement or an individual instance's position in the linked PDL. Instance scale is previewed, not edited. |
| SDM area | **View > SDM Area**, then **SDM area** in the outline | The editable starting `area_max` radius in the loaded GCX. The final `area_min` boundary is displayed for context. This overlay starts hidden for each newly opened GEOM. |
| Lighting | **Lights** in the outline and **Game lighting** in the toolbar | Light edits use their loaded light file; Game lighting is a preview toggle. |

**Save Map** writes each dirty, supported source (such as GCX, GEOM, light files, PDL or OctoCamo data); it does not turn plaintext output into an installed game-ready stage. Inspect the save status and validate each changed file before encryption and in-game testing.

<!-- Add screenshots: placement collision; spectator-camera Inspector; PDL vegetation; SDM area overlay. -->

## Build and test

HavenStudio targets .NET 10. From the repository root:

```text
dotnet build HavenStudio/HavenStudio.csproj
dotnet test HavenStudio.Tests/HavenStudio.Tests.csproj
```

The automated tests check editor and format behaviour. They do not establish that a saved stage is safe or visually correct in-game.

To run the optional, read-only stage-discovery checks against a local stage in PowerShell:

```powershell
$env:HAVEN_STAGE_FEATURE_ROOT = 'C:\path\to\stage'
dotnet test HavenStudio.Tests/HavenStudio.Tests.csproj --filter FullyQualifiedName~StageFeatureDiscoveryTests
```

These checks exercise OctoCamo discovery and camera-table scanning/editing in memory; they do not change the stage files. They have been run on AA, VV and JJ. Older offset-specific OctoCamo fixture tests remain explicitly JJ-only and use `HAVEN_JJ_OCTOCAMO_STAGE` instead.

## Credits

- GhzGangster and Jayveer for their various GCX and MDN projects
- Zoft for his dictionary contributions
- TrikzMe for some LT3 reverse engineering
