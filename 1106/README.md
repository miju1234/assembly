### 🧠 Q1.
**문제:** What will be the value of BX after the following instructions execute?

```asm
; 문제 코드
mov  bx,0FFFFh
and  bx,6Bh

; 풀이
; AND는 각 비트를 비교해 둘 다 1인 비트만 1로 남김
; 0FFFFh = 1111 1111 1111 1111b
; 006Bh  = 0000 0000 0110 1011b
; 결과: 0000 0000 0110 1011b = 006Bh

; ✅ 정답: BX = 006Bh
