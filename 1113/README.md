# 📘 Chapter 7 — Integer Arithmetic (PPT 핵심 요약)

---

## 🔹 1. Signed / Unsigned Arithmetic  
- **ADD / SUB**  
  - 부호/무부호 모두 사용 가능  
  - CF: unsigned carry  
  - OF: signed overflow  
- **INC / DEC**  
  - CF를 변경하지 않음  

---

## 🔹 2. Extended Addition / Subtraction  
- **ADC** : Carry 포함 덧셈  
- **SBB** : Borrow 포함 뺄셈  
- 64비트 정수 연산 →  
  1) 하위 DWORD  
  2) 상위 DWORD + ADC/SBB  

---

## 🔹 3. Multiplication (MUL / IMUL)  
- **MUL** → unsigned multiply  
- **IMUL** → signed multiply  
- 암시적 레지스터 규칙:  
  - 8bit:  AL × r/m8  → AX  
  - 16bit: AX × r/m16 → DX:AX  
  - 32bit: EAX × r/m32 → EDX:EAX  

---

## 🔹 4. Division (DIV / IDIV)  
- **DIV** → unsigned divide  
- **IDIV** → signed divide  
- 나누기 직전 반드시 **부호 확장 필요**  
  - 8bit → CBW  
  - 16bit → CWD  
  - 32bit → CDQ  
  - 64bit → CQO  
- 결과 저장 위치:  
  - 몫: AL / AX / EAX  
  - 나머지: AH / DX / EDX  

---

## 🔹 5. Shift Instructions  
- **SHL/SAL**: 왼쪽 시프트 → ×2  
- **SHR**: 오른쪽 시프트 (unsigned) → ÷2  
- **SAR**: 오른쪽 시프트 (signed) → ÷2  
- N비트 이동 → ×2ⁿ 또는 ÷2ⁿ  

---

## 🔹 6. Rotate Instructions  
- **ROL / ROR** → 비트 순환  
- **RCL / RCR** → CF를 포함해 순환  

---

## 🔹 7. ASCII & Packed BCD Adjust  
- **AAA / AAS** : ASCII 보정  
- **AAM / AAD** : BCD 변환  
- **DAA / DAS** : Packed BCD 보정  

---

## 🔹 8. Important Rules  
- DIV/IDIV 실행 전 EDX:EAX(또는 DX:AX) 설정 필수  
- 부호 확장은 CBW/CWD/CDQ/CQO  
- 곱셈/나눗셈 시 overflow 발생 가능  
- Shift는 곱셈/나눗셈의 효율적 대체 연산  

---


# 🧩 Chapter 7 — Integer Arithmetic
## 🔹 7.9 Review Questions and Exercises
---

# 🧠 7.9.1 Short Answer

---

### Q1  
**문제:** What is the purpose of CBW, CWD, CDQ, and CQO?

```asm
; 풀이:
; CBW: AL의 부호를 AH로 확장하여 AX로 만드는 명령.
; CWD: AX의 부호를 DX로 확장하여 DX:AX 구성.
; CDQ: EAX의 부호를 EDX로 확장하여 EDX:EAX 구성.
; CQO: RAX의 부호를 RDX로 확장하여 RDX:RAX 구성 (64비트).

; 정답 요약:
; 부호 확장을 위한 명령들이다.
```

---

### Q2  
**문제:** When is the CF flag set by the ADD instruction?

```asm
; 풀이:
; ADD는 unsigned overflow(즉, 자리올림)가 발생하면 CF=1로 세트된다.

; 정답:
; When an unsigned carry-out occurs.
```

---

### Q3  
**문제:** When is the OF flag set by the ADD instruction?

```asm
; 풀이:
; OF는 signed overflow: 즉, 결과가 signed 표현 범위를 벗어날 때 1로 세트.

; 정답:
; When a signed overflow occurs.
```

---

### Q4  
**문제:** How does the MUL instruction differ from the IMUL instruction?

```asm
; 풀이:
; MUL: unsigned multiplication.
; IMUL: signed multiplication.
; 또한 IMUL은 여러 형태의 피연산자를 지원.

; 정답:
; MUL=unsigned, IMUL=signed.
```

---

### Q5  
**문제:** Which instruction(s) use(edx:eax) as an implicit operand?

```asm
; 풀이:
; 32비트에서 EDX:EAX 묶음을 사용하는 명령은 DIV, IDIV, MUL, IMUL 등 고정된 형태.

; 정답:
; MUL, IMUL, DIV, IDIV.
```

---

### Q6  
**문제:** What is the purpose of the AAS, AAA, AAM, AAD instructions?

```asm
; 풀이:
; ASCII/BCD 조작을 위한 특별한 보정 명령들이다.
; AAA, AAS: ASCII adjust after addition/subtraction.
; AAM, AAD: unpacked BCD 변환 조정.

; 정답:
; They adjust results for ASCII or BCD arithmetic instructions.
```

---

### Q7  
**문제:** What is the purpose of the DAA and DAS instructions?

```asm
; 풀이:
; DAA: BCD addition 보정.
; DAS: BCD subtraction 보정.

; 정답:
; Adjust packed BCD addition/subtraction results.
```

---

### Q8  
**문제:** Which flags are affected by the INC instruction?

```asm
; 풀이:
; INC는 CF를 절대 변경하지 않음.
; OF, SF, ZF, AF, PF 등을 변경한다.

; 정답:
; All arithmetic flags except CF.
```

---

### Q9  
**문제:** Which flags are affected by the DEC instruction?

```asm
; 풀이:
; DEC 역시 CF는 변경하지 않음.
; OF, SF, ZF, AF, PF는 영향을 받음.

; 정답:
; Same as INC: all except CF.
```

---

### Q10  
**문제:** What is the difference between DIV and IDIV?

```asm
; 풀이:
; DIV: unsigned division.
; IDIV: signed division.

; 정답:
; DIV=unsigned, IDIV=signed.
```

---

# 🧮 7.9.2 Algorithm Workbench

---

### Q1  
**문제:** Write code that sign-extends AL into AX.

```asm
; 문제 코드
cbw

; 풀이:
; CBW는 AL의 부호비트를 AH로 확장하여 AX를 구성한다.

; 정답:
; cbw 사용.
```

---

### Q2  
**문제:** Sign-extend AX into DX:AX.

```asm
cwd

; 풀이:
; CWD는 AX의 부호비트를 DX 전체로 확장한다.

; 정답:
; cwd 사용.
```

---

### Q3  
**문제:** Sign-extend EAX into EDX:EAX.

```asm
cdq

; 풀이:
; CDQ는 EAX의 부호비트를 EDX 전체에 복사한다.

; 정답:
; cdq 사용.
```

---

### Q4  
**문제:** Multiply EAX by the contents of a memory operand named VAL32.

```asm
imul VAL32

; 풀이:
; 단일 피연산자 IMUL 형태는 EAX * VAL32 → EDX:EAX 배치.

; 정답:
; imul VAL32
```

---

### Q5  
**문제:** Unsigned multiply AX by BX, storing the result in DX:AX.

```asm
mul bx

; 풀이:
; MUL (16비트)은 AX * BX = DX:AX 구성.

; 정답:
; mul bx
```

---

### Q6  
**문제:** Unsigned divide DX:AX by BX.

```asm
div bx

; 풀이:
; 몫 = AX, 나머지 = DX.

; 정답:
; div bx
```

---

### Q7  
**문제:** Signed divide DX:AX by BX.

```asm
idiv bx

; 풀이:
; 몫=AX, 나머지=DX. 부호있는 나눗셈.

; 정답:
; idiv bx
```

---

### Q8  
**문제:** Write code that doubles the value in EAX using a shift instruction.

```asm
shl eax,1

; 풀이:
; 왼쪽 시프트 1비트는 ×2 효과.

; 정답:
; shl eax,1
```

---

### Q9  
**문제:** Write code that divides EAX by 4 using a shift instruction.

```asm
sar eax,2

; 풀이:
; 부호가 있는 정수라면 SAR 사용.
; SHL/SHR은 부호 보존되지 않음.

; 정답:
; sar eax,2
```

---

### Q10  
**문제:** Subtract 1 from EBX, but do not modify the Carry flag.

```asm
dec ebx

; 풀이:
; DEC는 CF에 영향을 주지 않는 유일한 감소 명령.

; 정답:
; dec ebx
```

---
# 🧩 Chapter 7 — Integer Arithmetic
## 🔹 7.10 Programming Exercises
---

### 1️⃣ AddTwo (확장 버전)  
**문제:**  
Modify the AddTwo program so that it handles 32-bit and 64-bit values properly.

```asm
; 문제 코드 (예시 구현)
INCLUDE Irvine32.inc

.data
val1 QWORD 100000000h
val2 QWORD 200000000h
sum  QWORD ?

.code
main PROC
    ; 하위 DWORD 더하기
    mov eax, DWORD PTR val1
    add eax, DWORD PTR val2
    mov DWORD PTR sum, eax

    ; 상위 DWORD + carry 반영
    mov edx, DWORD PTR val1+4
    adc edx, DWORD PTR val2+4
    mov DWORD PTR sum+4, edx

    ; 결과 출력
    mov eax, DWORD PTR sum
    call WriteHex
    mov eax, DWORD PTR sum+4
    call WriteHex

    exit
main ENDP
END main

; 풀이:
; - 32비트 환경(MASM/Irvine32)에서는 QWORD 전체를 한 번에 덧셈할 수 없다.
; - 따라서 하위 DWORD → ADD, 상위 DWORD → ADC 로 각각 처리.
; - sum에 64비트 완성.

; 정답:
; ADD + ADC 를 이용한 64비트 정수 덧셈 구현.
```

---

### 2️⃣ Integer Multiplication Table  
**문제:**  
Display a multiplication table using integer multiplication instructions.

```asm
; 문제 코드 (예시)
INCLUDE Irvine32.inc

.data
msg BYTE "Multiplication Table",0

.code
main PROC
    mov ecx,10        ; row
outer:
    mov ebx,1
inner:
    mov eax,ecx
    imul ebx          ; eax = ecx * ebx
    call WriteDec
    mov al,' '
    call WriteChar
    inc ebx
    cmp ebx,11
    jle inner

    call Crlf
    loop outer

    exit
main ENDP
END main

; 풀이:
; - 이중 루프 사용.
; - IMUL로 ecx * ebx 계산.
; - WriteDec 이용하여 표처럼 출력.

; 정답:
; IMUL을 활용한 1~10 곱셈표 출력.
```

---

### 3️⃣ Salary Calculator  
**문제:**  
Read hourly wage and hours worked. Compute gross pay using integer multiply.

```asm
; 문제 코드 (예시)
INCLUDE Irvine32.inc

.data
prompt1 BYTE "Hourly wage: ",0
prompt2 BYTE "Hours worked: ",0
msg     BYTE "Gross pay: ",0

.code
main PROC
    mov edx,OFFSET prompt1
    call WriteString
    call ReadInt
    mov ebx,eax        ; wage

    mov edx,OFFSET prompt2
    call WriteString
    call ReadInt       ; hours

    imul ebx           ; eax = hours * wage

    mov edx,OFFSET msg
    call WriteString
    call WriteDec

    exit
main ENDP
END main

; 풀이:
; - 시급(정수)과 근무 시간(정수)을 입력.
; - IMUL로 곱함.
; - EAX로 출력.

; 정답:
; gross pay = wage * hours.
```

---

### 4️⃣ Packed BCD Addition  
**문제:**  
Add two packed BCD values using ADD and DAA.

```asm
; 문제 코드 (예시)
INCLUDE Irvine32.inc

.data
bcd1 BYTE 45h   ; BCD 45
bcd2 BYTE 38h   ; BCD 38
sum  BYTE ?

.code
main PROC
    mov al,bcd1
    add al,bcd2
    daa              ; BCD 보정
    mov sum,al

    movzx eax,sum
    call WriteHex

    exit
main ENDP
END main

; 풀이:
; - ADD로 더한 뒤, BCD 오류를 보정하려면 반드시 DAA 사용.
; - 예: 45 + 38 = 83 (BCD)

; 정답:
; sum = 83h (BCD)
```

---

### 5️⃣ Packed BCD Subtraction  
**문제:**  
Subtract two packed BCD values using SUB and DAS.

```asm
; 문제 코드 (예시)
INCLUDE Irvine32.inc

.data
bcd1 BYTE 59h
bcd2 BYTE 23h
diff BYTE ?

.code
main PROC
    mov al,bcd1
    sub al,bcd2
    das
    mov diff,al

    movzx eax,diff
    call WriteHex

    exit
main ENDP
END main

; 풀이:
; - SUB 후 DAS 적용으로 BCD 보정.
; - 59 - 23 = 36 (BCD)

; 정답:
; diff = 36h (BCD)
```

---

### 6️⃣ Division with Remainder  
**문제:**  
Read two integers and show quotient and remainder.

```asm
; 문제 코드 (예시)
INCLUDE Irvine32.inc

.data
prompt1 BYTE "Dividend: ",0
prompt2 BYTE "Divisor: ",0
msgQ BYTE "Quotient: ",0
msgR BYTE "Remainder: ",0

.code
main PROC
    mov edx,OFFSET prompt1
    call WriteString
    call ReadInt
    mov ebx,eax        ; dividend → ebx

    mov edx,OFFSET prompt2
    call WriteString
    call ReadInt       ; divisor
    mov ecx,eax        ; divisor → ecx

    mov eax,ebx
    cdq
    idiv ecx           ; quotient = eax, remainder = edx

    mov edx,OFFSET msgQ
    call WriteString
    mov eax,eax
    call WriteInt
    call Crlf

    mov edx,OFFSET msgR
    call WriteString
    mov eax,edx
    call WriteInt

    exit
main ENDP
END main

; 풀이:
; - idiv ecx → 몫=eax, 나머지=edx.
; - 32비트 signed divide에 맞게 cdq 사용.

; 정답:
; quotient = eax, remainder = edx.
```

---

### 7️⃣ Shift Multiply  
**문제:**  
Multiply EAX by 32 using shifts instead of MUL.

```asm
; 문제 코드
shl eax,5

; 풀이:
; 2^5 = 32 → shl eax,5 는 eax × 32.

; 정답:
; shl eax,5
```

---

### 8️⃣ Shift Divide  
**문제:**  
Divide EAX by 8 using shifts instead of DIV.

```asm
; 문제 코드
sar eax,3

; 풀이:
; 2^3 = 8 → sar은 signed division에 해당.

; 정답:
; sar eax,3
```

---

### 9️⃣ Absolute Value  
**문제:**  
Compute the absolute value of a signed integer in EAX.

```asm
; 문제 코드
cmp eax,0
jge done
neg eax
done:

; 풀이:
; 음수이면 neg eax로 양수화.

; 정답:
; if eax < 0, then neg eax.
```

---

### 🔟 Compare Two 64-bit Integers  
**문제:**  
Compare two 64-bit integers stored as QWORDs and show which is larger.

```asm
; 문제 코드 (예시)
INCLUDE Irvine32.inc

.data
A  QWORD 100000000h
B  QWORD 0F0000000h

msgA BYTE "A is larger",0
msgB BYTE "B is larger",0
msgE BYTE "Equal",0

.code
main PROC
    ; 상위 DWORD 먼저 비교
    mov eax, DWORD PTR A+4
    cmp eax, DWORD PTR B+4
    ja  showA
    jb  showB

    ; 상위가 같으면 하위 DWORD 비교
    mov eax, DWORD PTR A
    cmp eax, DWORD PTR B
    ja showA
    jb showB

    ; 같음
    mov edx,OFFSET msgE
    call WriteString
    jmp quit

showA:
    mov edx,OFFSET msgA
    call WriteString
    jmp quit

showB:
    mov edx,OFFSET msgB
    call WriteString

quit:
    exit
main ENDP
END main

; 풀이:
; - QWORD 비교는 상위 DWORD부터 비교해야 한다.
; - 같으면 하위 DWORD 비교.

; 정답:
; Compare high DWORD → compare low DWORD.
```

---

