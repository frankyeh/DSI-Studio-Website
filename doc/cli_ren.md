# Rename DICOM Files

Use `--action=ren` to recursively scan raw DICOM folders, move and rename every valid DICOM file into a standardized `<output>/<patient>/<sequence>/<image>` hierarchy, and optionally convert the organized studies into DSI Studio `.sz` source files and `.nii.gz` volumes.

To convert already-sorted DICOM series without moving raw files, use [`--action=src`](/doc/cli_t1.html). For 3D rendering and GUI command execution, use [`--action=vis`](/doc/cli_vis.html) (`--action=ren` only renames DICOM files).

## Examples

### Rename DICOM files under a directory

```bash
dsi_studio --action=ren --source=d:/raw_data
```

DSI Studio scans all subdirectories under `--source`, detects DICOM files by header signature (even when PACS files have no `.dcm` extension), skips non-DICOM files, and moves valid slices into `<output>/<patient>/<sequence>/<image>`:

```text
<output>/
  <AcqDate>_<Sex><Age>_<PatientName>/
    <SeriesNum>_<SeriesDescription>/
      <SeriesDescription>_i<InstanceNum>.dcm
```

To move the organized hierarchy into another directory on the same drive/filesystem, add `--output`:

```bash
dsi_studio --action=ren --source=d:/raw_data --output=d:/sorted_dicom
```

### Rename and convert to SRC/NIfTI

```bash
dsi_studio --action=ren --source=d:/dicom --to_src_nii=1
```

After renaming, DSI Studio scans `*.dcm` inside each `<output>/<patient>/<sequence>/` folder (merging adjacent series folders when they share the first filename) and writes `<patient>_s<SeriesNum>_<SeriesDescription>.sz` or `.nii.gz` directly inside `<output>/<patient>/`:

- **Multi-volume diffusion series (`b > 0`):** saved as `.sz`.
- **Single-volume / structural series:** saved as 3D `.nii.gz` (reoriented to LPS, with mismatched scout/localizer slices removed).
- **Multi-volume series without a b-table:** saved as 4D `.nii.gz`.

### Allow conversion outputs to be overwritten

```bash
dsi_studio --action=ren --source=d:/dicom --to_src_nii=1 --overwrite=1
```

By default (`--overwrite=0`), existing `.sz` and `.nii.gz` files in `<output>/<patient>/` are skipped. Setting `--overwrite=1` replaces existing `.sz` and `.nii.gz` conversion outputs while still refusing to overwrite destination `.dcm` files.

### Windows batch example

A single `--source` path already scans nested subfolders recursively. Use a shell loop when you want separate per-cohort runs and log files:

```bat
for /f "delims=" %%x in ('dir /ad /b "D:\MRI\CA"') do (
    call dsi_studio.exe --action=ren --source="D:\MRI\CA\%%x" > "%%x.log.txt"
)
```

### Linux and macOS batch example

```bash
for dir in /data/MRI/CA/*/; do
  [ -d "$dir" ] || continue
  name=$(basename "$dir")
  dsi_studio --action=ren --source="$dir" --to_src_nii=1 > "${name}.log.txt"
done
```

## Options

| Parameter | Default | Description |
|:--|:--|:--|
| `--action=ren` | required | Selects DICOM rename and organization mode. |
| `--source=<dir>` | required | Root directory containing raw DICOM files; all subdirectories are scanned recursively. Non-DICOM files are skipped (`not a DICOM file. Skipping`). |
| `--output=<dir>` | same as `--source` | Root directory for `<output>/<patient>/<sequence>/<image>`. Must be on the same drive/filesystem as `--source`. |
| `--to_src_nii=<0\|1>` | `0` | Set to `1` to convert each renamed `<output>/<patient>` folder into `.sz` (diffusion with `b > 0`) and `.nii.gz` (3D structural or 4D non-diffusion) files. |
| `--overwrite=<0\|1>` | `0` | Read only when `--to_src_nii=1`. Set to `1` to overwrite existing `.sz` and `.nii.gz` conversion outputs; never overwrites `.dcm` files. |
| `--thread_count=<N>` | hardware threads | Maximum CPU threads used for parallel file renaming and conversion. |
| `--interact` | off | Prompts interactively on standard input for omitted options. |

### Naming tags and sanitization

| Path component | Format | DICOM tags used |
|:--|:--|:--|
| `<patient>` | `<AcqDate>_<Sex><Age>_<PatientName>` | `(0008,0022)` Acquisition Date, `(0010,0040)` Patient's Sex, `(0010,1010)` Patient's Age, `(0010,0010)` Patient's Name (`_` when a tag is absent) |
| `<sequence>` | `<SeriesNum>_<SeriesDescription>` | `(0020,0011)` Series Number (1-digit numbers zero-padded to 2 digits, e.g. `04`), `(0008,103E)` Series Description |
| `<image>` | `<SeriesDescription>_i<InstanceNum>.dcm` | `(0008,103E)` Series Description, `(0020,0013)` Instance Number (up to 32 characters) |

Every character outside `A-Z`, `a-z`, `0-9`, and `.` in `<patient>`, `<sequence>`, and `<image>` is replaced with `_` (empty, `.`, or `..` components become `unknown`).

## Important

- **Moves files in place:** `--action=ren` moves raw DICOM files using `std::filesystem::rename` rather than copying them. Back up raw archives first, and keep `--source` and `--output` on the same filesystem/volume (cross-drive `rename` fails).
- **Safe re-runs vs. `.dcm` collisions:** Re-running `--action=ren` on an already-organized directory (`source == destination`) leaves matching `.dcm` files untouched and still runs `--to_src_nii=1` on each `<output>/<patient>` folder. If two distinct source files resolve to the same destination `.dcm` path (for example, duplicate `(0020,0013)` Instance Numbers in one series), DSI Studio logs `destination already exists`, leaves the colliding file unmoved, and returns exit code `1`.
- **`--overwrite` only affects `.sz` / `.nii.gz`:** Passing `--overwrite=1` without `--to_src_nii=1` has no effect and triggers an unused-option warning.
- **Do not use `--source=*` or `--loop` for folders:** DSI Studio's wildcard file matcher expands regular files only, not directories. Pass the parent directory directly to `--source`, or use an OS shell loop across directories as shown above.

