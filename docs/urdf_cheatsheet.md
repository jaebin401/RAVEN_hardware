<robot>  ───────────────────────── 로봇 전체를 감싸는 최상위 루트
│   name="robot_name"
│
├── <link>  ────────────────────── 강체(rigid body) 하나 = 팔의 한 마디
│   │   name="link_name"
│   │
│   ├── <inertial> ─────────────── 동역학용 질량 특성 (RViz엔 안 쓰임 / 시뮬에 필수)
│   │   ├── <origin xyz rpy/> ──── 무게중심(CoM) 프레임의 위치·자세
│   │   ├── <mass value/> ──────── 질량 [kg]
│   │   └── <inertia .../> ─────── 관성 텐서 [kg·m²] (ixx ixy ixz iyy iyz izz)
│   │
│   ├── <visual> ───────────────── 화면에 보이는 형상 (RViz/시뮬 렌더링)
│   │   ├── <origin xyz rpy/> ──── 시각 형상의 위치·자세 (링크 프레임 기준)
│   │   ├── <geometry>
│   │   │   └── <mesh filename/> ─ STL/DAE 메시 파일 경로
│   │   └── <material name>
│   │       └── <color rgba/> ──── 색상 RGBA (각 0~1)
│   │
│   └── <collision> ───────────── 충돌 검사용 형상 (물리 연산)
│       ├── <origin xyz rpy/>
│       └── <geometry>
│           └── <mesh filename/>
│
└── <joint>  ───────────────────── 두 링크를 잇는 관절 = 자유도(DOF)
    │   name="joint_name"  type="revolute"
    │
    ├── <origin xyz rpy/> ──────── 부모 프레임 기준, 자식(관절) 프레임의 위치·자세
    ├── <parent link="parent_link"/> ── 부모 링크 이름
    ├── <child  link="child_link"/>  ── 자식 링크 이름
    ├── <axis xyz/> ────────────── 회전(또는 직동) 축 단위벡터
    └── <limit lower upper effort velocity/> ── 가동범위·최대토크·최대속도