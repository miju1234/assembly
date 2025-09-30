# 📘 Chapter 3.9.1 Short Answer (29문항 풀이)

## 1–10
1. **명령 니모닉 예시?** → `MOV`, `ADD`, `CALL` 같은 어셈블리 명령 이름.  
2. **호출 규약이란?** → 함수 호출 시 인자 전달, 스택 정리 방식 규칙. ASM에서는 `PROTO`, `INVOKE`로 명시.  
3. **스택 공간 예약 방법?** → `.STACK` 지시어 사용 (예: `.STACK 4096`).  
4. **Assembler vs Assembly language 구분?** → Assembler는 프로그램, Assembly language가 언어 이름.  
5. **엔디안 방식 설명?** → 빅엔디안: 상위 바이트가 낮은 주소. 리틀엔디안: 하위 바이트가 낮은 주소.  
6. **기호 상수 사용 이유?** → 코드 가독성↑, 유지보수 쉬움, 오류 감소.  
7. **소스 vs 리스팅 파일?** → 소스(.asm)는 입력 코드, 리스팅(.lst)은 어셈블 결과(주소·기계어·에러).  
8. **데이터 레이블 vs 코드 레이블?** → 데이터는 변수 주소, 코드는 점프 타깃.  
9. **식별자는 숫자로 시작할 수 있나?** → ❌ 불가능.  
10. **16진수 표기 방식?** → `3Ah`가 맞고, `0x3A`는 MASM 표준이 아님.

## 11–20
11. **지시어는 실행 시 동작하나?** → ❌ 어셈블 시 처리됨.  
12. **지시어 대소문자 구분?** → 구분 없음 (`BYTE`, `byte` 동일).  
13. **명령어 구성 요소 4가지?** → 레이블, 니모닉, 피연산자, 주석.  
14. **`MOV`는 무엇?** → 명령 니모닉의 한 예.  
15. **레이블 뒤 콜론 규칙?** → 코드 레이블만 `:` 필요.  
16. **블록 주석 방법?** → `COMMENT ! ... !` 구문.  
17. **숫자 주소 직접 사용 문제점?** → 가독성·재배치 불리. 레이블 사용 권장.  
18. **ExitProcess 인자 타입?** → 32비트 정수 종료 코드 (DWORD).  
19. **프로시저 끝내는 지시어?** → `ENDP`.  
20. **`END` 뒤 식별자 의미?** → 프로그램 진입점 지정.

## 21–29
21. **`PROTO` 목적?** → 프로시저 원형 선언, 인자·호출 규약 정의.  
22. **오브젝트 파일은 누가 생성?** → 어셈블러. 링커는 실행 파일 생성.  
23. **리스팅 파일 생성 주체?** → 어셈블러.  
24. **라이브러리 결합 시점?** → 링커가 실행 파일 생성할 때.  
25. **32비트 부호 있는 정수 지시어?** → `SDWORD`.  
26. **16비트 부호 있는 정수 지시어?** → `SWORD`.  
27. **64비트 부호 없는 정수 지시어?** → `QWORD`.  
28. **8비트 부호 있는 정수 지시어?** → `SBYTE`.  
29. **18–28 요약?** → 각 데이터 크기에 맞는 선언 지시어를 이해했는지 점검 문제.

---

# Ch.3 — 3.9.2 Algorithm Workbench (Q&A)

## Q1. 25를 10진, 2진, 8진, 16진 상수로 정의하라.  
**A1.**  
DEC25 = 25  
BIN25 = 11001b  
OCT25 = 31o  
HEX25 = 19h  

---

## Q2. 프로그램에 `.data`와 `.code`를 여러 번 사용할 수 있는가?  
**A2.** 가능하다. MASM은 같은 세그먼트로 병합한다.

---

## Q3. 더블워드를 빅엔디언 방식으로 메모리에 저장하라.  
**A3.**  
val_be BYTE 12h,34h,56h,78h  

---

## Q4. DWORD 변수에 음수를 저장할 수 있는가?  
**A4.** 가능하다. 2의 보수 값이 그대로 저장된다.  
예: `-5` → `0FFFFFFFBh`

---

## Q5. `add eax,5`와 `add edx,5`는 기계어가 어떻게 다른가?  
**A5.** 두 명령어 모두 즉치 5를 더하지만, ModR/M 바이트가 다르다.  
- `add eax,5` → `83 C0 05`  
- `add edx,5` → `83 C2 05`

---

## Q6. 456789ABh를 리틀엔디언으로 저장하면?  
**A6.** AB 89 67 45

---

## Q7. 부호 없는 DWORD 120개를 미초기화 배열로 선언하라.  
**A7.**  
arr_dw DWORD 120 DUP(?)  

---

## Q8. A~E 다섯 문자를 초기화한 바이트 배열을 선언하라.  
**A8.**  
alpha5 BYTE "ABCDE"  

---

## Q9. 32비트 부호 정수를 최소값으로 초기화하라.  
**A9.**  
i_min SDWORD -2147483648  

---

## Q10. WORD 배열에 3개 정수를 초기화하라.  
**A10.**  
wArray WORD 1000,2000,3000  

---

## Q11. 좋아하는 색상 문자열을 널 종료로 정의하라.  
**A11.**  
favclr BYTE "Blue",0  

---

## Q12. 부호 있는 DWORD 50개 배열을 미초기화로 선언하라.  
**A12.**  
dArray SDWORD 50 DUP(?)  

---

## Q13. "TEST"를 500번 반복하는 문자열을 선언하라.  
**A13.**  
bigstr BYTE 500 DUP("TEST"),0  

---

## Q14. 부호 없는 BYTE 20개를 모두 0으로 초기화하라.  
**A14.**  
bArray BYTE 20 DUP(0)  

---

## Q15. 다음 더블워드 변수를 메모리에 저장했을 때(리틀엔디언), 바이트 순서는?  
val1 DWORD 87654321h
**A15.** 메모리의 낮은 주소부터 높은 주소 순으로:  
21h 43h 65h 87h

---

# Ch.3 — 3.10 Programming Exercises (Q&A + MASM Code)

## Q1. Integer Expression Calculation (★)  
Using the AddTwo program as reference, calculate:  
**A = (A + B) − (C + D)** using registers EAX, EBX, ECX, and EDX.  

**A1.**
INCLUDE Irvine32.inc
.data
A DWORD 10
B DWORD 20
C DWORD 5
D DWORD 3

.code
main PROC
    mov eax, A
    add eax, B
    mov ecx, C
    add ecx, D
    sub eax, ecx
    call WriteInt
    call Crlf
    exit
main ENDP
END main

---

## Q2. Symbolic Integer Constants (★)  
Define symbolic constants for all seven days of the week.  
Create an array variable that uses them as initializers.  

**A2.**
INCLUDE Irvine32.inc
.data
MONDAY    = 1
TUESDAY   = 2
WEDNESDAY = 3
THURSDAY  = 4
FRIDAY    = 5
SATURDAY  = 6
SUNDAY    = 7

weekdays DWORD MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY

.code
main PROC
    mov eax, LENGTHOF weekdays
    call WriteInt
    call Crlf
    exit
main ENDP
END main

---

## Q3. Data Definitions (★★)  
Define one variable for each data type in Table 3-2 (BYTE, WORD, DWORD, SDWORD, REAL4 …)  
and initialize with consistent values.  

**A3.**
INCLUDE Irvine32.inc
.data
bVal  BYTE   127
sbVal SBYTE  -100
wVal  WORD   65000
swVal SWORD  -32000
dwVal DWORD  4000000000
sdwVal SDWORD -2000000000
qVal  QWORD  123456789012345678h
tVal  TBYTE  1234567890123456789012h
r4    REAL4  3.14
r8    REAL8  2.718281828459045
r10   REAL10 1.6180339887

.code
main PROC
    mov eax, dwVal
    call WriteDec
    call Crlf
    exit
main ENDP
END main

---

## Q4. Symbolic Text Constants (★)  
Define symbolic names for string literals and use them in variable definitions.  

**A4.**
INCLUDE Irvine32.inc
.data
HELLO TEXTEQU <"Hello, world!">
BYE   TEXTEQU <"Goodbye!">

msg1 BYTE HELLO,0
msg2 BYTE BYE,0

.code
main PROC
    mov edx, OFFSET msg1
    call WriteString
    call Crlf
    mov edx, OFFSET msg2
    call WriteString
    call Crlf
    exit
main ENDP
END main

---

## Q5. Listing File for AddTwoSum (★★★★)  
Generate a listing file (`.lst`) for **AddTwoSum** and describe machine code bytes.  

**A5.**  
- 실행 명령:  
  ml /c /coff AddTwoSum.asm  
  link /subsystem:console AddTwoSum.obj  

- 예시 해석:  
  - `mov eax, val1` → `A1 ?? ?? ?? ??` (MOV r32,m32)  
  - `add eax, val2` → `03 05 ?? ?? ?? ??` (ADD r32,m32)  
  - `call WriteInt` → `E8 ?? ?? ?? ??` (CALL rel32)  

※ 실제 주소는 환경마다 다름. `.lst`에서 직접 확인 가능.

---

## Q6. AddVariables Program with 64-bit Variables (★★★)  
Modify AddVariables to use 64-bit (`QWORD`) variables.  

**A6.**
INCLUDE Irvine32.inc
.data
val1 QWORD 12345678h
val2 QWORD 87654321h
sum  QWORD ?

.code
main PROC
    ; 32-bit MASM은 QWORD 연산을 직접 지원하지 않아 오류 발생
    ; 해결: (1) x64 MASM 사용, (2) 상·하위 DWORD 분리 계산
    mov eax, DWORD PTR val1
    add eax, DWORD PTR val2
    mov DWORD PTR sum, eax

    mov eax, DWORD PTR sum
    call WriteInt
    call Crlf
    exit
main ENDP
END main

**설명:**  
- 32-bit 환경에서는 QWORD 덧셈이 불완전하므로, 상·하위 DWORD로 나눠야 한다.  
- 64-bit MASM 환경에서는 `mov rax, qword ptr val1` 같은 명령으로 바로 가능하다.
