# FH5 ModelSwap Tool

FH5 ModelSwap Tool is a guided, safe, automated pipeline for swapping FH5 car parts by overwriting existing model names. The output is a new car zip that is ready to drop back into the FH5 cars folder.

## What this tool does
- Extracts target and donor car zips using QuickBMS + zip.bms
- Copies a donor model into the target, renaming it to the target part name
- Enforces model size limits (donor must be <= target)
- Patches the target carbin, swaps AO references, and preserves exact byte size
- Reimports/repacks with QuickBMS using the original zip as base
- Provides a Modelbin Offset tab for batch XYZ adjustments across mesh entries

## Running the app
1. Place `FH5-ModelSwap-Tool.exe` next to `tools/`.
2. Run the EXE once; it will create `tools/`, `runtime/`, `logs/`, and `output/` if missing.
3. The app will automatically download QuickBMS on first run.
4. Use the step-by-step tabs (Paths → Scan Parts → Parts & Options → Run) to complete the swap.

## Optional external tools
The UI includes download links for:
- 010 Editor (optional)
- 3DSimED (optional)

These are not installed automatically.

## Folder layout
- `app/` : application source code
- `tools/` : QuickBMS scripts and templates
- `runtime/` : temp working folder for extracts/patching
- `output/` : finished zips
- `logs/` : detailed run logs

## Notes
- QuickBMS is downloaded at runtime from the official source.
- Replace `tools/zip.bms` with a correct zip.bms for your FH5 archives if needed.
- This tool does NOT create new models or bypass game restrictions.
- If windows cant find exe error 
