```markdown
# 🧩 Chapter 6 — Conditional Processing

---

## 🔹 6.10 Review Questions and Exercises

---

### 🧠 6.10.1 Short Answer

---

### Q1  
**문제:** What will be the value of BX after the following instructions execute?

```asm
; 문제 코드
mov  bx,0FFFFh
and  bx,6Bh
```
; 풀이
; AND는 각 비트를 비교해 둘 다 1인 비트만 1로 남김.
; 0FFFFh = 1111 1111 1111 1111b
; 006Bh  = 0000 0000 0110 1011b
; 결과: 0000 0000 0110 1011b = 006Bh

; ✅ 정답: BX = 006Bh
```

---

### Q2  
**문제:** What will be the value of BX after the following executes?

```asm
; 문제 코드
mov bx,91BAh
and bx,92h

; 풀이
; AND는 비트 단위 논리곱.
; BX = 91BAh = 1001 0001 1011 1010b
; 92h = 0000 0000 1001 0010b
; 결과: 0000 0000 1001 0010b = 0092h

; ✅ 정답: BX = 0092h
```

---

### Q3  
**문제:** What will be the value of BX after the following executes?

```asm
; 문제 코드
mov  bx,649Bh
or   bx,3Ah

; 풀이
; OR은 하나라도 1이면 1.
; 649Bh = 0110 0100 1001 1011b
; 003Ah = 0000 0000 0011 1010b
; 결과: 0110 0100 1011 1011b = 64BBh

; ✅ 정답: BX = 64BBh
```

---

### Q4  
**문제:** What will be the value of BX after the following executes?

```asm
; 문제 코드
mov bx,29D6h
xor bx,8181h

; 풀이
; XOR은 서로 다르면 1, 같으면 0.
; 29D6h = 0010 1001 1101 0110b
; 8181h = 1000 0001 1000 0001b
; 결과: 1010 1000 0101 0111b = A857h

; ✅ 정답: BX = A857h
```

---

### Q5  
**문제:** What will be the value of EBX after the following executes?

```asm
; 문제 코드
mov  ebx,0AFAF649Bh
or   ebx,3A219604h

; 풀이
; OR 수행 시, 각 비트 중 하나라도 1이면 결과도 1.
; 결과: EBX = BFAF7E9Fh

; ✅ 정답: EBX = BFAF7E9Fh
```

---

### Q6  
**문제:** How can the TEST instruction be used to check if the AL register is even or odd?

```asm
; 풀이
; 짝수는 LSB가 0, 홀수는 LSB가 1.
; TEST AL,1 → ZF=1이면 짝수, 0이면 홀수.

; ✅ 정답:
; ZF = 1 → even
; ZF = 0 → odd
```

---

### Q7  
**문제:** What flags are affected by the TEST instruction?

```asm
; 풀이
; Affected: SF, ZF, PF
; Cleared: CF, OF
; Destination operand unaffected.

; ✅ 정답: SF, ZF, PF are set; CF and OF are cleared.
```

---

### Q8  
**문제:** Which instruction compares two operands without changing them?

```asm
; ✅ 정답: CMP
```

---

### Q9  
**문제:** Which instruction performs an implied AND without storing the result?

```asm
; ✅ 정답: TEST
```

---

### Q10  
**문제:** Which conditional jump is used when two unsigned numbers are equal?

```asm
; ✅ 정답: JE (or JZ)
```

---

## 🧮 6.10.2 Algorithm Workbench

---

### 1️⃣ Bit Mask Practice  
**문제:** Clear bit 2 of AL using AND.

```asm
; 문제 코드
and al,11111011b

; 풀이:
; 3번째 비트를 0으로 만든다.

; ✅ 정답: AL의 비트 2가 0으로 설정됨.
```

---

### 2️⃣ Compare Two Unsigned Numbers  
**문제:** Compare AX and BX. Jump to label L1 if AX > BX.

```asm
; 문제 코드
cmp ax,bx
ja L1

; ✅ 정답: JA (Jump if Above)
```

---

### 3️⃣ Determine Sign of AX  
**문제:** Jump to Negative if AX < 0.

```asm
; 문제 코드
test ax,ax
js Negative

; ✅ 정답: JS (Jump if Sign)
```

---

### 4️⃣ Scan Array for First Nonzero Value  
**문제:** Scan an array for the first nonzero value.

```asm
; 문제 코드
mov ecx,LENGTHOF array
mov esi,OFFSET array
L1: cmp WORD PTR [esi],0
jnz found
add esi,2
loop L1

; ✅ 정답: 첫 번째 0이 아닌 값 발견 시 found로 점프.
```

---

### 5️⃣ Convert String to Uppercase  
**문제:** Convert a string to uppercase.

```asm
; 문제 코드
mov ecx,LENGTHOF str
mov esi,OFFSET str
L1: and BYTE PTR [esi],11011111b
inc esi
loop L1

; 풀이:
; 비트 5를 제거하면 소문자가 대문자로 변환된다.

; ✅ 정답: 모든 문자가 대문자로 변환됨.
```

---

### 6️⃣ Simple XOR Encryption  
**문제:** Use XOR to encrypt a buffer.

```asm
; 문제 코드
mov ecx,size
mov esi,OFFSET buffer
L1: xor buffer[esi],KEY
inc esi
loop L1

; 풀이:
; XOR 연산은 대칭적이므로 같은 연산으로 복호화 가능.

; ✅ 정답: XOR 기반 간단한 암·복호화 가능.
```
```
```markdown
# 🧩 Chapter 6 — Conditional Processing

---

## 🔹 6.11 Programming Exercises

---

### 1️⃣ Check Even/Odd Number  
**문제:**  
Write a program that inputs a number and displays whether it is even or odd.

```asm
; 문제 코드
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
; 입력된 정수의 LSB를 TEST 명령으로 확인.
; LSB=0 → 짝수 (ZF=1), LSB=1 → 홀수 (ZF=0).
; ✅ 정답: 짝수면 “Even”, 홀수면 “Odd” 출력.
```

---

### 2️⃣ Sum of Positive Integers  
**문제:**  
Read integers until 0 is entered. Display the sum of positive numbers.

```asm
; 문제 코드
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
; 0이 입력될 때까지 반복, 양수만 합산.
; cmp eax,0 → jle로 음수 무시.
; ✅ 정답: 0 입력 시 종료, 양수들의 합 출력.
```

---

### 3️⃣ Upper/Lower Case Converter  
**문제:**  
Convert all lowercase letters in a string to uppercase.

```asm
; 문제 코드
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
; ASCII 소문자에서 비트 5(00100000)를 0으로 만들면 대문자됨.
; AND 11011111b 적용으로 변환 수행.
; ✅ 정답: 입력 문자열이 모두 대문자로 변환되어 출력됨.
```

---

### 4️⃣ Find Maximum Value  
**문제:**  
Scan an array of 10 integers and find the largest value.

```asm
; 문제 코드
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

; 풀이:
; 배열의 첫 값을 초기 최대값으로 설정 후 순차 비교.
; 더 큰 값 발견 시 eax 갱신.
; ✅ 정답: eax에 최댓값 저장 후 출력.
```

---

### 5️⃣ Encrypt/Decrypt Text (XOR)  
**문제:**  
Write a program that uses XOR to encrypt and decrypt text.

```asm
; 문제 코드
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

; 풀이:
; XOR는 같은 키로 두 번 연산 시 원본 복원 가능.
; 첫 번째 XOR → 암호화, 두 번째 XOR → 복호화.
; ✅ 정답: 동일 루틴으로 암호화·복호화 수행 가능.
```
