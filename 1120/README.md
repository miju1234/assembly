```markdown
# 🧩 Chapter 8 — Advanced Procedures
## 🔹 8.9 Review Questions and Exercises

---

# 🧠 8.9.1 Short Answer

---

### Q1  
**문제:** What is the purpose of the PROC directive?

```asm
; 풀이:
; PROC는 새로운 프로시저(함수)의 시작을 정의한다.
; 해당 프로시저의 시작 위치를 어셈블러에게 알려주고
; 호출(call)/복귀(ret) 구조를 구성할 수 있게 한다.

; 정답:
; It marks the beginning of a procedure.
```

---

### Q2  
**문제:** What is the purpose of the ENDP directive?

```asm
; 풀이:
; ENDP는 PROC로 시작한 프로시저 정의의 끝을 표시한다.

; 정답:
; It marks the end of a procedure.
```

---

### Q3  
**문제:** What is the purpose of the RET instruction?

```asm
; 풀이:
; RET는 스택의 return address를 pop하여 호출한 위치로 복귀한다.

; 정답:
; It returns control to the calling procedure.
```

---

### Q4  
**문제:** What is the runtime stack used for?

```asm
; 풀이:
; 지역 변수 보관, 리턴 주소 저장, 프로시저 간 데이터 전달 등
; 호출 구조를 유지하는 핵심 공간.

; 정답:
; Storing return addresses, parameters, local variables, saved registers.
```

---

### Q5  
**문제:** What is a stack frame?

```asm
; 풀이:
; 프로시저 호출 시 생성되는 스택 기반 활성 레코드.
; EBP(또는 RBP)를 기준으로 지역변수/매개변수에 접근.

; 정답:
; A procedure’s activation record created on the stack.
```

---

### Q6  
**문제:** Which register is typically used as a frame pointer?

```asm
; 정답:
; EBP (또는 64bit: RBP)
```

---

### Q7  
**문제:** Why is the EBP register often pushed onto the stack at the start of a procedure?

```asm
; 풀이:
; 이전 프레임 포인터를 보존하고,
; 새로운 스택 프레임을 설정하기 위함.

; 정답:
; To preserve the previous frame pointer.
```

---

### Q8  
**문제:** What advantage does a stack frame provide?

```asm
; 풀이:
; 매개변수/지역변수 접근을 규칙화하고,
; 프로시저가 중첩 호출되어도 안정적으로 스택 구성 유지 가능.

; 정답:
; Provides stable access to procedure parameters and local variables.
```

---

### Q9  
**문제:** What is passed on the stack by the CALL instruction?

```asm
; 풀이:
; CALL은 현재 명령 다음 주소(EIP)를 스택에 push한다.

; 정답:
; The return address.
```

---

### Q10  
**문제:** Why should a procedure save registers it modifies?

```asm
; 풀이:
; 호출 규약에 맞게, 변경한 레지스터는 복원해주어야
; 상위 호출자가 예상한 상태 유지 가능.

; 정답:
; To preserve the caller’s register state.
```

---

### Q11  
**문제:** What is the difference between passing parameters by value vs by reference?

```asm
; 풀이:
; value: 데이터 복사 전달  
; reference: 메모리 주소 전달 → 원본 변경 가능

; 정답:
; Value = copy, Reference = pointer/address.
```

---

# 🧮 8.9.2 Algorithm Workbench

---

### 1️⃣  
**문제:** Write a procedure named DisplaySum that receives two integers by value and displays their sum.

```asm
; 문제 코드 (예시)
DisplaySum PROC
    push ebp
    mov ebp,esp

    mov eax, [ebp+8]    ; first
    add eax, [ebp+12]   ; second
    call WriteInt
    call Crlf

    pop ebp
    ret 8               ; 두 인자 pop
DisplaySum ENDP

; 풀이:
; - value 방식: 스택의 [ebp+8], [ebp+12]
; - 리턴 시 ret 8 로 스택 정리.

; 정답:
; DisplaySum 프로시저 구현.
```

---

### 2️⃣  
**문제:** Write a procedure that receives a pointer to an array and its length, and returns the sum.

```asm
SumArray PROC
    push ebp
    mov ebp,esp

    mov esi,[ebp+8]     ; 배열 주소
    mov ecx,[ebp+12]    ; 길이
    xor eax,eax

L1:
    add eax,[esi]
    add esi,4
    loop L1

    pop ebp
    ret 8
SumArray ENDP

; 풀이:
; - reference 방식: 배열 주소 직접 받음.
; - 길이만큼 순회 후 eax에 결과 반환.

; 정답:
; 배열 요소 합산 프로시저.
```

---

### 3️⃣  
**문제:** Write a procedure that reverses a string in place.

```asm
Reverse PROC
    push ebp
    mov ebp,esp

    mov esi,[ebp+8]      ; 시작
    mov edi,[ebp+8]      ; 끝 찾기 위해 재사용

find_end:
    cmp BYTE PTR [edi],0
    je got_end
    inc edi
    jmp find_end

got_end:
    dec edi              ; null 이전 문자

rev_loop:
    cmp esi,edi
    jge done

    mov al,[esi]
    mov bl,[edi]
    mov [esi],bl
    mov [edi],al

    inc esi
    dec edi
    jmp rev_loop

done:
    pop ebp
    ret 4
Reverse ENDP

; 풀이:
; - in-place swap 방식.
; - 양 끝에서 중앙으로 접근.

; 정답:
; 문자열 반전 프로시저.
```

---

### 4️⃣  
**문제:** Write a procedure that counts uppercase letters in a string.

```asm
CountUpper PROC
    push ebp
    mov ebp,esp

    mov esi,[ebp+8]
    xor eax,eax

L1:
    mov bl,[esi]
    cmp bl,0
    je done
    cmp bl,'A'
    jb skip
    cmp bl,'Z'
    ja skip
    inc eax
skip:
    inc esi
    jmp L1

done:
    pop ebp
    ret 4
CountUpper ENDP

; 풀이:
; - ASCII 'A'~'Z' 범위 체크.
; - eax에 개수 반환.

; 정답:
; 문자열 내 대문자 개수 카운트.
```

---

### 5️⃣  
**문제:** Write a procedure that copies a string (like strcpy).

```asm
StrCopy PROC
    push ebp
    mov ebp,esp

    mov edi,[ebp+8]   ; dest
    mov esi,[ebp+12]  ; src

L1:
    mov al,[esi]
    mov [edi],al
    cmp al,0
    je done
    inc esi
    inc edi
    jmp L1

done:
    pop ebp
    ret 8
StrCopy ENDP

; 풀이:
; - NUL 포함 전체 복사.

; 정답:
; strcpy 구현.
```

---
```
```markdown
# 🧩 Chapter 8 — Advanced Procedures
## 🔹 8.10 Programming Exercises

---

### 1️⃣ DisplaySum Application  
**문제:**  
Create a program that uses the DisplaySum procedure (two integers → sum 출력).

```asm
; 문제 코드 (예시)
INCLUDE Irvine32.inc

.code
main PROC
    push 25
    push 17
    call DisplaySum
    exit
main ENDP

DisplaySum PROC
    push ebp
    mov ebp,esp

    mov eax,[ebp+8]
    add eax,[ebp+12]
    call WriteInt
    call Crlf

    pop ebp
    ret 8
DisplaySum ENDP
END main

; 풀이:
; - 두 인자를 value 방식으로 스택에 전달.
; - [ebp+8], [ebp+12]에서 값을 읽어 합산하여 출력.

; 정답:
; DisplaySum을 호출하는 전체 예제 구현.
```

---

### 2️⃣ Count Vowels  
**문제:**  
Write a procedure that counts vowels in a string (AEIOU, aeiou).

```asm
CountVowels PROC
    push ebp
    mov ebp,esp

    mov esi,[ebp+8]   ; str ptr
    xor eax,eax       ; vowel count = 0

L1:
    mov bl,[esi]
    cmp bl,0
    je done

    ; 대문자 체크
    cmp bl,'A'
    je inc_v
    cmp bl,'E'
    je inc_v
    cmp bl,'I'
    je inc_v
    cmp bl,'O'
    je inc_v
    cmp bl,'U'
    je inc_v

    ; 소문자 체크
    cmp bl,'a'
    je inc_v
    cmp bl,'e'
    je inc_v
    cmp bl,'i'
    je inc_v
    cmp bl,'o'
    je inc_v
    cmp bl,'u'
    je inc_v

    jmp next

inc_v:
    inc eax

next:
    inc esi
    jmp L1

done:
    pop ebp
    ret 4
CountVowels ENDP

; 풀이:
; - 한 글자씩 읽어 모음 판별.
; - 대문자/소문자 모두 검사.

; 정답:
; eax = vowel count
```

---

### 3️⃣ Find Minimum in Array  
**문제:**  
Write a procedure that returns the minimum value in an array.

```asm
FindMin PROC
    push ebp
    mov ebp,esp

    mov esi,[ebp+8]     ; array ptr
    mov ecx,[ebp+12]    ; length
    mov eax,[esi]       ; first element
    add esi,4
    dec ecx

L1:
    cmp [esi],eax
    jge skip
    mov eax,[esi]
skip:
    add esi,4
    loop L1

    pop ebp
    ret 8
FindMin ENDP

; 풀이:
; - 첫 요소를 초기 min 값으로 설정.
; - 반복하면서 더 작은 값이 있으면 eax 갱신.

; 정답:
; eax = minimum value
```

---

### 4️⃣ Power Function  
**문제:**  
Compute X^N using a loop.

```asm
Power PROC
    push ebp
    mov ebp,esp

    mov eax,1           ; result
    mov ebx,[ebp+8]     ; X
    mov ecx,[ebp+12]    ; N

L1:
    cmp ecx,0
    je done
    imul eax,ebx
    dec ecx
    jmp L1

done:
    pop ebp
    ret 8
Power ENDP

; 풀이:
; - 반복 곱셈 루프.
; - 1 * X * X ... (N번)

; 정답:
; eax = X^N
```

---

### 5️⃣ Replace Character in String  
**문제:**  
Replace all occurrences of a target character with another character.

```asm
ReplaceChar PROC
    push ebp
    mov ebp,esp

    mov esi,[ebp+8]    ; string
    mov bl,[ebp+12]    ; target
    mov bh,[ebp+16]    ; replacement

L1:
    mov al,[esi]
    cmp al,0
    je done
    cmp al,bl
    jne next
    mov [esi],bh
next:
    inc esi
    jmp L1

done:
    pop ebp
    ret 12
ReplaceChar ENDP

; 풀이:
; - 문자 하나씩 검사.
; - target이면 replacement로 교체.

; 정답:
; 문자열 내 특정 문자 전체 치환.
```

---

### 6️⃣ Capitalize First Letter of Each Word  
**문제:**  
Convert “hello world example” → “Hello World Example”.

```asm
CapWords PROC
    push ebp
    mov ebp,esp

    mov esi,[ebp+8]
    mov bl,1           ; new word flag

L1:
    mov al,[esi]
    cmp al,0
    je done

    cmp bl,1
    jne skip_cap

    ; new word의 첫 문자
    cmp al,'a'
    jb skip_cap
    cmp al,'z'
    ja skip_cap
    and BYTE PTR [esi],11011111b

skip_cap:
    cmp al,' '
    jne not_space
    mov bl,1
    jmp cont
not_space:
    mov bl,0
cont:
    inc esi
    jmp L1

done:
    pop ebp
    ret 4
CapWords ENDP

; 풀이:
; - 단어 시작을 공백으로 판단.
; - 새 단어의 첫 글자를 대문자로 변환.

; 정답:
; Hello World 스타일의 문자열 변환.
```

---

### 7️⃣ Sum of Positive Numbers (Procedure Version)  
**문제:**  
Use a loop inside a procedure to sum positive integers until 0 is entered.

```asm
SumPositive PROC
    push ebp
    mov ebp,esp

    xor eax,eax   ; total = 0

L1:
    call ReadInt
    cmp eax,0
    je done
    cmp eax,0
    jle L1
    add DWORD PTR [ebp+8],eax
    jmp L1

done:
    pop ebp
    ret 4
SumPositive ENDP

; 풀이:
; - 입력 반복 → 0이면 종료.
; - 양수만 누적.

; 정답:
; positive values sum returned.
```

---

### 8️⃣ Recursive Factorial  
**문제:**  
Write a recursive function for n!.

```asm
Factorial PROC
    push ebp
    mov ebp,esp

    mov eax,[ebp+8]
    cmp eax,1
    jg recurse
    mov eax,1
    jmp done

recurse:
    dec eax
    push eax
    call Factorial
    mov ebx,[ebp+8]
    imul eax,ebx

done:
    pop ebp
    ret 4
Factorial ENDP

; 풀이:
; - if n <= 1 return 1
; - else return n * factorial(n-1)

; 정답:
; eax = n!
```

---

### 9️⃣ Print Array in Reverse  
**문제:**  
Display array elements in reverse order.

```asm
PrintReverse PROC
    push ebp
    mov ebp,esp

    mov esi,[ebp+8]      ; array
    mov ecx,[ebp+12]     ; count
    lea esi,[esi+ecx*4-4]

L1:
    mov eax,[esi]
    call WriteInt
    call Crlf
    sub esi,4
    loop L1

    pop ebp
    ret 8
PrintReverse ENDP

; 풀이:
; - 배열 끝에서 시작해 역방향 출력.

; 정답:
; array reverse printing.
```

---

### 🔟 String Length (Procedure Version)  
**문제:**  
Write a procedure that returns the length of a string.

```asm
StrLength PROC
    push ebp
    mov ebp,esp

    mov esi,[ebp+8]
    xor eax,eax

L1:
    cmp BYTE PTR [esi],0
    je done
    inc eax
    inc esi
    jmp L1

done:
    pop ebp
    ret 4
StrLength ENDP

; 풀이:
; - NUL 문자를 만날 때까지 증가.
; - eax에 길이를 저장.

; 정답:
; eax = string length
```

