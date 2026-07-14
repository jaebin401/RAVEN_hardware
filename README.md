# RAVEN — Hardware

> **RAVEN** — Robotic Arm for Venturing into Engineering by uNdergraduate student
> 3자유도 로봇 매니퓰레이터의 기구 설계 · CAD · URDF 저장소

준직접구동(QDD) 액추에이터 기반 3-DOF 로봇 팔 **RAVEN**의 *하드웨어* 리포지토리입니다.
SolidWorks 어셈블리, 3D 프린팅/구매 파트, 그리고 시뮬레이션·제어용 URDF를 관리합니다.


## 사양

| 항목 | 내용 |
|---|---|
| 자유도 | 3-DOF (revolute × 3) |
| 액추에이터 | Robstride RS02 × 3 (QDD, 피크 17 N·m, 48 V) |
| 프레임 | 30×30 mm 카본 파이버 각관(2 mm 두께) + 3D 프린팅 마운트 |
| 상완(UpperArm) | 카본 튜브 160 mm — `RVN_BUY_UpperArm_CarbonTube160` |
| 전완(ForeArm) | 카본 튜브 250 mm — `RVN_BUY_ForeArm_CarbonTube250` |

## 운동학 구조

`urdf/urdf/RAVEN.urdf` (sw2urdf 추출) 기준:

| 조인트 | Parent → Child | 회전축 | 범위 | 동작 |
|---|---|---|---|---|
| `shoulder_yaw` | `base_link` → `shoulder_link` | Z (−1) | ±180° | 베이스 요(yaw) |
| `upperArm_pitch` | `shoulder_link` → `upperArm_link` | Y (+1) | 0 ~ 180° | 숄더 피치 |
| `foreArm_pitch` | `upperArm_link` → `foreArm_link` | Y (−1) | 0 ~ 180° | 엘보 피치 |

- 조인트 오프셋: `base → shoulder` z = 120.5 mm, `upperArm → foreArm` x = 221.25 mm
- `effort` / `velocity` 한계값은 미입력(0) 상태 — RS02 사양 반영 예정

## 디렉토리 구조

```
RAVEN_hardware/
├── cad/
│   ├── Link STEP files/             # 링크 단위 STEP (sw2urdf 추출용 중립 포맷)
│   │   ├── base_link.step
│   │   ├── Shoulder_link.step
│   │   ├── UpperArm_link.step
│   │   └── ForeArm_link.step
│   └── part files/                  # 개별 파트 (.3mf, 3D 프린팅용)
│       ├── 000_RVN_3DP_Base_MotorMount_v1
│       ├── 010_RVN_3DP_Shoulder_MotorMount_v1
│       ├── 011_RVN_3DP_Shoulder_Drive_v1
│       ├── 012_RVN_3DP_Shoulder_InnerShaft_v4
│       ├── 020_RVN_3DP_UpperArm_MotorMount_v2
│       ├── 021_RVN_3DP_UpperArm_Drive_v3
│       ├── 030_RVN_3DP_ForeArm_Drive_v1
│       └── 031_RVN_3DP_ForeArm_EndEffector_v2
├── urdf/
│   ├── meshes/                      # base / shoulder / upperArm / foreArm .STL
│   └── urdf/                        # RAVEN.urdf, RAVEN_assembly_v2.csv
├── images/
└── docs/
    └── part_naming_protocol.md      # 파트·어셈블리 네이밍 규칙
```

## 파트 네이밍

모든 파트·어셈블리는 [`docs/part_naming_protocol.md`](docs/part_naming_protocol.md)의 규칙을 따릅니다.

```
파트:     (No.)_RVN_(Make)_(Link)_(Function)_v(N)
어셈블리:  RVN_(Link)_ASM_v(N)
```

- **No.** — 3자리 0-padding, 링크별 10단위 블록 (`Base` 000–009 / `Shoulder` 010–019 / `UpperArm` 020–029 / `ForeArm` 030–039, 6-DOF 확장 대비 040번대부터 예약)
- **Make** — `3DP`(프린팅) / `BUY`(구매) / `MCH`(가공)
- **Link** — `Base` / `Shoulder` / `UpperArm` / `ForeArm`
- 어셈블리명은 sw2urdf 작업 시 URDF `<link>` 이름과 일치시킴

## URDF 파이프라인

1. Fusion 360 에서 설계, 각 파트 step 파일로 export
2. SolidWorks import, 하위 어셈블리 작성
3. 총 어셈블리 후 **sw2urdf** 익스포터로 링크·조인트 정의 후 URDF 추출
4. `meshes/*.STL` + `RAVEN.urdf` 

## 관련

- 제어·펌웨어(SocketCAN, RS02 MIT 모드 등)는 별도 소프트웨어 리포지토리에서 관리
- 상위 통합 대상: 휴머노이드 로봇 **QUB**

