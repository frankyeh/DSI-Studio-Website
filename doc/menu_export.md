# [Export] Menu

Use **[Export]** to save per-voxel diffusion metrics, fiber orientations, ODFs, attached slices, and template-aligned MNI volumes from the open FIB session, or to apply in-memory bias-field correction. Its entries build dynamically from the loaded FIB file and refresh automatically as volumes are added or removed under **[Slices]**.

## Menu items

Items appear in up to three separator-divided groups depending on the loaded FIB file:

| Menu item | Behavior |
|---|---|
| <var>metric</var><code>...</code> | Saves the selected 3D metric or slice on the FIB voxel grid (mm spacing) as NIfTI (`.nii.gz`, `.nii`) or raw MATLAB `.mat` (default `<fib_stem>_<metric>.nii.gz`). External slices with different grid dimensions are resampled onto the FIB grid using their active registration. |
| <span class="gui-menu-item">fiber...</span> | Exports all resolved per-voxel unit fiber orientation vectors on the FIB grid as a 4D NIfTI volume (`.nii.gz` or `.nii`, default `<fib_stem>_fiber.nii.gz`), ordered by fiber population and `(x, y, z)` components. |
| <span class="gui-menu-item">odfs...</span> | Exports per-voxel half-sphere orientation distribution functions on the FIB grid as a 4D NIfTI volume (`.nii.gz` or `.nii`, default `<fib_stem>_odfs.nii.gz`). Shown only when ODF data was saved in the FIB file during reconstruction. |
| <var>metric</var> <code>in mni...</code> | Warps the scalar map from a native-space FIB into MNI template space and saves a 3D NIfTI volume (`.nii.gz` or `.nii`); omitted for QSDR/template FIBs and directional <span class="gui-menu-item">fiber...</span> / <span class="gui-menu-item">odfs...</span> data. Rename `<fib_stem>_<metric>.nii.gz` (for example, `.mni.nii.gz`) and avoid `.mat`, which saves unwarped data. |
| <span class="gui-menu-item">Correct Bias Field...</span> | Estimates and removes smooth intensity bias across in-memory `iso`, fiber anisotropy (`qa`/`fa`), and restricted diffusion maps immediately without a dialog (shown only when `iso` is present). Changes stay in memory until saved via **[File][Save FIB As...]**, leave DTI maps unchanged, and repeat if clicked again. |

## Dynamic metric entries

Available metric rows depend on the reconstruction method, saved scalar maps, template alignment, and attached slices:

| Menu item | Behavior |
|---|---|
| <span class="gui-menu-item">qa...</span> | Exports quantitative anisotropy as a 3D scalar map, serving as the primary fiber anisotropy volume in multi-fiber GQI and QSDR reconstructions. |
| <span class="gui-menu-item">fa...</span> | Exports primary fractional anisotropy as a 3D scalar map in single-fiber DTI and 2D histology FIB reconstructions. |
| <span class="gui-menu-item">qir...</span> | Exports the isotropy-normalized quantitative anisotropy ratio as a 3D scalar map, automatically computed for multi-fiber FIB files that include an `iso` volume. |
| <span class="gui-menu-item">iso...</span> | Exports the isotropic diffusion component from GQI or QSDR reconstruction as a 3D scalar map; its presence also enables <span class="gui-menu-item">Correct Bias Field...</span>. |
| <span class="gui-menu-item">dti_fa...</span> | Exports diffusion tensor fractional anisotropy (`0`–`1`) as a 3D scalar map when tensor metrics were saved alongside multi-fiber reconstruction. |
| <span class="gui-menu-item">md...</span> | Exports diffusion tensor mean diffusivity (`(λ₁ + λ₂ + λ₃) / 3`, in `10⁻³ mm²/s`) as a 3D scalar map when saved during reconstruction. |
| <span class="gui-menu-item">ad...</span> | Exports axial diffusivity (principal tensor eigenvalue `λ₁`, in `10⁻³ mm²/s`) as a 3D scalar map when saved during reconstruction. |
| <span class="gui-menu-item">rd...</span> | Exports radial diffusivity (mean perpendicular diffusivity `(λ₂ + λ₃) / 2`, in `10⁻³ mm²/s`) as a 3D scalar map when saved during reconstruction. |
| <span class="gui-menu-item">rd1...</span> | Exports the second diffusion tensor eigenvalue diffusivity (`λ₂`, in `10⁻³ mm²/s`) as a 3D scalar map when saved during reconstruction. |
| <span class="gui-menu-item">rd2...</span> | Exports the third diffusion tensor eigenvalue diffusivity (`λ₃`, in `10⁻³ mm²/s`) as a 3D scalar map when saved during reconstruction. |
| <span class="gui-menu-item">gfa...</span> | Exports generalized fractional anisotropy computed from the orientation distribution function as a 3D scalar map when saved during reconstruction. |
| <span class="gui-menu-item">rdi...</span> | Exports the restricted diffusion imaging map as a 3D scalar volume when saved during GQI or QSDR reconstruction. |
| <span class="gui-menu-item">nrdi02L...</span> | Exports the non-restricted diffusion imaging map at sampling length `0.2` (along with any additional `nrdi*L...` sampling-length maps saved during reconstruction). |
| <span class="gui-menu-item">ha...</span> | Exports fiber helix angle (in degrees) as a 3D scalar map when saved during tensor reconstruction. |
| <span class="gui-menu-item">txx...</span> <span class="gui-menu-item">txy...</span> <span class="gui-menu-item">txz...</span> <span class="gui-menu-item">tyy...</span> <span class="gui-menu-item">tyz...</span> <span class="gui-menu-item">tzz...</span> | Exports individual symmetric diffusion tensor components (`mm²/s`) as 3D maps when tensor output was enabled during reconstruction. |
| <span class="gui-menu-item">hraw...</span> | Exports raw histology image intensity as a scalar volume when saved in histology FIB reconstructions. |
| <span class="gui-menu-item">vol...</span> | Exports the 3D relative tissue volume (Jacobian determinant) map normalized around `1.0` relative to the template. Included directly in QSDR FIBs and available on native-space FIBs once template normalization has been computed. |
| <span class="gui-menu-item">color...</span> | Exports the stored directional color map on the FIB voxel grid as a 24-bit RGB 3D NIfTI volume (`.nii.gz` or `.nii`; `.mat` output is not supported). |
| <span class="gui-menu-item">t1w_template...</span> <span class="gui-menu-item">t2w_template...</span> <span class="gui-menu-item">wm_template...</span> | Exports built-in anatomical or white-matter template volumes attached to template-aligned FIB sessions onto the FIB diffusion grid. |
| <var>slice_name</var><code>...</code> | Exports auto-loaded companion BIDS images, embedded modalities, or external slices added under **[Slices]** onto the FIB diffusion grid. Use **[Slices][Save Slices as...]** to save an inserted slice on its own native voxel grid instead. |

## Coordinate, registration, and bias-field cautions

Wait for background slice registration to finish before exporting external volumes added under **[Slices]**, and export custom slices to FIB space first (or save them on their native grid via **[Slices][Save Slices as...]**) rather than warping unaligned custom grids directly with <var>metric</var> <code>in mni...</code>. Give MNI exports a `.mni.nii.gz` suffix so they do not overwrite native maps and are recognized automatically in [Differential Tractography](/doc/gui_t3_dt.html).

## Related exports in other menus

Other Fiber Tracking exports live in their respective menus: modified `.fz` files and workspaces in [File](/doc/menu_file.html), tract bundles, density maps, and connectivity tables in [Tracts](/doc/menu_tracts.html), ROIs and voxel tables in [Regions](/doc/menu_regions.html) and [Regions Misc](/doc/menu_regions_misc.html), and native-grid slices or DICOMs in [Slices](/doc/menu_slices.html). Stereotactic `.dv.csv` trajectories are saved from [Devices](/doc/menu_devices.html), screenshots, videos, and 3D `.obj`/`.glb` meshes from [Screen](/doc/menu_screen.html), and multi-subject batch exports in [Export Data](/doc/cli_exp.html).
