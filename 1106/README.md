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

---

# ✅ 마무리 요약
- 6.10: 조건 처리, TEST/AND/OR/XOR/비트 연산 및 조건 분기.
- 6.11: 실습 중심 프로그램 (짝수판별, 합계, 대문자 변환, 최대값, XOR 암호화).

> 📘 모든 코드는 MASM (Irvine32 라이브러리) 기준.
> 복사 버튼 한 번으로 전체 복사 가능.
```
