# Meshgen UGRID Scripts

This repository provides mesh generators that write ASCII UGRID files and
companion `.mapbc` files. It also includes a converter from ASCII UGRID to
Tecplot ASCII (`.dat`) for all-hex volume meshes.

## 1) Backward-facing step (BFS)

```bash
python3 BFS_meshgen.py bfs_sample_input.json bfs_output.ugrid
```

BFS input parameters:
- `landing_length`
- `nx_landing`
- `x1` (first x-cell at the step corner)
- `y1` (first y-cell at walls)
- `step_height`
- `upper_height`
- `ny_step`
- `delta_z`
- `nz`
- `step_length`
- `nx_step`

BFS spacing behavior:
- Landing x-spacing shrinks toward the step corner (`x=0`) with first size `x1`.
- Step-block x-spacing is symmetric in x about the midpoint of the step block.
- Upper block y-spacing uses `y1` at the top wall and grows toward centerline `y=0`.
- Lower step block y-spacing uses `y1` at the bottom wall and grows toward the step corner (`y=0`).

## 2) Flat plate (FP)

```bash
python3 FP_meshgen.py fp_sample_input.json fp_output.ugrid
```

FP BC IDs:
- 1 inlet
- 2 outlet
- 3 inviscid wall on landing
- 4 far field
- 5 symmetry sides (z)
- 6 viscous wall on plate

## 3) Smooth seal (SS)

```bash
python3 ss_meshgen.py ss_sample_input.json ss_output.ugrid
```

SS input parameters:
- `shaft_radius`
- `nx_landing`
- `x_landing`
- `x_seal`
- `nx_seal`
- `x1`
- `y_clearance`
- `ny_clearance` (even)
- `r_stator`
- `ny_landing` (even)
- `y1`
- `n_theta`

SS block layout:
- Inlet clearance block
- Inlet outer landing block
- Seal clearance block
- Outlet clearance block
- Outlet outer landing block

SS spacing behavior:
- Inlet x-spacing shrinks toward `x1` at the right side.
- Seal x-spacing is symmetric and shrinks to `x1` at both ends.
- Outlet x-spacing starts at `x1` on the left and grows to the outlet.
- Clearance and outer-landing radial spacing are reflected so `y1` is used at both sides.
- Blocks are rotated in theta with periodic connectivity in k (no theta/z BC).

SS BC IDs:
- 1 rotating wall on shaft
- 2 rotating wall on stator
- 3 inlet
- 4 outlet

## 4) Convert UGRID to Tecplot

```bash
python3 ugrid_to_tecplot.py bfs_output.ugrid bfs_output.dat
```
