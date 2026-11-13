# Export Data

Use `--action=exp` to export scalar metrics, fiber directions, or ODF volumes from `.fz` / `.fib.gz` files as compressed NIfTI (`.nii.gz`), convert tractography directly between `.trk.gz` and `.tt.gz`, or generate a demographic-matched volume from a `.fz` / `.fib.gz` connectometry database.

> **Source (`.sz` / `.src.gz`) exports:** `--action=exp` does not accept `.sz` or `.src.gz` files. To export 4D DWI NIfTI, `b0`, DWI sum, `b_table.txt`, `.bval`, or `.bvec` from a source file, use [Step T2 Reconstruction (`--action=rec`)](/doc/cli_t2.html) (`--save_nii=out.nii.gz` exports 4D NIfTI + `.bval` + `.bvec` without reconstruction, or chain `--cmd=save_nifti=...+save_b_table=...+save_bval=...+save_bvec=...` using `+`).

## Export Metrics from FIB Files

Export selected diffusion metrics from a single FIB file (writes `subject.fz.<metric>.nii.gz` for each token):

```bash
dsi_studio --action=exp --source=subject.fz --export=qa,iso,dti_fa,md,ad,rd
```

Batch-export `qa` and `iso` from native-space FIB files and warp the exported NIfTI volumes to MNI template space:

```bash
dsi_studio --action=exp --source=*.fz --export=qa,iso --export_to_mni
```

Export 4D fiber orientation vectors and half-sphere ODF volumes in FIB voxel space:

```bash
dsi_studio --action=exp --source=subject.fz --export=dirs,dir0,odfs
```

### Parameters

| Parameter | Required | Default | Description |
|:--|:--|:--|:--|
| `--source` | Yes | — | Input `.fz` or `.fib.gz` file, or a wildcard pattern (`*.fz`) for batch execution. |
| `--export` | Yes | — | Comma-separated export tokens (no spaces). Each token writes `<source>.<token>.nii.gz` (`--output` is ignored in this mode). |
| `--export_to_mni` | No | Disabled | Presence flag (omit to keep FIB space; do not pass `--export_to_mni=0`). Warps native-space scalar slice maps to template/MNI space, reusing `<source>.<template>.mz` when newer than the FIB or computing and saving it. Omit for QSDR FIBs already reconstructed in MNI space, and note that `dirs`, `fiber`, `dir0`–`dirN`, `odfs`, and `color` always export in FIB space. |
| `--bias_field_correction` | No | `1` | `1` or `0`. When `1` and the FIB report does not already record bias-field correction, applies `iso`-based bias-field correction to `iso`, fiber anisotropy (`qa`/`fa`), and `rdi` on load before exporting. Pass `--bias_field_correction=0` to export uncorrected stored values. |
| `--other_slices` | No | — | Comma-separated external NIfTI/image path(s) registered into the FIB session; each added volume can be exported resampled to FIB space (or MNI space with `--export_to_mni`) using its filename stem in `--export`. |
| `--subject_demo` | No | — | Demographic CSV/TSV file or inline text loaded into session metadata only when `--other_slices` is specified on a non-database FIB. |
| `--loop` | No | `--source` if `*` present | Explicit wildcard file pattern for batch execution when `--source` does not itself specify the loop pattern. |
| `--continue_on_error` | No | Disabled | Presence flag. Continues batch wildcard processing when an individual file fails instead of exiting immediately. |
| `--thread_count` | No | System threads | Maximum worker thread count used during MNI registration, resampling, and compression. |

> **Alias in `--action=ana`:** Running `--action=ana --source=subject.fz --export=...` without `--tract`, `--region`, `--atlas`, or `--info` forwards directly to `--action=exp`.

### `--export` Tokens

Metric availability depends on the reconstruction method and which optional outputs were saved during reconstruction (`--other_output` in `--action=rec`; default GQI/QSDR reconstruction stores `dti_fa`, `md`, `rd`, and `rdi` alongside `qa` and `iso`).

| Category | Token(s) | Output Shape & Meaning |
|:--|:--|:--|
| Core fiber & isotropic | `qa` | 3D scalar: primary fiber quantitative anisotropy (multi-fiber GQI/QSDR FIBs). |
| Core fiber & isotropic | `fa` | 3D scalar: fractional anisotropy in single-fiber DTI/histology FIBs (`fa0`, `fa1`, ... are not slice tokens; use `dti_fa` for tensor FA in GQI/QSDR FIBs). |
| Core fiber & isotropic | `iso` | 3D scalar: isotropic diffusion component. |
| Derived FIB maps | `qir` | 3D scalar: isotropy-normalized QA ratio, computed on non-database multi-fiber FIBs containing `iso`. |
| Derived FIB maps | `vol` | 3D scalar: Jacobian determinant / relative volume map (stored in QSDR FIBs, or computed via MNI registration on native-space FIBs). |
| DTI tensor metrics | `dti_fa`, `md`, `ad`, `rd`, `rd1`, `rd2` | 3D scalar: tensor fractional anisotropy and mean, axial, radial, 2nd-eigenvalue, and 3rd-eigenvalue diffusivities (10⁻³ mm²/s). |
| DTI helix & tensor | `ha`, `txx`, `txy`, `txz`, `tyy`, `tyz`, `tzz` | 3D scalar: helix angle (degrees) and symmetric diffusion tensor components (requires `helix` / `tensor` in reconstruction `--other_output`). |
| GQI / RDI / histology | `gfa`, `rdi`, `nrdi02L`, `nrdi04L`, ..., `hraw` | 3D scalar: generalized FA (`gfa`), restricted diffusion imaging (`rdi`), multi-shell non-restricted diffusion imaging at sampling length `0.2`, `0.4`, ... (`nrdi*L`), and single-image histology intensity (`hraw`). |
| Fiber vectors (FIB space) | `dirs` (or `fiber`) | 4D float `(X, Y, Z, 3 * num_fiber)`: unit `(x, y, z)` orientation vectors for all resolved fiber populations. |
| Single-fiber vector (FIB space) | `dir0`..`dirN` | 4D float `(X, Y, Z, 3)`: unit `(x, y, z)` vector for fiber index `N` (`0 <= N < num_fiber`, single digit) at voxels with positive anisotropy. |
| ODFs (FIB space) | `odfs` | 4D float `(X, Y, Z, half_odf_size)`: half-sphere ODF values (requires `odf` saved during reconstruction). |
| Color map (FIB space) | `color` | 3D RGB volume when a `color` slice is present in the FIB. |
| Embedded / custom slices | `<slice_name>` | 3D scalar: any modality embedded via `--other_image` during reconstruction or attached via `--other_slices` (matched by exact slice name, with substring fallback). |

## Convert Tractography Files

Convert compressed TrackVis `.trk.gz` to DSI Studio `.tt.gz`:

```bash
dsi_studio --action=exp --source=af.trk.gz --output=af.tt.gz
```

Convert `.tt.gz` to `.trk.gz`:

```bash
dsi_studio --action=exp --source=af.tt.gz --output=af.trk.gz
```

Batch-convert all `.tt.gz` files in a directory to `.trk.gz`:

```bash
dsi_studio --action=exp --source=*.tt.gz --output=*.trk.gz
```

| Parameter | Required | Description |
|:--|:--|:--|
| `--source` | Yes | Input `.trk.gz` or `.tt.gz` file (or wildcard pattern such as `*.tt.gz`). |
| `--output` | Yes | Output `.tt.gz` (when `--source` ends with `.trk.gz`) or `.trk.gz` (when `--source` ends with `.tt.gz`). Use `*` to mirror `--source` stems in batch mode. |
| `--continue_on_error` | No | Presence flag. Continues wildcard batch conversion if one tract file fails. |

`--action=exp` performs header-direct conversion strictly between compressed `.trk.gz` and `.tt.gz` without loading a FIB file. To convert uncompressed `.trk`, MRtrix `.tck`, or `.trx` tractography, use [Tractography Analysis (`--action=ana`)](/doc/cli_ana.html) with `--source=<fib.fz> --tract=<input> --output=<output>`.

## Demographic-Matched Image Export

For a `.fz` or `.fib.gz` connectometry database containing embedded subject demographics, `--match` fits a voxelwise multiple linear regression across database subjects on the database metric and exports the non-negative predicted volume (`max(0, predicted)`) for the target covariates:

```bash
dsi_studio --action=exp --source=database.db.fz --match=45,1 --output=matched_image.nii.gz
```

| Parameter | Required | Default | Description |
|:--|:--|:--|:--|
| `--source` | Yes | — | Input connectometry database with a `.fz` or `.fib.gz` suffix (for example, `database.db.fz` or `database.db.fib.gz`) that embeds subject volumes and `demo` demographics. `.dz` suffixes are rejected by `--action=exp`. |
| `--match` | Yes | — | Comma- or space-separated numeric covariate values matching the exact count and column order of demographic features in the database (for example, `45,1` for a two-column `age,sex` table where two-level text labels are encoded `0`/`1`). Do **not** pass a filename. |
| `--output` | No | `<source>.matched.nii.gz` | Output NIfTI file path. |
| `--bias_field_correction` | No | `1` | `1` or `0` passed through FIB loading (`cmd_load_fib`) prior to regression export. |

