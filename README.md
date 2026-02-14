# DVS ModelSwap Tool (FH5)

This tool is a guided pipeline for FH5 part swapping by overwriting existing assets.

It includes:
- Model Swap (QuickBMS extract -> patch -> optional reimport)
- Wheel Swap (file-based wheel modelbin + AO swapping; output folder or in-place patch)
- Direct Modelbin (batch modelbin swaps; does not reimport automatically)
- Modelbin Offset editor (manual XYZ delta edits in a modelbin file)
- Live Edit (Experimental) (in-memory editing; still under development, expect issues)
- QuickBMS Reimport (repack an edited folder into a car zip)

Important: Live Edit is still under active development. Expect bugs, crashes, and incomplete workflows.
If you hit issues or have recommendations, report them on Discord (links below).

## Support / Recommendations / Bug Reports (Discord)
- Join server: https://discord.gg/fh6
- Report issues here: https://discord.com/channels/1436870768435662900/1441668760028319835

## Quick Start
1. Put `FH5-ModelSwap.exe` in a folder you want to keep your outputs in.
2. Run the EXE once. It will create these folders next to the EXE if missing:
   - `tools/` (QuickBMS + scripts/templates)
   - `runtime/` (temporary working data)
   - `logs/` (run logs)
   - `output/` (organized output folders)
3. Pick a mode from the left sidebar and follow the on-screen steps.

## Output Location and Organization
All outputs go next to the EXE folder (not inside your repo):
- `output/<job_name>_<timestamp>/...`

The tool creates timestamped folders so operations do not overlap.
Examples:
- Wheel Swap: `output/<car>_wheelswap_raw_<timestamp>/` (zip inside the folder if enabled)
- Model Swap (export-only): `output/<car>_modelswap_raw_<timestamp>/raw/...`
- Live Edit bake outputs: `output/<car>_liveedit_<timestamp>/...`

## Release Folder Layout (Next To The EXE)
The EXE creates/uses these folders next to itself:
- `tools/`
  - `quickbms.exe` (required for extract/reimport)
  - `zip.bms` (required; must match your FH5 archive format)
  - `grub_parser.bt` (used by some parsing features)
- `runtime/` (temporary extracts / patch staging)
- `output/` (timestamped job folders)
- `logs/` (timestamped log files)

If QuickBMS is missing, the app may try to fetch it automatically depending on your build/release. If you are offline, make sure `tools/quickbms.exe` is present.

## Mode: Model Swap
What it does:
- Extracts target and donor car zips using QuickBMS + `zip.bms`
- Copies donor modelbin into the target part's modelbin path (enforces size rules)
- Patches the target `.carbin` (keeps exact byte size)
- Finds and swaps AO references, and copies AO swatches (strict mode can fail if AO is missing)
- Reimports/repackages (optional) using QuickBMS with the original zip as base

Typical workflow:
1. Select Target zip + Donor zip
2. Scan target parts + scan donor parts
3. Select the target part and donor part
4. Run swap
5. If you have an FH5 cars folder selected, the tool can reimport to a zip there. Otherwise it exports an unpacked folder into `output/`.

Non-negotiable rules the tool enforces:
- No custom part names: you overwrite an existing target part name
- Donor model size must be <= target model size
- Carbin size must remain EXACTLY the same (padding with 00 allowed)
- AO swap is expected (strict mode can hard-fail if it cannot be found/copied)

## Mode: Wheel Swap
Wheel Swap is file-based: you must provide actual `.modelbin` and `.swatchbin` paths on disk.

Two output styles:
1. Output folder mode (default)
   - Writes patched files to `output/<name>_wheelswap_raw_<timestamp>/`
   - Optionally creates a zip inside that same folder
2. Patch in place mode (optional)
   - Overwrites the selected Target modelbin and Target AO directly on disk
   - Creates timestamped backups next to each target file:
     - `<target>.bak` or `<target>.bak.<timestamp>`

If you are patching files inside the FH5 install or other protected folders, Windows permissions may block in-place overwrites.

## Common Issues / Tips
  - Check the newest log in `logs/` next to the EXE.
- QuickBMS extract/reimport fails:
  - Your `tools/zip.bms` may not match the archive format you are working with.
  - Make sure `tools/quickbms.exe` exists next to the EXE.
- In-place patch fails with Access Denied:
  - Move the files to a non-protected folder, patch them there, then copy them back.

## Mode: Direct Modelbin (Advanced)
Direct Modelbin is for batch swapping multiple modelbins at once (does not reimport automatically).

Key points:
- File-based swaps (no QuickBMS zip pipeline)
- Can do in-place or write to a new output file
- Can optionally patch carbins / swap AO when "Safe Swap Compatible (carbin patch)" is enabled and you are swapping in-place within a valid car folder structure
- Creates backups for in-place swaps

## Mode: Modelbin Offset (Editor)
Modelbin Offset is a manual modelbin editor.
It loads a `.modelbin`, parses mesh transform entries, and applies a user-defined XYZ delta to selected meshes (or filtered meshes), then saves the file.

This is offline and does not require FH5 running.

## Mode: Live Edit (Experimental)
Live Edit attaches to FH5 and edits transforms in memory.

Status:
- Under development
- Expect issues
- Workflows and offsets/bindings may change

If you hit problems, share your logs and steps on Discord:
https://discord.com/channels/1436870768435662900/1441668760028319835

## Mode: QuickBMS Reimport
Use this to repack an edited folder into a new car zip:
- Base car zip (original)
- Patch folder (edited files)
- Output zip (optional; defaults to `output/` next to the EXE)

This is useful when you want to edit files manually and then reimport cleanly using QuickBMS + `zip.bms`.

## Logs
Every run writes a log file here (next to the EXE):
- `logs/<timestamp>.log`

If something fails, the log is the first place to look.
When reporting issues on Discord, attach the newest log.

## Optional External Tools
The UI includes links for:
- 010 Editor (optional)
- 3DSimED (optional)

These are not installed automatically.

## Disclaimer
Distribution notice (non-copyright): This tool must not be shared, re-uploaded, mirrored, or bundled outside the
official Git source for this project. If you want to recommend it, link to the Git source (or the Discord) instead
of re-hosting binaries.
