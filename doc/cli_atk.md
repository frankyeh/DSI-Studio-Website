# Automatic Fiber Tracking

> Use `--action=atk` to initiate automatic fiber tracking.

Automatic fiber tracking maps individual bundles using deterministic fiber tracking and tract recognition. The implementation is detailed in Yeh, Fang-Cheng. "Shape analysis of the human association pathways." Neuroimage 223 (2020): 117329.

## Examples

**1. Run fiber tracking on all FIB files to map standard pathways:**
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

**4. Calculate overlap of each completed named bundle with a built-in atlas:**
```bash
dsi_studio --action=atk \
  --source=subject.fz \
  --track_id=Corticos \
  --overlap=HCP-MMP
```

For each completed named bundle, DSI Studio voxelizes the tract and writes an overlap-statistics table containing only nonempty atlas labels. The file is saved beside the normal bundle output as:

```text
<bundle-tract-file>.overlap.txt
```

For example, an AutoTrack bundle path such as:

```text
subject.ProjectionBrainstem_CorticospinalTractL.tt.gz
```

produces:

```text
subject.ProjectionBrainstem_CorticospinalTractL.tt.gz.overlap.txt
```

---

## Core Functions

| **Parameter**          | **Default** | **Description** |
|-------------------------|-------------|-----------------|
| `source`               | *(required)* | Specify `.fz` or legacy `.fib.gz` files for automatic bundle tracking. |
| `track_id`             | Atlas-defined standard groups | Bundle names separated by commas. Partial names are accepted and may match multiple bundles. For a reproducible named-bundle workflow, use the current atlas entry; choose the parent entry for the whole tract family and a child only for a specific subdivision. |
| `template`             | Current/default template | Select the tractography template. Use the template list provided by the current DSI Studio version rather than relying on fixed numeric names. |
| `tolerance`            | FIB/template dependent | Bundle-recognition tolerance in mm. If omitted, DSI Studio derives a tolerance series from the selected FIB/template. |
| `track_voxel_ratio`    | FIB dependent | Track-to-voxel ratio. If omitted, DSI Studio uses the value derived from the FIB. |
| `check_ending`         | `1` | Apply ending constraints used by automatic tract recognition. |
| `yield_rate`           | `0.00001` | Restart/stop an unproductive tracking attempt when tract yield is extremely low. |
| `overwrite`            | `0` | Overwrite existing results when set to `1`. |
| `trk_format`           | `tt.gz` | Output tractography format. |

---

## Optional Functions

The majority of parameters used in `--action=trk` are also supported.

- **Fiber Tracking Parameters**:
  - `--otsu_threshold`
  - `--fa_threshold`
  - `--turning_angle`
  - `--step_size`
  - `--smoothing`
  - `--tip_iteration`

- **Length Constraints**:
  Parameters such as `--min_length` and `--max_length` are not used because AutoTrack derives tract-length constraints from the atlas.

- **Post-Tracking Routines**:
  - `--overlap=<atlas-name>`: calculate voxelized bundle overlap with every nonempty label of the exact built-in atlas name and save `<bundle-tract-file>.overlap.txt`.
  - `--connectivity`
  - `--export`

## AutoTrack guidance

Standard named AutoTrack entries already include atlas-defined anatomical constraints. Do not add extra ROI, ROA, End, Seed, NotEnd, Limiting, or Terminative regions by default; add an additional region only when a specific anatomical question requires it.

Topology-informed pruning (TIP) is intended for a coherent bundle rather than generic whole-brain cleanup. When a named bundle reaches roughly **5,000–10,000 or more tracts before pruning**, **3–4 TIP iterations** are a practical starting point. If the bundle is too sparse, inspect acquisition, reconstruction, tracking yield, and tolerance before applying aggressive pruning.

Streamline count reflects tractography sampling. It is not an axon count or a direct measure of biological connection strength.
