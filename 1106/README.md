🧩 Chapter 6 — Conditional Processing
───────────────────────────────────────────────
🔹 6.10 Review Questions and Exercises
───────────────────────────────────────────────
🧠 6.10.1 Short Answer
───────────────────────────────────────────────

──────────────────────────────
Q1.
──────────────────────────────
; 문제:
; What will be the value of BX after the following instructions execute?

mov  bx,0FFFFh
and  bx,6Bh

; 풀이:
; AND는 각 비트를 비교해 둘 다 1인 비트만 1로 남김.
; 0FFFFh = 1111 1111 1111 1111b
; 006Bh  = 0000 0000 0110 1011b
; 결과 → 0000 0000 0110 1011b = 006Bh

; ✅ 정답: BX = 006Bh

──────────────────────────────
Q2.
──────────────────────────────
mov bx,91BAh
and bx,92h

; 풀이:
; AND는 비트 단위 논리곱.
; BX = 91BAh = 1001 0001 1011 1010b
; 92h = 0000 0000 1001 0010b
; 결과 → 0000 0000 1001 0010b = 0092h

; ✅ 정답: BX = 0092h

──────────────────────────────
Q3.
──────────────────────────────
mov  bx,649Bh
or   bx,3Ah

; 풀이:
; OR은 하나라도 1이면 1.
; 649Bh = 0110 0100 1001 1011b
; 003Ah = 0000 0000 0011 1010b
; 결과 → 0110 0100 1011 1011b = 64BBh

; ✅ 정답: BX = 64BBh

──────────────────────────────
Q4.
──────────────────────────────
mov bx,29D6h
xor bx,8181h

; 풀이:
; XOR은 서로 다르면 1, 같으면 0.
; 29D6h = 0010 1001 1101 0110b
; 8181h = 1000 0001 1000 0001b
; 결과 → 1010 1000 0101 0111b = A857h

; ✅ 정답: BX = A857h

──────────────────────────────
Q5.
──────────────────────────────
mov  ebx,0AFAF649Bh
or   ebx,3A219604h

; 풀이:
; OR 수행 시, 각 비트 중 하나라도 1이면 결과도 1.
; 결과: EBX = BFAF7E9Fh

; ✅ 정답: EBX = BFAF7E9Fh

──────────────────────────────
Q6.
──────────────────────────────
; 문제:
; How can the TEST instruction be used to check if the AL register is even or odd?

test al,1

; 풀이:
; 짝수는 LSB가 0, 홀수는 LSB가 1.
; TEST AL,1 → ZF=1이면 짝수, 0이면 홀수.

; ✅ 정답:
; ZF = 1 → even
; ZF = 0 → odd

──────────────────────────────
Q7.
──────────────────────────────
; 문제:
; What flags are affected by TEST instruction?

; 풀이:
; Affected: SF, ZF, PF
; Cleared: CF, OF
; Destination operand unaffected.

; ✅ 정답: SF, ZF, PF are set; CF and OF are cleared.

──────────────────────────────
Q8.
──────────────────────────────
; 문제:
; Which instruction compares two operands without changing them?

; ✅ 정답: CMP

──────────────────────────────
Q9.
──────────────────────────────
; 문제:
; Which instruction performs an implied AND without storing the result?

; ✅ 정답: TEST

──────────────────────────────
Q10.
──────────────────────────────
; 문제:
; Which conditional jump is used when two unsigned numbers are equal?

; ✅ 정답: JE (or JZ)

───────────────────────────────────────────────
🧮 6.10.2 Algorithm Workbench
───────────────────────────────────────────────

──────────────────────────────
1️⃣ Bit Mask Practice
──────────────────────────────
; 문제: Clear bit 2 of AL using AND.

and al,11111011b

; 풀이:
; 3번째 비트를 0으로 만든다.

; ✅ 정답: AL의 비트 2가 0으로 설정됨.

──────────────────────────────
2️⃣ Compare Two Unsigned Numbers
──────────────────────────────
cmp ax,bx
ja L1

; ✅ 정답: JA (Jump if Above)

──────────────────────────────
3️⃣ Determine Sign of AX
──────────────────────────────
test ax,ax
js Negative

; ✅ 정답: JS (Jump if Sign)

──────────────────────────────
4️⃣ Scan Array for First Nonzero Value
──────────────────────────────
mov ecx,LENGTHOF array
mov esi,OFFSET array
L1: cmp WORD PTR [esi],0
jnz found
add esi,2
loop L1

; ✅ 정답: 첫 번째 0이 아닌 값 발견 시 found로 점프.

──────────────────────────────
5️⃣ Convert String to Uppercase
──────────────────────────────
mov ecx,LENGTHOF str
mov esi,OFFSET str
L1: and BYTE PTR [esi],11011111b
inc esi
loop L1

; ✅ 정답: 소문자 → 대문자 변환 (and로 비트 5 제거)

──────────────────────────────
6️⃣ Simple XOR Encryption
──────────────────────────────
mov ecx,size
mov esi,OFFSET buffer
L1: xor buffer[esi],KEY
inc esi
loop L1

; ✅ 정답: XOR로 간단한 암호화/복호화 수행 가능.

───────────────────────────────────────────────
🧩 6.11 Programming Exercises
───────────────────────────────────────────────

──────────────────────────────
1️⃣ Check Even/Odd Number
──────────────────────────────
INCLUDE Irvine32.inc
.data
msgEven BYTE "Even",0
msgOdd  BYTE "Odd",0
.code
main PROC
  call ReadInt
  test eax,1
  jz even
odd:
  mov edx,OFFSET msgOdd
  call WriteString
  jmp quit
even:
  mov edx,OFFSET msgEven
  call WriteString
quit:
  exit
main ENDP
END main

; 풀이:
; LSB 검사(TEST EAX,1)로 짝/홀 판별.
; ✅ 정답: 짝수면 ZF=1, 홀수면 ZF=0.

──────────────────────────────
2️⃣ Sum of Positive Integers
──────────────────────────────
INCLUDE Irvine32.inc
.data
sum DWORD 0
prompt BYTE "Enter number (0 to quit): ",0
msg BYTE "Sum of positives: ",0
.code
main PROC
L1:
  mov edx,OFFSET prompt
  call WriteString
  call ReadInt
  cmp eax,0
  je done
  jle L1
  add sum,eax
  jmp L1
done:
  mov edx,OFFSET msg
  call WriteString
  mov eax,sum
  call WriteInt
  exit
main ENDP
END main

; 풀이:
; 0 입력 시 종료, 양수만 누적합 출력.

──────────────────────────────
3️⃣ Upper/Lower Case Converter
──────────────────────────────
INCLUDE Irvine32.inc
.data
buffer BYTE 100 DUP(0)
prompt BYTE "Enter text: ",0
msg BYTE "Uppercase: ",0
.code
main PROC
  mov edx,OFFSET prompt
  call WriteString
  mov edx,OFFSET buffer
  mov ecx,SIZEOF buffer
  call ReadString
  mov esi,OFFSET buffer
L1:
  mov al,[esi]
  cmp al,0
  je done
  and BYTE PTR [esi],11011111b
  inc esi
  jmp L1
done:
  mov edx,OFFSET msg
  call WriteString
  mov edx,OFFSET buffer
  call WriteString
  exit
main ENDP
END main

; 풀이:
; 입력받은 문자열의 각 문자에 AND 11011111b 수행.
; ✅ 정답: 모든 문자를 대문자로 변환.

──────────────────────────────
4️⃣ Find Maximum Value
──────────────────────────────
INCLUDE Irvine32.inc
.data
array SDWORD 10,20,55,12,9,87,42,61,30,11
maxVal SDWORD ?
msg BYTE "Maximum value: ",0
.code
main PROC
  mov esi,OFFSET array
  mov eax,[esi]
  mov ecx,LENGTHOF array
  dec ecx
L1:
  add esi,4
  cmp [esi],eax
  jle next
  mov eax,[esi]
next:
  loop L1
  mov maxVal,eax
  mov edx,OFFSET msg
  call WriteString
  mov eax,maxVal
  call WriteInt
  exit
main ENDP
END main

; ✅ 정답: 배열 내 최댓값을 eax에 저장 후 출력.

──────────────────────────────
5️⃣ Encrypt/Decrypt Text (XOR)
──────────────────────────────
INCLUDE Irvine32.inc
KEY = 239
BUFMAX = 128
.data
prompt BYTE "Enter text: ",0
msgEnc BYTE "Encrypted: ",0
msgDec BYTE "Decrypted: ",0
buffer BYTE BUFMAX+1 DUP(0)
bufSize DWORD ?
.code
main PROC
  call InputString
  call TranslateBuffer
  mov edx,OFFSET msgEnc
  call DisplayMessage
  call TranslateBuffer
  mov edx,OFFSET msgDec
  call DisplayMessage
  exit
main ENDP

InputString PROC
  pushad
  mov edx,OFFSET prompt
  call WriteString
  mov edx,OFFSET buffer
  mov ecx,BUFMAX
  call ReadString
  mov bufSize,eax
  call Crlf
  popad
  ret
InputString ENDP

DisplayMessage PROC
  pushad
  call WriteString
  mov edx,OFFSET buffer
  call WriteString
  call Crlf
  popad
  ret
DisplayMessage ENDP

TranslateBuffer PROC
  pushad
  mov ecx,bufSize
  mov esi,OFFSET buffer
L1:
  xor buffer[esi],KEY
  inc esi
  loop L1
  popad
  ret
TranslateBuffer ENDP
END main

; ✅ 정답: XOR 암호화와 복호화는 동일 루틴으로 수행 가능.
───────────────────────────────────────────────
