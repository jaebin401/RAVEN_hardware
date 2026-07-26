# RAVEN — Hardware

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-CERN--OHL--S--2.0-blue" alt="License: CERN-OHL-S 2.0"></a>
  <img src="https://img.shields.io/badge/CAD-AutoDesk%20Fusion-orange" alt="Fusion">
</p>

<p align="center">
  🇰🇷 <a href="https://github.com/jaebin401/RAVEN_hardware/tree/main_KR"><strong>한국어 README는 여기서 확인하세요</strong></a>
</p>

> **RAVEN** — Robotic Arm for Venturing into Engineering by uNdergraduate student <br>
> Mechanical design · CAD · URDF repository for a 3-DOF robotic manipulator

<p align="center">
  <img src="images/RAVEN_thumbnail.png" width="440" alt="RAVEN" />
</p>

This is the *hardware* repository for **RAVEN**, a 3-DOF robotic arm built on quasi-direct-drive (QDD) actuators.<br>
It manages Fusion 360 (f3d) modeling files, 3D-printing part files (3mf), the BOM, and the URDF used for simulation and control.

> This repository is the hardware part of the **RAVEN** project. The overall project overview and its sub-repositories are managed in the umbrella repository → **[RAVEN](https://github.com/jaebin401/RAVEN)**


## Specifications

| Item | Details |
|---|---|
| Degrees of freedom | 3-DOF (revolute × 3) |
| Actuators | Robstride RS02 × 3 (QDD, peak 17 N·m, 48 V) |
| Frame | 2020 · 3030 aluminum extrusion + 3D-printed mounts |
| Upper arm | 3030 aluminum extrusion, 160 mm |
| Forearm | 2020 aluminum extrusion, 205 mm (250 mm overall link length) |

## Kinematic Structure

Based on `urdf/urdf/RAVEN.urdf` (exported via sw2urdf):

| Joint | Parent → Child | Axis | Range | Motion |
|---|---|---|---|---|
| `shoulder_Joint` | `base_link` → `shoulder_link` | +Z `(0 0 1)` | −3.14 ~ 3.14 rad | Base yaw |
| `upperArm_Joint` | `shoulder_link` → `upperArm_link` | +Y `(0 1 0)` | 0 ~ 3.14 rad | Shoulder pitch |
| `foreArm_Joint` | `upperArm_link` → `foreArm_link` | −Y `(0 -1 0)` | 0 ~ 3.14 rad | Elbow pitch |

- Joint offsets (origin, m): `shoulder` (0, 0, 0.0504) · `upperArm` (0, 0.027, 0.07) · `foreArm` (−0.225, ≈0, ≈0)
- `effort` / `velocity` limits are currently unset (0) in the URDF — to be filled in from RS02 specs (rated 6 / peak 17 N·m, no-load 410 rpm)

## Directory Structure

```
RAVEN_hardware/
├── cad/
│   ├── Link files - .STEP/      # Per-link STEP files (neutral format for sw2urdf)
│   ├── modeling files - .f3d/   # Native Fusion 360 files (upcoming)
│   └── part files - .3mf/       # Individual parts (for 3D printing)
├── urdf/
│   ├── meshes/                  # Per-link STL meshes
│   └── urdf/                    # RAVEN.urdf + export CSV
├── docs/                        # Part naming protocol, ADRs (upcoming)
└── images/
    ├── link image/              # Link render images
    └── part image/              # Part render images
```

## Part Naming

All parts and assemblies follow the rules defined in [`docs/part_naming_protocol.md`](docs/part_naming_protocol.md).

```
Part:      (No.)_RVN_(Make)_(Link)_(Function)_v(N)
Assembly:  RVN_(Link)_ASM_v(N)
```

- **No.** — 3-digit zero-padded, 10-number blocks per link (`Base` 000–009 / `Shoulder` 010–019 / `UpperArm` 020–029 / `ForeArm` 030–039, 040+ reserved for the future 6-DOF expansion)
- **Make** — `3DP` (printed) / `BUY` (purchased) / `MCH` (machined)
- **Link** — `Base` / `Shoulder` / `UpperArm` / `ForeArm`
- Assembly names are matched to the URDF `<link>` names during the sw2urdf workflow

## Related

- Project umbrella repository: **[jaebin401/RAVEN](https://github.com/jaebin401/RAVEN)**
- Korean version of this README: **[main_KR branch](https://github.com/jaebin401/RAVEN_hardware/tree/main_KR)**
- Control/firmware (SocketCAN, RS02 MIT mode, etc.) is managed in a separate software repository
- Intended for eventual integration into the humanoid robot **QUB**

## License

| Scope | License |
|---|---|
| Design files (CAD `.f3d`, STEP, STL, URDF) | [CERN-OHL-S 2.0](LICENSE) |
| Documentation · images (README, naming protocol, ADRs, `images/*`) | [CC BY 4.0](LICENSE-CC-BY-4.0.txt) |

Copyright © 2026 Jaebin Ahn

Design files are licensed under **CERN-OHL-S 2.0**, a strongly reciprocal open hardware license.
If you manufacture, distribute, or sell hardware based on this design, any modified or improved design sources must be released under the same license.
For artifacts such as CAD binaries where a license notice cannot be embedded in the file itself, this document serves as the license notice.

## Author

**Jaebin Ahn (jaebin401)**  
Undergraduate, Mechanical Engineering (Software minor)  
Apple Developer Academy @ POSTECH

Goal: robotics researcher. Planning to pursue graduate studies in the field after undergrad.
- GitHub: [@jaebin401](https://github.com/jaebin401)
- Instagram: [@study_4_machine](https://www.instagram.com/study_4_machine/)
- LinkedIn: [Jaebin Ahn](https://www.linkedin.com/in/jaebin-272ba8366)
