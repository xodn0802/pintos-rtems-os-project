# PintOS & RTEMS 운영체제 프로젝트

인하대학교 컴퓨터공학과 지능형 임베디드 SW 연구실 학부연구생 프로그램 (2025 하계)
교육용 운영체제 PintOS의 핵심 기능 분석·구현 및 RTEMS 항공위성용 운영체제 연구를 수행했습니다.

## 프로젝트 개요

- **기간**: 2025.06 ~ 2025.08
- **소속**: 지능형 임베디드 SW 연구실 (담당교수: 정진만)
- **사용 환경**: Linux, C, GDB
- **활동 방식**: 매주 분석·구현 결과를 발표 자료로 정리 및 발표

## 주요 수행 내용

### 1. PintOS 구조 분석 및 도식화
- 공식 문서와 GDB 동적 분석을 통해 타이머, 인터럽트 처리, 프로세스, 스케줄링, 메모리 관리 시스템의 설계 구조를 분석하고 도식화
- x86(IA-32)의 가상 주소 공간 분리, 페이지 권한(supervisor/user), Mode Switch 메커니즘을 프레임 단위로 추적·발표

### 2. Stride / Lottery 스케줄러 구현 및 정량적 Fairness 분석
- Stride 및 Lottery 스케줄러를 PintOS에 구현 (register/scheduling/unregister 인터페이스 설계)
- 시간 구간(Window) 기반 Fairness 측정 방법론을 설계하고 그 정당성을 검증
- 티켓 비율(1:2:4, 1:4:9, 1:10:20)별로 실측 — Stride는 평균 오차 0.013 / 최대 오차 0.034의 높은 공정성 확인
- 오차 그래프에서 주기적 파동 패턴을 발견하고, 주기 P = LCM(28, 25) = 700 ticks 로 수학적으로 도출
- Lottery는 큰 수의 법칙에 따라 장기적으론 수렴하나 단기 구간(특히 20:1:1)에서 starvation 발생함을 실험으로 입증

### 3. 시스템 콜 구현 (프로세스 / 파일시스템)
- 프로세스 관리(EXEC, WAIT, EXIT) 및 파일시스템(CREATE, OPEN, READ, WRITE, CLOSE, REMOVE 등) 시스템 콜 구현
- 유저가 전달한 포인터의 불법성을 고정/가변 버퍼 유형으로 분류해 검증 절차 구현
- EXEC의 ELF 로딩, WAIT/EXIT의 좀비·reaping 처리를 생산자-소비자 문제로 일반화하여 세마포어로 동기화
- 전역 락(filesys_lock)의 동시성 한계를 분석하고 per-inode 락 기반 개선안 제시

### 4. RTEMS 항공위성용 OS 파일시스템 분석
- RTEMS의 VFS 중심 계층형 파일시스템 아키텍처 분석 (IMFS, RFS, DOSFS, JFFS2, YAFFS2, DEVFS)
- BeagleBone Black 보드 + FIO 도구로 파일시스템별 I/O 성능 벤치마크 수행
- I/O 블록 크기·캐시 크기에 따른 성능 변화를 측정하고 용도별 파일시스템 선택 기준 정리

## 발표 자료

프로젝트 진행 과정에서 작성한 주차별 발표 자료를 본 저장소에 첨부했습니다.

- `핀토스투어_발표자료` : PintOS 구조 분석 (타이머, 인터럽트, 프로세스, 스케줄링, 메모리)
- `week3-proportional-schedulers` : Stride / Lottery 스케줄러 구현 및 Fairness 정량 분석
- `Userprogram` : 프로세스 / 파일시스템 시스템 콜 구현
- `Rtems_filesystems` : RTEMS 파일시스템 아키텍처 분석 및 I/O 성능 벤치마크
