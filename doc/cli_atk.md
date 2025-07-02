# Automatic Fiber Tracking

> Use `--action=atk` to initiate automatic fiber tracking.

Automatic fiber tracking maps individual bundles using deterministic fiber tracking and tract recognition. The implementation is detailed in Yeh, Fang-Cheng. "Shape analysis of the human association pathways." Neuroimage 223 (2020): 117329.

## Examples

**1. Run fiber tracking on all fib.gz files to map all association pathways:**
```bash
dsi_studio --action=atk --source=*.fz
```

**2. Track specific bundles (e.g., corticospinal tracts and optic radiation):**
```bash
dsi_studio --action=atk --source=*.fz --track_id=Corticos,Optic
```

**3. Output tracts in the template space:**
```bash
dsi_studio --action=atk --source=*.fz --template_track
```

---

## Core Functions

| **Parameter**          | **Default**                                                                 | **Description**                                                                 |
|-------------------------|-----------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| `source`               |                                                                             | Specify fib.gz files for automatic bundle tracking.                            |
| `track_id`             | `Arcuate,Cingulum,Aslant,InferiorFronto,InferiorLongitudinal,SuperiorLongitudinal,Uncinate,Fornix,Corticos,ThalamicR,Optic,Lemniscus,Reticular,Corpus` | Specify the bundle names, separated by commas. Partial names are accepted. Example: `--track_id=arcuate` tracks left and right arcuate fasciculus. |
| `template`             | `0`                                                                         | Specify the template for tracking. Supported templates:<br>`0`: ICBM152<br>`1`: CIVM_mouse<br>`2`: Neonate<br>`3`: INDI_rhesus<br>`4`: Pitt_Marmoset<br>`5`: WHS_SD_rat |
| `tolerance`            | `22,26,30`                                                                  | Set tolerance for bundle recognition (in mm). Larger values may include more variation but increase false positives. |
| `track_voxel_ratio`    | `2.0`                                                                       | Set the track-to-voxel ratio for streamline count. Higher values improve mapping but increase computation time. |
| `check_ending`         | `1` (default: `0` for cingulum)                                             | Removes tracts terminating in high anisotropy locations.                       |
| `thread_count`         | Hardware max                                                               | Specify the number of CPU cores used for computation.                          |
| `yield_rate`           | `0.00001`                                                                  | Terminates tracking early if no new fibers are generated.                      |
| `default_mask`         | `0`                                                                        | Specify whether to use the default mask.                                       |
| `overwrite`            | `0`                                                                        | Specify whether to overwrite existing files.                                   |
| `trk_format`           | `tt.gz`                                                                    | Set the output format for tractography files. Supported formats: `.tt.gz`, `.trk`, `.trk.gz`, `.tck`, `.txt`, `.mat`, `.nii`, `.nii.gz`. |

---

## Optional Functions

The majority of parameters used in --action=trk are also supported.

- **Fiber Tracking Parameters**: The following parameters are supported: 
  - `--otsu_threshold`
  - `--fa_threshold`
  - `--turning_angle`
  - `--step_size`
  - `--smoothing`
  - `--tip_iteration`
  - 
- **Length Constraints**:
  Parameters like `--min_length` and `--max_length` are not supported, as length constraints are automatically determined from the atlas.


- **Post-Tracking Routines**: The following parameters are also supported:

  - `--connectivity`
  - `--export`
   
