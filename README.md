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

## Build and test

HavenStudio targets .NET 10. From the repository root:

```text
dotnet build HavenStudio/HavenStudio.csproj
dotnet test HavenStudio.Tests/HavenStudio.Tests.csproj
```

The automated tests check editor and format behaviour. They do not establish that a saved stage is safe or visually correct in-game.

## Credits

- GhzGangster and Jayveer for their various GCX and MDN projects
- Zoft for his dictionary contributions
- TrikzMe for some LT3 reverse engineering
