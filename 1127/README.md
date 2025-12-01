# 🧩 Chapter 9 — Strings and Arrays  
## 9.9.1 Short Answer (Q1–Q14)

---

### 🧠 Q1
```asm
; 문제 원문(영문):
; 1. Which Direction flag setting causes index registers to move backward through memory 
;    when executing string primitives?

; 문제 해석(한글):
; - 문자열 명령을 실행할 때, 인덱스 레지스터가 메모리를 역방향(감소)으로 이동하도록 하는 DF 설정은?

; 풀이:
; DF = 1이면 방향이 backward(감소 방향)이다.
; DF은 방향 플래그로, 문자열 명령에서 ESI/EDI가 증가/감소할지 결정함.

; 정답:
; DF = 1 (index registers move backward).
```

---

### 🧠 Q2
```asm
; 문제 원문(영문):
; 2. When a repeat prefix is used with STOSW, what value is added to or subtracted 
;    from the index register?

; 문제 해석(한글):
; - STOSW 명령에서 REP 프리픽스가 사용될 때, 인덱스 레지스터는 얼마씩 증가/감소하는가?

; 풀이:
; STOSW는 word(2 bytes)를 저장한다.
; DF=0이면 +2 증가, DF=1이면 -2 감소.

; 정답:
; ±2 is added to/subtracted from EDI.
```

---

### 🧠 Q3
```asm
; 문제 원문(영문):
; 3. In what way is the CMPS instruction ambiguous?

; 문제 해석(한글):
; - CMPS 명령이 애매한 이유는 무엇인가?

; 풀이:
; CMPS는 단순히 두 메모리 블록을 비교할 뿐,
; “문자”인지 “바이트 데이터”인지 의미적 정보가 없음 → 애매함.

; 정답:
; It compares raw bytes/words without semantic meaning → ambiguous.
```

---

### 🧠 Q4
```asm
; 문제 원문(영문):
; 4. When the Direction flag is clear and SCASB has found a matching character, 
;    where does EDI point?

; 문제 해석(한글):
; - DF=0이고 SCASB가 문자를 찾았을 때, EDI는 어디를 가리키는가?

; 풀이:
; DF=0이면 forward 방향 → EDI는 "다음 비교할 위치"를 가리키게 됨.
; 즉, 매칭된 문자 다음 바이트를 가리킴.

; 정답:
; EDI points to the byte *after* the matching character.
```

---

### 🧠 Q5
```asm
; 문제 원문(영문):
; 5. When scanning an array for the first occurrence of a particular character, 
;    which repeat prefix would be best?

; 문제 해석(한글):
; - 특정 문자의 첫 번째 발생 위치를 찾을 때 어떤 repeat prefix가 가장 적합한가?

; 풀이:
; 첫 "일치"일 때 멈추면 됨 → REPNE SCASB 사용.

; 정답:
; REPNE (repeat while not equal).
```

---

### 🧠 Q6
```asm
; 문제 원문(영문):
; 6. What Direction flag setting is used in the Str_trim procedure from Section 9.3?

; 문제 해석(한글):
; - Str_trim 절차에서는 DF 값을 어떻게 설정하는가?

; 풀이:
; Str_trim은 문자열 앞/뒤 공백을 제거하기 위해 forward 스캔이 필요 → DF=0.

; 정답:
; DF = 0 (forward direction).
```

---

### 🧠 Q7
```asm
; 문제 원문(영문):
; 7. Why does the Str_trim procedure from Section 9.3 use the JNE instruction?

; 문제 해석(한글):
; - Str_trim에서 JNE 명령을 사용하는 이유는?

; 풀이:
; 비교 결과가 다를 때(unequal) 루프 유지해야 해서 JNE 사용.

; 정답:
; Because it loops while characters are not equal (JNE).
```

---

### 🧠 Q8
```asm
; 문제 원문(영문):
; 8. What happens in the Str_ucase procedure from Section 9.3 if the target string contains a digit?

; 문제 해석(한글):
; - Str_ucase 함수가 문자열을 처리할 때, 숫자가 들어 있으면 어떻게 되는가?

; 풀이:
; Str_ucase는 알파벳 소문자 범위(a~z)만 검사한다.
; 숫자는 조건에 맞지 않으므로 아무 변화 없음.

; 정답:
; Digits are unchanged.
```

---

### 🧠 Q9
```asm
; 문제 원문(영문):
; 9. If the Str_length function from Section 9.3 used SCASB, which repeat prefix would be most appropriate?

; 문제 해석(한글):
; - Str_length에서 SCASB를 사용한다면 어떤 프리픽스를 사용하는 것이 가장 적절한가?

; 풀이:
; 문자열 길이는 null(0)까지 비교 → REPNE SCASB 사용.

; 정답:
; REPNE.
```

---

### 🧠 Q10
```asm
; 문제 원문(영문):
; 10. If the Str_length function from Section 9.3 used SCASB, how would it calculate 
;     and return the string length?

; 문제 해석(한글):
; - SCASB로 문자열 길이를 구한다면, 결과 길이는 어떻게 계산되는가?

; 풀이:
; ECX은 남은 반복 횟수를 가진다.
; 문자열의 총 길이 = 초기 ECX값 - 최종 ECX값 - 1

; 정답:
; Length = (initial ECX – final ECX – 1).
```

---

### 🧠 Q11
```asm
; 문제 원문(영문):
; 11. What is the maximum number of comparisons needed by the binary search algorithm 
;     when an array contains 1,024 elements?

; 문제 해석(한글):
; - 원소 1024개일 때 이진 탐색의 최대 비교 횟수는?

; 풀이:
; log2(1024) = 10

; 정답:
; 10 comparisons.
```

---

### 🧠 Q12
```asm
; 문제 원문(영문):
; 12. In the BinarySearch procedure from Section 9.5, why could the statement 
;     at label L2 be removed without affecting the outcome?

; 문제 해석(한글):
; - Section 9.5의 BinarySearch에서 L2 문장을 제거해도 결과가 동일한 이유?

; 풀이:
; L2는 불필요한 중간 점프이며 로직을 변경하지 않음.

; 정답:
; It was redundant; removing it doesn't change the logic.
```

---

### 🧠 Q13
```asm
; 문제 원문(영문):
; 13. In the BinarySearch procedure from Section 9.5, what might the statement at label L4 be eliminated?

; 문제 해석(한글):
; - Section 9.5의 BinarySearch에서 L4 문장을 제거할 수 있는 이유는?

; 풀이:
; L4도 마찬가지로 불필요한 제어 흐름 → 제거해도 기능 동일.

; 정답:
; It is also redundant.
```

---

### 🧠 Q14
```asm
; 문제 원문(영문):
; 14. In the BinarySearch procedure from Section 9.5, why might the statement at label L4 
;     be eliminated without affecting the outcome?

; 문제 해석(한글):
; - L4 제거해도 결과가 변하지 않는 이유?

; 풀이:
; L4는 단순한 분기 포인트이며 실제 비교/계산에 영향 없음.

; 정답:
; Because it does not alter the search logic.
```

# 🧩 Chapter 9 — Strings and Arrays  
## 9.9.2 Algorithm Workbench (Q1–Q12)

---

### 🧠 Q1
```asm
; 문제 원문 (영문):
; 1. Show an example of a base-index operand in 32-bit mode.

; 문제 해석 (한국어):
; - 32비트 모드에서 base-index 주소 지정 방식의 예를 하나 보여라.

; 풀이:
; base-index = [base + index]
; 예: [EAX + EBX]

; 정답:
; [EAX + EBX]
```

---

### 🧠 Q2
```asm
; 문제 원문 (영문):
; 2. Show an example of a base-index-displacement operand in 32-bit mode.

; 문제 해석:
; - 32비트 모드에서 base-index-displacement(변위 포함) 주소 방식 예시를 보여라.

; 풀이:
; base-index-displacement = [base + index + disp]

; 정답:
; [EAX + EBX + 4]
```

---

### 🧠 Q3
```asm
; 문제 원문:
; 3. Suppose a two-dimensional array of doublewords has three logical rows and four logical
;    columns. Write an expression using ESI and EDI that addresses the third column in the 
;    second row. (Numbering for rows and columns starts at zero.)

; 문제 해석:
; - 2D doubleword(4바이트) 배열: 3행 × 4열.
; - 두 번째 행(1), 세 번째 열(2)를 ESI, EDI 사용해 주소 표현하라.

; 풀이:
; doubleword = 4 bytes
; rowSize = columns * 4 = 4 * 4 = 16 bytes
; address = base + rowIndex * rowSize + colIndex * 4
; → = ESI + 1*16 + 2*4 = ESI + 24

; 정답:
; [ESI + 24]
```

---

### 🧠 Q4
```asm
; 문제 원문:
; 4. Write instructions using CMPSW that compare two arrays of 16-bit values named sourcew
;    and targetw.

; 문제 해석:
; - sourcew, targetw라는 16비트 배열 두 개를 CMPSW로 비교하라.

; 풀이:
; CMPSW는 [ESI] ↔ [EDI] 비교

; 정답:
; mov esi, OFFSET sourcew
; mov edi, OFFSET targetw
; cmpsw
```

---

### 🧠 Q5
```asm
; 문제 원문:
; 5. Write instructions that use SCASW to scan for the 16-bit value 0100h in an array named
;    wordArray, and copy the offset of the matching member into the EAX register.

; 문제 해석:
; - wordArray에서 0100h 값을 SCASW로 검색하여,
;   찾은 위치의 offset을 EAX로 복사하라.

; 풀이:
; 1) AX에 검색값 0100h 저장
; 2) EDI = wordArray
; 3) REPNE SCASW로 검색
; 4) offset = EDI - base - 2 (SCASW는 2바이트씩 이동)

; 정답 예시:
; mov ax,0100h
; mov edi,OFFSET wordArray
; mov ecx,LENGTHOF wordArray
; repne scasw
; sub edi,OFFSET wordArray
; sub edi,2
; mov eax,edi
```

---

### 🧠 Q6
```asm
; 문제 원문:
; 6. Write a sequence of instructions that use the Str_compare procedure to determine 
;    the larger of two input strings and write it to the console window.

; 문제 해석:
; - Str_compare 프로시저로 두 문자열을 비교하여
;   더 큰(사전순으로 뒤) 문자열을 출력하라.

; 풀이:
; Str_compare:
; EAX < 0 → str1 < str2
; EAX = 0 → equal
; EAX > 0 → str1 > str2

; 정답 예시:
; INVOKE Str_compare, ADDR str1, ADDR str2
; cmp eax,0
; jg printStr1
; mov edx,ADDR str2
; jmp print
; printStr1:
; mov edx,ADDR str1
; print:
; call WriteString
```

---

### 🧠 Q7
```asm
; 문제 원문:
; 7. Show how to call the Str_trim procedure and remove all trailing "@" characters 
;    from a string.

; 문제 해석:
; - Str_trim을 호출하여 문자열 끝의 '@' 문자들을 제거하라.

; 풀이:
; Str_trim은 기본적으로 공백 제거 → 수정하여 '@'를 제거하는 형태 사용

; 정답 예시:
; mov edx, OFFSET myString
; mov al,'@'
; call Str_trim
```

---

### 🧠 Q8
```asm
; 문제 원문:
; 8. Show how to modify the Str_ucase procedure from the Irvine32 library so it changes 
;    all characters to lower case.

; 문제 해석:
; - Str_ucase를 수정해 문자열 전체를 소문자로 변환하라.

; 풀이:
; 대문자 'A'~'Z' → OR 20h → 소문자

; 정답 예시:
; ; inside loop
; cmp al,'A'
; jl  next
; cmp al,'Z'
; jg  next
; or  al,20h
```

---

### 🧠 Q9
```asm
; 문제 원문:
; 9. Create a 64-bit version of the Str_trim procedure.

; 문제 해석:
; - Str_trim의 64비트 버전을 작성하라.

; 풀이:
; 64비트 모드 → RSI, RDI 사용 / DF=0 / 공백 또는 특정 문자 제거

; 정답 (구조 예시):
; cld
; mov rsi, rdi
; ; scan leading
; ; scan trailing
; ; rewrite trimmed string
```

---

### 🧠 Q10
```asm
; 문제 원문:
; 10. Show an example of a base-index operand in 64-bit mode.

; 문제 해석:
; - 64비트 모드에서 base-index 주소 방식 예시를 보여라.

; 풀이:
; base-index = [base + index*scale]

; 정답:
; [RAX + RBX]
```

---

### 🧠 Q11
```asm
; 문제 원문:
; 11. Assuming that EBX contains a row index into a two-dimensional array of 32-bit integers
;     named myArray and EDI contains the index of a column, write a single statement that
;     moves the content of the given array element into the EAX register.

; 문제 해석:
; - EBX = row index, EDI = column index
; - 32비트 정수 배열 myArray[row][col] 값을 EAX로 읽어라.

; 풀이:
; elementSize = 4 bytes
; colOffset = EDI * 4
; rowOffset = EBX * rowSize

; 정답:
; mov eax, [myArray + ebx*ROW_SIZE + edi*4]
```

---

### 🧠 Q12
```asm
; 문제 원문:
; 12. Assuming that RBX contains a row index into a two-dimensional array of 64-bit integers
;     named myArray and RDI contains the index of a column, write a single statement that
;     moves the content of the given array element into the RAX register.

; 문제 해석:
; - RBX = 행 인덱스, RDI = 열 인덱스
; - 64비트 정수 배열 요소를 RAX로 읽어라.

; 풀이:
; elementSize = 8 bytes

; 정답:
; mov rax, [myArray + rbx*ROW_SIZE + rdi*8]
```

# 🧩 Chapter 9 — 9.10 Programming Exercises (1–7)

---

## ⭐ Exercise 1 — Improved Str_copy Procedure
```asm
; 문제 원문 (영문 그대로):
; 1. Improved Str_copy Procedure
; The Str_copy procedure shown in this chapter does not limit the number of characters to be copied.
; Create a new version (named Str_copyN) that receives an additional input parameter indicating the
; maximum number of characters to be copied.

; 문제 해석 (한국어):
; - 기존 Str_copy는 글자 개수를 제한하지 않고 전체를 복사한다.
; - Str_copyN이라는 새 버전을 만들고, "최대 복사 글자 수"를 추가 인자로 받아서 그만큼만 복사하라.

; 풀이:
; - source(ESI), target(EDI), maxCount(ECX)를 사용한다.
; - ECX가 0이 되거나 source에서 NULL이 오면 종료한다.

; 정답 (예시 코드):
Str_copyN PROC
    push esi
    push edi

copyLoop:
    cmp ecx,0
    je done
    mov al,[esi]
    mov [edi],al
    cmp al,0
    je done
    inc esi
    inc edi
    dec ecx
    jmp copyLoop

done:
    pop edi
    pop esi
    ret
Str_copyN ENDP
```

---

## ⭐ Exercise 2 — Str_concat Procedure
```asm
; 문제 원문:
; 2. Str_concat Procedure
; Write a procedure named Str_concat that concatenates a source string to the end of a target string.
; Sufficient space must exist in the target string to accommodate the new characters. Pass pointers to
; the source and target strings. Here is a sample call:

; 문제 해석:
; - target 문자열의 NULL 위치를 먼저 찾은 뒤, 그 뒤부터 source 문자열을 복사한다.

; 풀이:
; 1) target 문자열의 끝(NULL)을 탐색한다.
; 2) 그 위치부터 source 전체를 복사한다.

; 정답 (예시 코드):
Str_concat PROC
    push esi
    push edi

    mov edi,targetPtr

findEnd:
    cmp byte ptr [edi],0
    je startCopy
    inc edi
    jmp findEnd

startCopy:
    mov esi,sourcePtr
copyLoop:
    mov al,[esi]
    mov [edi],al
    cmp al,0
    je done
    inc esi
    inc edi
    jmp copyLoop

done:
    pop edi
    pop esi
    ret
Str_concat ENDP
```

---

## ⭐ Exercise 3 — Str_remove Procedure
```asm
; 문제 원문:
; 3. Str_remove Procedure
; Write a procedure named Str_remove that removes n characters from a string. Pass a pointer to the
; position in the string where the characters are to be removed. Pass an integer specifying the number
; of characters to remove. The following code, for example, shows how to remove “xxxx” from target.

; 문제 해석:
; - 문자열 내부에서 특정 위치부터 n개의 문자를 삭제한다.
; - 삭제 후 나머지 문자열을 앞쪽으로 당기면 된다.

; 풀이:
; - [ptr + n] 이후부터 시작하여 앞으로 shift.

; 정답 (예시 코드):
Str_remove PROC
    ; EDI = pointer to start position
    ; ECX = n (remove count)

shiftLoop:
    mov al,[edi + ecx]
    mov [edi],al
    cmp al,0
    je done
    inc edi
    jmp shiftLoop

done:
    ret
Str_remove ENDP
```

---

## ⭐ Exercise 4 — Str_find Procedure
```asm
; 문제 원문:
; 4. Str_find Procedure
; Write a procedure named Str_find that searches for the first matching occurrence of a source string
; inside a target string and returns the matching position. If a match is found:
;   - Zero flag is set
;   - EAX = pointer to matching position
; Otherwise ZF is clear and EAX is undefined.

; 문제 해석:
; - target 문자열 내부에서 source 문자열이 처음 나타나는 위치를 찾아라.

; 풀이:
; - target[i]마다 source와 비교한다.
; - 완전히 일치하면 EAX가 그 위치를 반환하고 ZF=1 유지.

; 정답 (예시 코드):
Str_find PROC
    mov edi,targetPtr
outerLoop:
    cmp byte ptr [edi],0
    je notFound

    mov esi,sourcePtr
    mov ebx,edi

compareLoop:
    mov al,[esi]
    cmp al,[ebx]
    jne noMatch
    cmp al,0
    je found
    inc esi
    inc ebx
    jmp compareLoop

noMatch:
    inc edi
    jmp outerLoop

found:
    mov eax,edi   ; match position
    ; ZF=1 유지됨
    ret

notFound:
    ; ZF=0 유지
    ret
Str_find ENDP
```

---

## ⭐ Exercise 5 — Str_nextWord Procedure
```asm
; 문제 원문:
; 5. Str_nextWord Procedure
; Finds the first occurrence of a delimiter, replaces it with NULL, and if found:
;   - ZF = 1
;   - EAX = address of the next character
; Otherwise ZF = 0.

; 문제 해석:
; - delimiter 문자를 '\0'로 바꾸고 그 다음 위치를 EAX로 반환한다.

; 풀이:
; - 문자열을 스캔하면서 delimiter를 찾는다.

; 정답 (예시 코드):
Str_nextWord PROC
    mov edi,stringPtr
scanLoop:
    mov al,[edi]
    cmp al,0
    je notFound
    cmp al,delimiter
    je found
    inc edi
    jmp scanLoop

found:
    mov byte ptr [edi],0
    lea eax,[edi+1]
    ret         ; ZF=1

notFound:
    ret         ; ZF=0
Str_nextWord ENDP
```

---

## ⭐ Exercise 6 — Constructing a Frequency Table
```asm
; 문제 원문:
; 6. Constructing a Frequency Table
; Create a procedure named Get_frequencies that counts occurrences of each ASCII character in a
; string, storing the results in a 256-element DWORD array.

; 문제 해석:
; - 문자열을 순회하며 freqTable[문자코드]++ 수행.

; 풀이:
; - al = 문자
; - freqTable[al] 증가

; 정답 (예시 코드):
Get_frequencies PROC
    mov esi,stringPtr

countLoop:
    mov al,[esi]
    cmp al,0
    je done
    movzx ebx,al
    inc DWORD PTR [freqTable + ebx*4]
    inc esi
    jmp countLoop

done:
    ret
Get_frequencies ENDP
```

---

## ⭐ Exercise 7 — Sieve of Eratosthenes
```asm
; 문제 원문:
; 7. Sieve of Eratosthenes
; Create a 65,000-element byte array. Initialize all to 0 using STOSB.
; Mark multiples of each prime (2,3,5,...) as 1. Remaining zeros indicate primes from 2 to 65,000.

; 문제 해석:
; - 65,000 크기 배열을 만들고, 소수 판정 알고리즘을 구현한다.

; 풀이:
; - i = 2부터 시작하여 i*i < N일 동안 반복
; - array[i] == 0 → i는 소수 → i의 배수를 모두 1로 설정

; 정답 (고수준 의사 코드):
; initialize array[0..64999] = 0
; for i = 2 to sqrt(65000):
;   if array[i] == 0:
;       for j = i*i to 64999 step i:
;           array[j] = 1
; primes are positions where array[x] == 0
```

# 🧩 Chapter 9 — 9.10 Programming Exercises (8–14)

---

## ⭐ Exercise 8 — Bubble Sort (Early Exit with Exchange Flag)
```asm
; 문제 원문 (영문 그대로):
; 8. Bubble Sort
; Add a variable to the BubbleSort procedure in Section 9.5.1 that is set to 1 whenever a pair of
; values is exchanged within the inner loop. Use this variable to exit the sort before its normal
; completion if you discover that no exchanges took place during a complete pass through the
; array. (This variable is commonly known as an exchange flag.)

; 문제 해석 (한국어):
; - BubbleSort 내부에서 swap이 일어날 때마다 exchangeFlag = 1 로 설정.
; - 한 사이클 전체에서 교환이 전혀 일어나지 않으면 정렬을 즉시 종료한다.

; 풀이:
; 1) 외부 루프마다 exchangeFlag = 0으로 초기화.
; 2) swap 발생 시 exchangeFlag = 1.
; 3) inner loop가 끝난 뒤 exchangeFlag가 0이면 break.

; 정답 (예시):
BubbleSort PROC
outerLoop:
    mov exchangeFlag,0

innerLoop:
    ; if array[i] > array[i+1], swap them
    ; if swapped → exchangeFlag = 1
    ; ...
    jmp innerLoopDone

innerLoopDone:
    cmp exchangeFlag,0
    je finished
    jmp outerLoop

finished:
    ret
BubbleSort ENDP
```

---

## ⭐ Exercise 9 — Binary Search (Register-Based)
```asm
; 문제 원문:
; 9. Binary Search
; Rewrite the binary search procedure shown in this chapter by using registers for mid, first,
; and last. Add comments to clarify the registers’ usage.

; 문제 해석:
; - mid, first, last를 메모리 대신 레지스터로만 처리하는 이진 탐색을 작성하라.

; 풀이:
; - EBX = first
; - ECX = last
; - EDX = mid
; - 배열 값과 key 비교: == → 찾음, < → 오른쪽, > → 왼쪽

; 정답 (예시 코드):
BinarySearch PROC
    mov ebx,0          ; first
    mov ecx,arraySize-1 ; last

searchLoop:
    mov edx,ebx
    add edx,ecx
    shr edx,1           ; mid = (first+last)/2

    mov eax,[array + edx*4]
    cmp eax,key
    je found
    jl goRight

goLeft:
    mov ecx,edx
    dec ecx
    jmp searchLoop

goRight:
    mov ebx,edx
    inc ebx
    jmp searchLoop

found:
    mov eax,edx
    ret
BinarySearch ENDP
```

---

## ⭐ Exercise 10 — Letter Matrix (4×4 with 50% Vowel Probability)
```asm
; 문제 원문:
; 10. Letter Matrix
; Create a procedure that generates a four-by-four matrix of randomly chosen capital letters.
; When choosing the letters, there must be a 50% probability that the chosen letter is a vowel.
; Write a test program with a loop that calls your procedure five times and displays each matrix
; in the console window.

; 문제 해석:
; - 4×4 대문자 행렬 생성
; - 각 칸마다 50% 확률로 vowel(A,E,I,O,U) 또는 consonant 선택
; - 테스트 프로그램에서 5번 반복 출력

; 풀이:
; 1) Random(0~1) 생산
; 2) 0일 경우 vowel 배열에서 선택
; 3) 1일 경우 consonant 배열에서 선택

; 정답 (요약 예시):
GenerateMatrix PROC
    mov ecx,16       ; 16 letters
fillLoop:
    call RandomRange ; returns 0 or 1
    cmp eax,0
    je pickVowel
pickConsonant:
    ; pick random consonant
    jmp store
pickVowel:
    ; pick random vowel
store:
    mov [matrix + index],al
    inc index
    loop fillLoop
    ret
GenerateMatrix ENDP
```

---

## ⭐ Exercise 11 — Letter Matrix / Sets with Exactly Two Vowels
```asm
; 문제 원문:
; 11. Letter Matrix/Sets with Vowels
; Using the matrix generated in the previous programming exercise as a starting point for this
; program, traverse each matrix row, column, and diagonal, generating sets of letters.
; Display only four-letter sets containing exactly two vowels.

; 문제 해석:
; - 4×4 행렬의 4개 행, 4개 열, 2개 대각선을 탐색
; - 총 10개의 4글자 문자열을 만들고, 모음 개수가 정확히 2개인 경우만 출력

; 풀이:
; - set = {matrix positions}
; - vowelCount == 2 → 출력

; 정답 (예시):
; For each of 10 lines:
;   count vowels
;   if vowelCount == 2:
;       print set
```

---

## ⭐ Exercise 12 — calc_row_sum (Sum Row of 2D Array)
```asm
; 문제 원문:
; 12. Calculating the Sum of an Array Row
; Write a procedure named calc_row_sum that calculates the sum of a single row in a two-dimensional
; array of bytes, words, or doublewords. The procedure should have the following stack parameters:
;   array offset, row size, array type, row index.
; It must return the sum in EAX. Use explicit stack parameters, not INVOKE or extended PROC.

; 문제 해석:
; - 2D 배열에서 특정 row의 합을 계산해 EAX로 반환한다.
; - byte/word/dword 세 종류 모두 처리해야 한다.
; - 스택 인자는 직접 [EBP+xx] 방식으로 가져와야 한다.

; 풀이:
; rowAddr = base + rowIndex * rowSize
; typeSize(b/w/d)에 따라 이동하며 누적 합산

; 정답 (예시 코드):
calc_row_sum PROC
    push ebp
    mov  ebp,esp

    mov  esi,[ebp+12]  ; array offset
    mov  ecx,[ebp+16]  ; row size
    mov  ebx,[ebp+20]  ; array type (1,2,4)
    mov  edx,[ebp+24]  ; row index

    imul edx,ecx
    add  esi,edx       ; row start address

    xor eax,eax

sumLoop:
    ; array element add
    cmp ebx,1
    je readByte
    cmp ebx,2
    je readWord

readDword:
    add eax,[esi]
    add esi,4
    jmp next

readWord:
    movzx edx,WORD PTR [esi]
    add eax,edx
    add esi,2
    jmp next

readByte:
    movzx edx,BYTE PTR [esi]
    add eax,edx
    inc esi

next:
    sub ecx,ebx
    cmp ecx,0
    jne sumLoop

    pop ebp
    ret
calc_row_sum ENDP
```

---

## ⭐ Exercise 13 — Trim Leading Characters
```asm
; 문제 원문:
; 13. Trimming Leading Characters
; Create a variant of the Str_trim procedure that lets the caller remove all instances of a leading
; character from a string. For example, calling it on "###ABC" with '#' results in "ABC".

; 문제 해석:
; - 문자열 앞부분에서 특정 문자가 반복될 경우 모두 제거한다.

; 풀이:
; - 문자열을 시작부터 훑으며 removeChar와 같으면 index++.
; - 이후 전체 문자열을 앞으로 이동시키면 된다.

; 정답 (예시 코드):
Trim_leading PROC
    mov esi,stringPtr
    mov al,removeChar

skipLoop:
    cmp byte ptr [esi],al
    jne shift
    inc esi
    jmp skipLoop

shift:
    mov edi,stringPtr
copyLoop:
    mov bl,[esi]
    mov [edi],bl
    cmp bl,0
    je done
    inc esi
    inc edi
    jmp copyLoop

done:
    ret
Trim_leading ENDP
```

---

## ⭐ Exercise 14 — Trim a Set of Trailing Characters
```asm
; 문제 원문:
; 14. Trimming a Set of Characters
; Create a variant of the Str_trim procedure that lets the caller remove all instances of a set of
; characters from the end of a string. Example:
; "ABC#$&" + filter "%#!;$&*" → "ABC"

; 문제 해석:
; - 문자열 끝부분에서 filterSet에 속하는 문자들을 모두 제거한다.

; 풀이:
; - 문자열 끝에서부터 뒤로 이동하면서 filterSet에 있는 문자면 NULL로 변경.

; 정답 (예시 코드):
Trim_trailingSet PROC
    mov esi,stringPtr

findEnd:
    cmp byte ptr [esi],0
    je check
    inc esi
    jmp findEnd

check:
    dec esi
checkLoop:
    cmp esi,stringPtr
    jb done

    mov al,[esi]
    call IsInFilterSet    ; returns ZF=1 if al in set
    jnz keepChar

removeChar:
    mov byte ptr [esi],0
    dec esi
    jmp checkLoop

keepChar:
    ; stop trimming
    jmp done

done:
    ret
Trim_trailingSet ENDP
```

