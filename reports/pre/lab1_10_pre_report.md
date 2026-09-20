# LAB1-10 7세그먼트 디코더 — 실험 전 레포트

- 과목: 전자전기컴퓨터설계실험Ⅱ / LAB1 조합논리 (교육 번호 10, 기존 번호 11)
- 작성자: 엄상혁 (학번 2025440084) / 조: ______ / 작성일: 2026-09-__
- 설계 모듈: `seg_decoder` / TB: `tb_seg_decoder` / 저장소: https://github.com/dhawldnjs010-star/lab1_10_seg_decoder
- 소스 커밋: `2f36dbc` (`2f36dbc7e8014958276584da663d46e96727fc29`)

## 1. 실험 목적과 확인할 것

입력 조합과 출력의 관계를 **진리표·파형으로 설명**한다. 이 실험의 입출력은 입력 `bcd[3:0]` / 출력 `seg_data[7:0]`.

## 2. 예상 입력·출력 (진리표, 비트 순서: MSB가 왼쪽)

| bcd[3:0] | 16진 | seg_data[7:0] | hex |
|---|---|---|---|
| 0000 | 0 | 11111100 | 8'hfc |
| 0001 | 1 | 01100000 | 8'h60 |
| 0010 | 2 | 11011010 | 8'hda |
| 0011 | 3 | 11110010 | 8'hf2 |
| 0100 | 4 | 01100110 | 8'h66 |
| 0101 | 5 | 10110110 | 8'hb6 |
| 0110 | 6 | 10111110 | 8'hbe |
| 0111 | 7 | 11100000 | 8'he0 |
| 1000 | 8 | 11111110 | 8'hfe |
| 1001 | 9 | 11110110 | 8'hf6 |
| 1010 | A | 11101110 | 8'hee |
| 1011 | B | 00111110 | 8'h3e |
| 1100 | C | 10011100 | 8'h9c |
| 1101 | D | 01111010 | 8'h7a |
| 1110 | E | 10011110 | 8'h9e |
| 1111 | F | 10001110 | 8'h8e |

코드의 8비트 값을 그대로 옮긴 표이다. 세그먼트 비트 순서와 활성 레벨(active-high/low)은 보드 회로도와 대조해야 확정된다(실험 후 레포트에서 확인).

## 3. 직접 작성한 코드와 파일 역할

작성 파일: `src/seg_decoder.v`, `sim/tb_seg_decoder.sv`, `constraints/pins.xdc`

```verilog
// src/seg_decoder.v
module seg_decoder(input wire [3:0] bcd, output reg [7:0] seg_data);
    always @* begin
        case (bcd)
            0:seg_data=8'hfc; 1:seg_data=8'h60; 2:seg_data=8'hda; 3:seg_data=8'hf2;
            4:seg_data=8'h66; 5:seg_data=8'hb6; 6:seg_data=8'hbe; 7:seg_data=8'he0;
            8:seg_data=8'hfe; 9:seg_data=8'hf6; 10:seg_data=8'hee; 11:seg_data=8'h3e;
            12:seg_data=8'h9c; 13:seg_data=8'h7a; 14:seg_data=8'h9e; 15:seg_data=8'h8e;
            default: seg_data=8'h00;
        endcase
    end
endmodule
```

**설계 설명.** `case (bcd)`로 0~F 16가지를 8비트 패턴에 직접 대응시킨 룩업 테이블이다. 그 외 값은 `default: 8'h00`이다.

| 파일 | 역할 |
|---|---|
| `src/*.v` | 설계(RTL). 합성 대상이며 TB를 넣지 않는다. |
| `sim/tb_seg_decoder.sv` | DUT 연결, 입력 자극, 예상값 검사(`$fatal`), `wave.vcd` 덤프(`$dumpfile`·`$dumpvars`), `$finish`. 입력을 10 ns 간격으로 바꾸며 16개 조합을 검사한다. |
| `constraints/pins.xdc` | 보드 핀 배정(PACKAGE_PIN·IOSTANDARD·get_ports). Icarus 기능 시뮬레이션의 입력이 아니다. |
| `simulation.json` | `sources`, `testbench`(`sim/tb_seg_decoder.sv`), `simulation_top`(`tb_seg_decoder`) 지정. |

## 4. 핀 제약(XDC) 설명과 상태 — **정상**

모든 포트(12개)에 PACKAGE_PIN과 IOSTANDARD(LVCMOS33)가 지정되어 있다.

| 포트 | PACKAGE_PIN | IOSTANDARD |
|---|---|---|
| bcd[3] | Y1 | LVCMOS33 |
| bcd[2] | W3 | LVCMOS33 |
| bcd[1] | U2 | LVCMOS33 |
| bcd[0] | T1 | LVCMOS33 |
| seg_data[7] | P1 | LVCMOS33 |
| seg_data[6] | P3 | LVCMOS33 |
| seg_data[5] | P7 | LVCMOS33 |
| seg_data[4] | N3 | LVCMOS33 |
| seg_data[3] | T5 | LVCMOS33 |
| seg_data[2] | R2 | LVCMOS33 |
| seg_data[1] | R4 | LVCMOS33 |
| seg_data[0] | R6 | LVCMOS33 |

- `get_ports`의 이름·대괄호 표기가 RTL 포트명과 일치해야 한다. XDC는 시뮬레이션이 검증하지 않으므로 핀 배정은 Vivado 구현·보드에서 별도로 확인한다.

## 5. 사전 시뮬레이션 결과 (VS Code + Icarus Verilog)

| 항목 | 결과 |
|---|---|
| 콘솔 PASS 문구 | `LAB1_PASS seg_decoder cases=16` |
| 검사한 입력 조합 수 | 16개 (진리표 전 조합 검사) |
| 종료 시각 | 160 ns (`$finish`) |
| 로그·파형 | `evidence/simulation.txt`, `evidence/wave.vcd` (저장소에 커밋됨) |
| 파형 캡처 | ______ (VaporView에서 입력·출력 확대 후 캡처, `evidence/`에 저장) |

> `LAB1_PASS`는 TB가 계산한 기대값과 출력이 전부 일치했다는 자기검사 결과이다. TB의 기대식이 설계와 같은 관점으로 쓰였는지는 위 진리표와 파형으로 직접 대조해 설명한다.

- 파형에서 확인한 대표 구간(시간 · 입력 → 출력): ______
- 진리표와 어긋난 부분과 원인: ______

## 6. 수정 전후 결과 (실패 → 복구 실험)

| 단계 | 내용 |
|---|---|
| 정상 | 위 5절의 PASS 로그를 보관 |
| 수정 제안 | `8: seg_data=8'hfe` → `8'hfc` |
| 기대되는 실패 | bcd=8 한 행만 불일치(8이 0처럼 표시) — 실패 로그에서 vector·expected·actual 확인 |
| 실제 실패 로그 | ______ (`build/sim/run-.../simulation.log`) |
| 복구 후 | 원래대로 복구, Save All → `02 Simulate` → PASS 재확인: ☐ |
| 변경 이유·원인·복구 결과 | ______ |

## 7. 실험 당일 보드 확인 계획

- 장비: Spartan-7 XC7S75 교육용 보드(part `xc7s75fgga484-1`). 연결 전 전원·핀 기능·I/O 전압(LVCMOS33)을 확인하고, 배선 변경은 전원을 끈 상태에서 한다.
- 확인 계획: bcd 16가지를 넣어 7세그먼트가 0~F로 보이는지, 실제 점등 패턴이 코드 값과 맞는지(활성 레벨 포함) 확인한다.
- 조합회로라 클록이 없다. TB의 10 ns 간격은 검사를 빠르게 하기 위한 값이며, 보드에서는 스위치를 손으로 바꾸므로 **입력 조합**으로만 동작을 해석한다.
- 사진에는 보드 연결과 입력·출력 위치가 함께 보이게 촬영한다.
- 조교가 무작위로 고른 2개 실험 번호는 출석부에 기록된다: ☐ 선정됨 ☐ 시연 완료

## 8. 제출 점검

- [x] 진리표(2절), 코드·XDC 설명(3·4절), 사전 시뮬레이션 로그(5절)
- [ ] 사전 파형 캡처(5절), 수정 전후 실험(6절)
- [ ] `reports/pre/`에 저장 후 commit·push
