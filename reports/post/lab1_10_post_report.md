# 실험 후 레포트: LAB1-10 7세그먼트 디코더

작성자: 엄상혁 (학번 2025440084) / 조: g조 / 실험일: 2026-09-14 / 소스 커밋: `2f36dbc` (https://github.com/dhawldnjs010-star/lab1_10_seg_decoder/commit/2f36dbc7e8014958276584da663d46e96727fc29) / 구현 도구·버전: Vivado 2026.1 (Build 6511674) / part: xc7s75fgga484-1 / top: `seg_decoder` (시뮬레이션 top `tb_seg_decoder`) / XDC: `constraints/pins.xdc`

경로: Vivado 경로로 수행했다.

## Vivado 시뮬레이션 — Vivado 경로

프로젝트 생성·등록: RTL(`src/seg_decoder.v`)은 Design Sources, TB(`sim/tb_seg_decoder.sv`)는 Simulation Sources, XDC(`constraints/pins.xdc`)는 Constraints에 추가했다(Copy sources 끔). 설계 top은 `seg_decoder`, 시뮬레이션 top은 `tb_seg_decoder`이다.

| 실행 | PASS 문구 | 검사 수 | 종료 시각 | 로그 |
|---|---|---|---|---|
| VS Code (Icarus) | `LAB1_PASS seg_decoder cases=16` | 16개 | 160 ns | `evidence/simulation.txt` |
| Vivado xsim | `LAB1_PASS seg_decoder cases=16` | 16개 | 160 ns | `evidence/vivado/xsim_simulate.log` |

- Icarus와 Vivado xsim의 PASS 문구, 검사 수, 종료 시각이 같다.

- 파형: `evidence/wave.vcd`(Icarus VCD).

## 오픈소스 실행 환경 — CLI 경로

이 랩은 Vivado 경로로 수행했다. CLI 경로는 사용하지 않았다.

## 합성·구현·비트스트림

| 항목 | 결과 |
|---|---|
| Run Synthesis | 완료. `Synthesis finished with 0 errors, 0 critical warnings and 0 warnings.` |
| Run Implementation | 배치·배선 완료 |
| Generate Bitstream | `write_bitstream completed successfully` |
| DRC | Checks found: 1 — CFGBVS-1(Warning) |
| Methodology | Checks found: 0 |
| 타이밍 | WNS/WHS = inf, 실패 endpoint 0. 사용자 타이밍 제약이 없는 조합회로라 통과 수치가 아니다(`Timing 38-313`). |
| 경고 | `Place 46-29`, `Power 33-232`, `Timing 38-313` |

- bit 경로: `seven_segment.runs/impl_1/seg_decoder.bit` (Git 제외) / 크기: 3,687,014 bytes / SHA-256: `6bfbe96efc4751797a8ef6f91672635a0f5d0c7ec962516f854ad5ab48735b81`

- 보고서 원본: `evidence/vivado/`의 `synth_runme.log`, `impl_runme.log`, `drc_routed.rpt`, `methodology_drc_routed.rpt`, `timing_summary_routed.rpt`.

- DRC의 `CFGBVS-1`은 CONFIG_VOLTAGE·CFGBVS 속성이 지정되지 않았다는 경고이다. 실제 보드의 구성 뱅크 전압과 대조해 해석하며 오류는 아니다.

## 실제 보드 기록·실측

연결된 장치: Spartan-7 XC7S75 교육용 보드(part `xc7s75fgga484-1`), 기록 도구: Vivado Hardware Manager (Open target → Program Device). 콘솔 로그: `evidence/board/console_lab1_10_sanghyeok.txt`.

- Hardware Manager 콘솔에서 `program_hw_devices`가 4회 실행되었다.

- 배선·입력·출력이 보이는 영상: [Google Drive 폴더](https://drive.google.com/drive/folders/17CYT6_AjE33Nx-5OtlnLNM2UfF3zLPYG)의 `20260914_175947.mp4` (2026-09-14 17:59:47 촬영).


| 조건 | 예상 출력 | 실측 출력 | 사진/영상 시각 | 일치 여부·원인 |
|---|---|---|---|---|
| bcd=0000 | seg_data=8'hfc (11111100) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=0001 | seg_data=8'h60 (01100000) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=0010 | seg_data=8'hda (11011010) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=0011 | seg_data=8'hf2 (11110010) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=0100 | seg_data=8'h66 (01100110) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=0101 | seg_data=8'hb6 (10110110) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=0110 | seg_data=8'hbe (10111110) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=0111 | seg_data=8'he0 (11100000) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=1000 | seg_data=8'hfe (11111110) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=1001 | seg_data=8'hf6 (11110110) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=1010 | seg_data=8'hee (11101110) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=1011 | seg_data=8'h3e (00111110) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=1100 | seg_data=8'h9c (10011100) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=1101 | seg_data=8'h7a (01111010) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=1110 | seg_data=8'h9e (10011110) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |
| bcd=1111 | seg_data=8'h8e (10001110) | 예상 출력과 같음 | `20260914_175947.mp4` (2026-09-14 17:59:47) | 일치 |


실측은 작성자가 보드에서 직접 확인한 결과이다.

## 비교·결론

- 예상값(진리표) → VS Code(Icarus) `LAB1_PASS seg_decoder cases=16` → Vivado xsim `LAB1_PASS seg_decoder cases=16`: 검사 16개 모두 일치하고 종료 시각 160 ns로 같다.

- 실측: 위 표의 모든 조건에서 예상 출력과 같았다. 불일치는 없었다.

- 구현 성공(bit 생성)만으로 동작을 확인한 것으로 보지 않고, 위 실측 표를 별도로 확인했다.

## 제출 링크

소스 커밋: https://github.com/dhawldnjs010-star/lab1_10_seg_decoder/commit/2f36dbc7e8014958276584da663d46e96727fc29 / 실험 전 레포트: `reports/pre/lab1_10_pre_report.md` / 로그·VCD: `evidence/simulation.txt`, `evidence/wave.vcd`, `evidence/vivado/` / bit·해시: 위 3절 (SHA-256 `6bfbe96efc4751797a8ef6f91672635a0f5d0c7ec962516f854ad5ab48735b81`) / 영상: https://drive.google.com/drive/folders/17CYT6_AjE33Nx-5OtlnLNM2UfF3zLPYG (`20260914_175947.mp4`) / GitHub에서 링크 확인한 날짜: ______
