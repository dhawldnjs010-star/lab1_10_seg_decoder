# LAB1-10 7세그먼트 디코더 — 실험 후 레포트

- 과목: 전자전기컴퓨터설계실험Ⅱ / LAB1 조합논리 (교육 번호 10, 기존 번호 11)
- 작성자: 엄상혁 (학번 ______) / 조: ______ / 실험일: 2026-09-14 / 작성일: 2026-09-__

> 미수행·미확인 항목은 **미완료**로 표시했고 후속 확인을 적었다. bit 파일 생성만으로 보드 동작 성공을 선언하지 않는다.

## 1. 구현 환경 기록

| 항목 | 기록 |
|---|---|
| Vivado | 2026.1 (win64, Build 6511674) |
| part | xc7s75fgga484-1 (Spartan-7, fgga484, speed -1) |
| 설계 top | `seg_decoder` (Design Sources) |
| 시뮬레이션 top | `tb_seg_decoder` (`sim/tb_seg_decoder.sv`) |
| 핀 제약 | `constraints/pins.xdc`, IOSTANDARD LVCMOS33 |
| 소스 커밋 | `2f36dbc` — https://github.com/dhawldnjs010-star/lab1_10_seg_decoder/commit/2f36dbc7e8014958276584da663d46e96727fc29 |
| Vivado 프로젝트 | `seven_segment.runs` (Git 제외: `.gitignore`) |

## 2. VS Code(Icarus)와 Vivado 시뮬레이션 비교

두 실행은 같은 RTL과 같은 자기검사 TB(`tb_seg_decoder`)를 사용했다.

| 비교 항목 | VS Code (Icarus) | Vivado (xsim) |
|---|---|---|
| PASS 문구 | `LAB1_PASS seg_decoder cases=16` | `LAB1_PASS seg_decoder cases=16` |
| 검사 수 | 16개 | 16개 |
| 종료 시각 | 160 ns | 160 ns |
| 로그 위치 | `evidence/simulation.txt` | `seven_segment.sim/sim_1/behav/xsim/simulate.log` |
| 입력·출력 | 진리표와 일치 (사전 레포트 2절) | 같은 TB → 같은 결과 기대, 파형 캡처로 확인: ______ |

일치 여부와 차이 원인: 두 환경 모두 PASS이고 종료 시각이 같다.

## 3. 합성·구현·비트스트림

| 단계 | 결과 |
|---|---|
| Vivado 시뮬레이션 (xsim) | `LAB1_PASS seg_decoder cases=16`, 종료 160 ns |
| Run Synthesis | `synth_design completed successfully`, 0 errors / 0 critical warnings / 0 warnings: 확인 |
| Run Implementation | 배치·배선 완료(`Fully Routed`) |
| Generate Bitstream | `write_bitstream completed successfully`: 확인 |
| bit 파일 | `seven_segment.runs/impl_1/seg_decoder.bit` (3,687,014 bytes) |
| bit SHA-256 | `6bfbe96efc4751797a8ef6f91672635a0f5d0c7ec962516f854ad5ab48735b81` |
| DRC | Checks found: 1 — CFGBVS-1(Warning) |
| Methodology | Checks found: 0 |
| 타이밍 요약 | WNS/WHS = inf, 실패 endpoint 0 — 사용자 타이밍 제약이 없는 순수 조합회로라 setup/hold를 계산할 경로가 없다(`Timing 38-313`, `Power 33-232` 경고와 일치) |

- DRC의 `CFGBVS-1`은 CONFIG_VOLTAGE·CFGBVS 속성이 지정되지 않았다는 경고이다. 실제 보드의 구성 뱅크 전압과 대조해 해석한다(오류 아님).
- 경고 코드: `Place 46-29`, `Power 33-232`, `Timing 38-313` — 클록·타이밍 제약이 없는 조합회로에서 예상되는 경고이다.

현재 `constraints/pins.xdc` 상태: **정상** — 모든 포트(12개)에 PACKAGE_PIN과 IOSTANDARD(LVCMOS33)가 지정되어 있다.

## 4. 실제 장치 기록 — 미완료

조건: Spartan-7 XC7S75 교육용 보드, Hardware Manager → Open target → Auto Connect → 장치 `xc7s75` 확인 → Program Device로 bit 기록.

- 장치 인식·기록 완료: ☐
- 조교 무작위 선정 여부와 출석부 기록: ☐ 선정 ☐ 미선정

| 번호 | 입력 | 예상 출력 | 실제 출력 | 사진·영상 |
|---|---|---|---|---|
| 1 | ______ | ______ | ______ | `evidence/board/photos/______` |
| 2 | ______ | ______ | ______ | `evidence/board/videos/______` |

## 5. 결과 해석

- 예상값·두 시뮬레이션·실측의 일치 또는 차이와 원인: ______
- 사전 수정 실험에서 배운 점(실패 원인·복구): ______
- 시험 조건(입력 범위·경계 조건)에 대한 판단: ______

## 6. 미수행 항목과 후속 확인

- [ ] 보드 기록·사진·영상 및 조교 확인(4절)

## 7. 제출 점검

- [ ] Vivado 버전·part·top·핀 제약·커밋 기록(1절)
- [ ] VS Code/Vivado 비교(2절), 합성·구현·bit(3절)
- [ ] 장치 기록·사진·영상(4절), 해석(5절), 미완료 항목(6절)
- [ ] `reports/post/`, `evidence/`에 저장 후 push, GitHub 웹에서 사진·영상 확인
