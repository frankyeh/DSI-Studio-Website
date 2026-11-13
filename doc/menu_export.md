# [Export] Menu

Use **[Export]** to save per-voxel diffusion metrics, fiber orientations, ODFs, attached slices, and template-aligned MNI volumes from the open FIB session, or to apply in-memory bias-field correction. Its entries build dynamically from the loaded FIB file and refresh automatically as volumes are added or removed under **[Slices]**.

## Menu items

Items appear in up to three separator-divided groups depending on the loaded FIB file:

| Menu item | Behavior |
|---|---|
| **`<metric>...`** | Saves the selected 3D metric or slice on the FIB voxel grid (mm spacing) as NIfTI (`.nii.gz`, `.nii`) or raw MATLAB `.mat` (default `<fib_stem>_<metric>.nii.gz`). External slices with different grid dimensions are resampled onto the FIB grid using their active registration. |
| **fiber...** | Exports all resolved per-voxel unit fiber orientation vectors on the FIB grid as a 4D NIfTI volume (`.nii.gz` or `.nii`, default `<fib_stem>_fiber.nii.gz`), ordered by fiber population and `(x, y, z)` components. |
| **odfs...** | Exports per-voxel half-sphere orientation distribution functions on the FIB grid as a 4D NIfTI volume (`.nii.gz` or `.nii`, default `<fib_stem>_odfs.nii.gz`). Shown only when ODF data was saved in the FIB file during reconstruction. |
| **`<metric> in mni...`** | Warps the scalar map from a native-space FIB into MNI template space and saves a 3D NIfTI volume (`.nii.gz` or `.nii`); omitted for QSDR/template FIBs and directional **fiber...** / **odfs...** data. Rename `<fib_stem>_<metric>.nii.gz` (for example, `.mni.nii.gz`) and avoid `.mat`, which saves unwarped data. |
| **Correct Bias Field...** | Estimates and removes smooth intensity bias across in-memory `iso`, fiber anisotropy (`qa`/`fa`), and restricted diffusion maps immediately without a dialog (shown only when `iso` is present). Changes stay in memory until saved via **File › Save FIB As...**, leave DTI maps unchanged, and repeat if clicked again. |

## Dynamic metric entries

Available metric rows depend on the reconstruction method, saved scalar maps, template alignment, and attached slices:

| Menu item | Behavior |
|---|---|
| **qa...** / **fa...** | Exports the primary fiber anisotropy volume as a 3D map: quantitative anisotropy (**qa...**) for multi-fiber GQI/QSDR reconstructions, or fractional anisotropy (**fa...**) for single-fiber DTI and 2D histology FIB files. |
| **qir...** | Exports the isotropy-normalized quantitative anisotropy ratio as a 3D scalar map, automatically computed for multi-fiber FIB files that include an `iso` volume. |
| **iso...** | Exports the isotropic diffusion component from GQI or QSDR reconstruction as a 3D scalar map; its presence also enables **Correct Bias Field...**. |
| **dti_fa...**, **md...**, **ad...**, **rd...**, **rd1...**, **rd2...** | Exports diffusion tensor fractional anisotropy (**dti_fa...**) and mean, axial, radial, second-eigenvalue, or third-eigenvalue diffusivities (in `10⁻³ mm²/s`) as 3D maps when saved during reconstruction. |
| **gfa...**, **rdi...**, **nrdi02L...** (and `nrdi*L...`), **ha...**, **txx...**–**tzz...**, **hraw...** | Exports optional 3D maps saved during reconstruction: generalized FA (**gfa...**), restricted and non-restricted diffusion (**rdi...**, **nrdi02L...**), helix angle in degrees (**ha...**), tensor components (**txx...**–**tzz...**), or raw histology intensity (**hraw...**). |
| **vol...** | Exports the 3D relative tissue volume (Jacobian determinant) map normalized around `1.0` relative to the template. Included directly in QSDR FIBs and available on native-space FIBs once template normalization has been computed. |
| **color...** | Exports the stored directional color map on the FIB voxel grid as a 24-bit RGB 3D NIfTI volume (`.nii.gz` or `.nii`; `.mat` output is not supported). |
| **t1w_template...**, **t2w_template...**, **wm_template...**, or **`<slice_name>...`** | Exports attached template volumes, auto-loaded companion BIDS images, embedded modalities, or external slices added under **Slices** onto the FIB diffusion grid. Use **Slices › Save Slices as...** to save an inserted slice on its own native voxel grid instead. |

## Coordinate, registration, and bias-field cautions

Wait for background slice registration to finish before exporting external volumes added under **[Slices]**, and export custom slices to FIB space first (or save them on their native grid via **[Slices][Save Slices as...]**) rather than warping unaligned custom grids directly with **`<metric> in mni...`**. Give MNI exports a `.mni.nii.gz` suffix so they do not overwrite native maps and are recognized automatically in [Differential Tractography](/doc/gui_t3_dt.html).

## Related exports in other menus

Other Fiber Tracking exports live in their respective menus: modified `.fz` files and workspaces in [File](/doc/menu_file.html), tract bundles, density maps, and connectivity tables in [Tracts](/doc/menu_tracts.html), ROIs and voxel tables in [Regions](/doc/menu_regions.html) and [Regions Misc](/doc/menu_regions_misc.html), and native-grid slices or DICOMs in [Slices](/doc/menu_slices.html). Stereotactic `.dv.csv` trajectories are saved from [Devices](/doc/menu_devices.html), screenshots, videos, and 3D `.obj`/`.glb` meshes from [Screen](/doc/menu_screen.html), and multi-subject batch exports in [Export Data](/doc/cli_exp.html).
