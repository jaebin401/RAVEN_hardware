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
├── assembly files/                  # SolidWorks 어셈블리 (.SLDASM)
│   ├── RAVEN_assembly.SLDASM        #   전체 통합 어셈블리
│   ├── RVN_Base_ASM_v1
│   ├── RVN_Shoulder_ASM_v1
│   ├── RVN_UpperArm_ASM_v1
│   └── RVN_ForeArm_ASM_v1
├── part files/                      # 단품 파트 (.step / .SLDPRT)
│   ├── RVN_3DP_*                     #   3D 프린팅 MotorMount · Drive 파트
│   ├── RVN_BUY_RS02.SLDPRT          #   액추에이터
│   └── RVN_BUY_*_CarbonTube*        #   카본 튜브 링크
├── urdf/
│   ├── meshes/                      # base / shoulder / upperArm / foreArm .STL
│   └── urdf/                        # RAVEN.urdf, urdf.csv
└── docs/
    └── part_naming_protocol.md      # 파트·어셈블리 네이밍 규칙
```

## 파트 네이밍

모든 파트·어셈블리는 [`docs/part_naming_protocol.md`](docs/part_naming_protocol.md)의 규칙을 따릅니다.

```
파트:     RVN_(Make)_(Link)_(Function)_v(N)
어셈블리:  RVN_(Link)_ASM_v(N)
```

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

