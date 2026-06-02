## 진행상황 메모
- shoulder yaw 축 오류: STL파일 문제로 발견됨, 어셈블리를 처음부터 다시 하고, export할 때 coarse 가 아니라 fine으로 다시 urdf export 하니 기존 urdf에서도 축 틀어짐 문제가 해결됨
- RAVEN_abs.urdf에서 abs는 절대경로 의미, 리눅스에서 Rviz2의 launch 를 위한 파일

## 할일
- 질량 오버라이딩
- urdf 버전들 정리 필요 -> 비교후 불필요한 파일 삭제요망