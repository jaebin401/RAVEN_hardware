# RAVEN Part Naming Protocol

**Project:** RAVEN – Robotic Arm for Venturing into Engineering by uNdergraduate student
**Version:** 1.1
**Date:** 2026-05-23

---

## 1. 기본 구조

```
(No.)_RVN_(Make)_(Link)_(Function)_v(N)
```

| 필드 | 설명 | 예시 |
|---|---|---|
| `No.` | 파트 고유 번호 (3자리) | `001` |
| `RVN` | 프로젝트 코드 (고정) | `RVN` |
| `Make` | 제조 방식 | `3DP` |
| `Link` | 파트가 속한 링크 | `Base` |
| `Function` | 파트의 기능 | `Frame` |
| `v(N)` | 설계 버전 | `v1` |

**예시:**
```
001_RVN_3DP_Base_Frame_v1
011_RVN_BUY_Shoulder_Bearing_v1
021_RVN_3DP_UpperArm_Link_v1
034_RVN_3DP_ForeArm_EEMount_v1
```

---

## 2. 표기 규칙

- 구분자는 언더스코어(`_`)만 사용, 스페이스·하이픈 금지
- Make는 **대문자** (예: `3DP`, `BUY`)
- Link, Function은 **PascalCase** (예: `MotorMount`, `BearingHousing`)
- 버전은 소문자 `v` + 숫자 (예: `v1`, `v2`)
- 파트 번호는 **3자리** 0-padding (예: `001`, `011`)

---

## 3. Make 목록

| Make | 설명 |
|---|---|
| `3DP` | 3D 프린팅 파트 |
| `BUY` | 구매 파트 (Misumi 등) |
| `MCH` | 가공 파트 (CNC 등, 추후 확장 시) |

---

## 4. Link 목록

| Link | 설명 |
|---|---|
| `Base` | 고정부. 바닥에 고정되며 움직이지 않음 |
| `Shoulder` | J1 출력 링크. Base와 UpperArm 사이 |
| `UpperArm` | J2 출력 링크. Shoulder와 ForeArm 사이 |
| `ForeArm` | J3 출력 링크. UpperArm ~ End Effector |

---

## 5. Function 목록

| Function | 설명 |
|---|---|
| `Frame` | 구조를 담당하는 메인 바디 |
| `MotorMount` | 액추에이터(RS02) 고정 브라켓 |
| `Drive` | 액추에이터가 직접 구동할 파트 |
| `BearingHousing` | 베어링 하우징 |
| `ShaftCollar` | 샤프트 고정 칼라 |
| `ShaftOutput` | 출력 샤프트 |
| `OutputLink` | 조인트 출력 링크 |
| `EndCap` | 링크 끝단 캡 |
| `BottomPlate` | 바닥 마운팅 플레이트 |
| `EEMount` | 엔드이펙터 마운팅 플레이트 |
| `EndEffector` | 엔드이펙터 본체 |
| `InnerShaft` | 내부 샤프트 (모터 출력축과 결합되는 내측 축 부품) |
| `CableClamp` | 케이블 고정 클램프 |
| `CablePath` | 케이블 경로 가이드 |
| `Cover` | 외장 보호 커버 |

---

## 6. 조인트 명칭

조인트 자체(베어링, 샤프트 등)는 **출력 측 링크에 귀속**

| 조인트 | 위치 | 귀속 Link |
|---|---|---|
| Base Joint | Base ↔ Shoulder | `Shoulder` |
| Shoulder Joint | Shoulder ↔ UpperArm | `UpperArm` |
| Elbow Joint | UpperArm ↔ ForeArm | `ForeArm` |

---

## 7. 확장 규칙

- **설계 변경:** 버전 번호만 올림 (`v1` → `v2`), 파트 번호 유지
- **파트 추가:** 기존 번호 뒤로 이어서 부여
- **6-DOF 확장 시:** Link 목록에 추가, 기존 명세 수정 없음
- **가공 파트 추가 시:** Make에 `MCH` 추가하여 확장

## 8. 어셈블리 네이밍

### 기본 구조

```
RVN_(Link)_ASM_v(N)
```

파트 네이밍과 달리 어셈블리는 `No.`와 `Make` 필드를 생략한다.  
어셈블리는 여러 Make가 혼합되므로 제조방식 구분이 무의미하며,  
어셈블리 파일은 번호보다 Link명으로 식별하는 것이 직관적이기 때문이다.

### 어셈블리 목록

| 어셈블리명 | 설명 | URDF `<link>` 대응 |
|---|---|---|
| `RVN_Base_ASM_v1` | 베이스 전체 어셈블리 | `base_link` |
| `RVN_Shoulder_ASM_v1` | Shoulder 전체 어셈블리 | `shoulder_link` |
| `RVN_UpperArm_ASM_v1` | UpperArm 전체 어셈블리 | `upperArm_link` |
| `RVN_ForeArm_ASM_v1` | ForeArm 전체 어셈블리 | `foreArm_link` |
| `RVN_FULL_ASM_v1` | 전체 통합 어셈블리 | — |

### 규칙

- 어셈블리명은 sw2urdf 작업 시 URDF `<link>` 이름과 반드시 일치시킨다
- 하위 어셈블리가 필요한 경우 Link명 뒤에 세부 명칭을 추가한다
  ```
  RVN_Elbow_Joint_ASM_v1
  RVN_Shoulder_Pulley_ASM_v1
  ```
- 설계 변경 시 파트와 동일하게 버전 번호만 올린다