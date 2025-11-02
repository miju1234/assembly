## 5.8.1 Short Answer (1~20)

---

① **스택의 역할**  
- LIFO 구조 (Last In, First Out).  
- `CALL` → 반환주소 push, `RET` → pop 후 복귀.  
- PUSH 시 ESP 감소, POP 시 ESP 증가.  

---

② **CALL vs RET**  
- `CALL`: 다음 명령 주소(반환주소)를 스택에 넣고 분기.  
- `RET`: 스택에서 반환주소를 꺼내 EIP로 로드.  

---

③ **인자 전달(레지스터 vs 스택)**  
- 인자 적을 때: 레지스터(EAX, EBX, ECX 등)  
- 많거나 가변 길이: 스택 이용  

---

④ **반환값 레지스터**  
- EAX 사용(관례).  
- 반환 이후 EAX를 pop으로 복원하면 결과 손실 가능.  

---

⑤ **레지스터 보존 규칙**  
- 피호출자 보존: EBX, ESI, EDI, EBP  
- 호출자 보존: EAX, ECX, EDX  

---

⑥ **USES 키워드**  
- `PROC USES esi edi ecx` → 자동 push/pop 생성.  

---

⑦ **중첩 호출 시 루프 카운터 보존**  
- `push ecx` → `call inner` → `pop ecx`  

---

⑧ **JMP vs CALL**  
- JMP: 단순 분기(복귀 불가)  
- CALL: 반환주소 저장 후 분기 → RET 복귀 가능  

---

⑨ **스택 임시 사용 시 주의**  
- push/pop 개수 균형 유지.  
- 반환주소 파괴 시 복귀 실패.  

---

⑩ **지역 변수 공간 확보/해제**  
- `sub esp, 8` → 8바이트 확보  
- `add esp, 8` → 해제  

---

⑪ **로컬 DWORD 두 개 초기화**  
- `sub esp, 8`  
- `[esp] = 1000h`, `[esp+4] = 2000h`  
- `add esp, 8`  

---

⑫ **인덱스/간접 주소 지정 예시**  
- `mov eax, [edi+esi]`  
- `mov [edi+esi-4], eax`  (TYPE DWORD = 4)  

---

⑬ **반환주소 출력(스택 보존)**  
- `push eax`  
- `mov eax, [esp+4]`  
- (EAX 출력)  
- `pop eax`  

---

⑭ **반환주소 조작(주의)**  
- `add dword ptr [esp], 3`  
- `ret`  
- 교육용 예시 외 사용 금지.  

---

⑮ **Irvine32 라이브러리 호출 규칙**  
- WriteString: EDX = 문자열 주소  
- WriteInt / WriteHex: EAX = 출력 값  

---

⑯ **EFLAGS 저장/복원**  
- `pushfd` … `popfd`  

---

⑰ **PUSHAD / POPAD**  
- PUSHAD: EAX, ECX, EDX, EBX, ESP(실행 전), EBP, ESI, EDI 순서 저장  
- POPAD : 역순 복원  

---

⑱ **ArraySum 류의 보존 예시**  
- `push esi` / `push ecx` → 루프 → `pop ecx` / `pop esi`  
- 반환값: EAX  


---

⑲ **선택형 — 다음 코드 실행 후 main 복귀 직전의 EDX 값은?**

; main  
mov edx,0  
mov eax,40  
push eax  
call Ex5Sub  
; 여기서 EDX = ?  

Ex5Sub PROC  
  pop  eax  
  pop  edx  
  push eax  
  ret  
Ex5Sub ENDP  

🧠 **풀이**  
- CALL 시 스택(top=RA, 아래=40)  
- pop eax → EAX=RA  
- pop edx → EDX=40  
- push eax → RA 복구  
- ret → 정상 복귀  

✅ **정답:** EDX = 40  

---

⑳ **배열에 기록되는 값 (초기 EAX=10, ESI=0)**

; main  
mov eax,10  
mov esi,0  
call proc_1  
add  esi,4  
add  eax,10  
mov  array[esi],eax     ; array[12] = 40  

proc_1:  
  call proc_2  
  add  esi,4  
  add  eax,10  
  mov  array[esi],eax   ; array[8]  = 30  
  ret  

proc_2:  
  call proc_3  
  add  esi,4  
  add  eax,10  
  mov  array[esi],eax   ; array[4]  = 20  
  ret  

proc_3:  
  mov  array[esi],eax   ; array[0]  = 10  
  ret  

🧠 **풀이**  
- proc_3 → array[0] = 10  
- proc_2 → ESI+4, EAX+10 → array[4] = 20  
- proc_1 → ESI+4, EAX+10 → array[8] = 30  
- main  → ESI+4, EAX+10 → array[12] = 40  

✅ **정답:** array = [10, 20, 30, 40]

---

## 5.8.2 Algorithm Workbench (①~⑤)

---

① **EAX와 EBX의 값을 PUSH/POP만으로 교환하라.**

mov eax, 1111h  
mov ebx, 2222h  
push eax  
push ebx  
pop  eax     ; EAX ← EBX  
pop  ebx     ; EBX ← EAX  

🧠 **풀이**  
- push/pop 두 쌍으로 레지스터 내용을 스택에 저장 후 역순 복원  
- 임시 레지스터 없이 교환 가능  

---

② **RET 직전, 반환 주소를 현재 위치보다 3바이트 이후로 이동시켜라.**

add dword ptr [esp], 3  
ret  

🧠 **풀이**  
- [ESP]에 저장된 반환주소에 3을 더함  
- 반환 시 3바이트 뒤 명령부터 실행됨  
⚠️ 교육용 예제, 실무 코드에서는 위험  

---

③ **로컬 변수 두 개(DWORD)를 확보하고 초기화하라.**

sub esp, 8  
mov dword ptr [esp],   1000h  
mov dword ptr [esp+4], 2000h  
add esp, 8  

🧠 **풀이**  
- `sub esp,8` → 스택에 8바이트 확보  
- [esp]와 [esp+4]로 각각 접근  

---

④ **배열의 array[i] 값을 array[i-1]로 복사하라.**

mov esi, TYPE DWORD * i       ; i번째 인덱스  
mov eax, [array + esi]        ; array[i]  
mov [array + esi - TYPE DWORD], eax  ; array[i-1] = array[i]  

🧠 **풀이**  
- 인덱스 레지스터 기반 주소 지정  
- TYPE 키워드로 요소 크기(4바이트) 자동 계산  

---

⑤ **프로시저 내부에서 반환 주소를 출력하라.**

push eax  
mov  eax, [esp+4]     ; (push로 인해 RA는 esp+4 위치)  
call WriteHex  
pop  eax  
ret  

🧠 **풀이**  
- 반환주소를 읽기만 하고 스택은 건드리지 않음  
- RA를 pop하지 않아 정상적으로 RET 복귀 가능  

---

## 5.9 Programming Exercises (①~⑪)

---

① **Draw Text Colors**  
화면에 동일한 문자열을 네 가지 색상으로 반복 출력하라.  
`SetTextColor` 프로시저를 사용해 전경색을 변경한다.  

🧠 **풀이**  
- `include Irvine32.inc`  
- 루프를 사용해 색상 바꾸며 `WriteString` 호출  
- `call SetTextColor`, `call Crlf` 반복  

---

② **Linking Array Items**  
문자 배열(`chars`)과 링크 배열(`links`)을 이용해 연결 리스트를 순서대로 따라가며  
문자를 새로운 배열에 복사하라.  

🧠 **풀이**  
- 시작 인덱스 `start`에서 링크를 따라 이동  
- `mov esi, OFFSET chars`  
- `mov edi, OFFSET output`  
- `mov ecx, LENGTHOF chars`  
- `while (next != 0)` 루프로 복사  

---

③ **Simple Addition (1)**  
화면 중앙 근처에 커서를 위치시키고, 두 정수를 입력받아 합계를 출력하는 프로그램을 작성하라.  

🧠 **풀이**  
- `call Clrscr`, `call Gotoxy`  
- `call ReadInt` 두 번 → EAX, EBX에 저장  
- `add eax, ebx`, `call WriteInt`  

---

④ **Simple Addition (2)**  
위 프로그램을 기반으로 하여 **세 번 반복**하도록 수정하라. 각 루프마다 화면을 지운다.  

🧠 **풀이**  
- `mov ecx,3`  
- `L1:` → 덧셈 실행 → `call WaitMsg` → `call Clrscr`  
- `loop L1`  

---

⑤ **BetterRandomRange Procedure**  
기존 `RandomRange`를 개선하여 **M~N-1 범위**의 난수를 생성하는  
`BetterRandomRange`를 작성하라.  

🧠 **풀이**  
- `EAX=N`, `EBX=M`  
- `sub eax, ebx` → 범위 계산  
- `call RandomRange` → 결과에 EBX 더하기  
- 반환값 = EBX + RandomRange(N−M)  

---

⑥ **Random Table Generation**  
0~99 난수를 10개 생성하여 배열에 저장하고 출력하라.  

🧠 **풀이**  
- `mov ecx,10`  
- 루프 안에서 `call RandomRange`, `mov [array+esi], eax`  
- 루프 종료 후 `DumpMem`으로 배열 출력  

---

⑦ **Factorial Procedure**  
입력받은 정수 n의 팩토리얼 n!을 계산하라.  

🧠 **풀이**  
- `mov ecx,n`, `mov eax,1`  
- `L1: imul eax, ecx`, `loop L1`  
- `call WriteDec`  

---

⑧ **Array Average**  
사용자로부터 N개의 정수를 입력받아 배열에 저장하고, 평균을 계산해 출력하라.  

🧠 **풀이**  
- `ReadInt`로 입력 반복  
- 합계 누적 → `div ecx`로 평균 계산  
- `WriteInt`로 출력  

---

⑨ **Reverse String**  
문자열을 입력받아 역순으로 뒤집어 출력하라.  

🧠 **풀이**  
- `ReadString`으로 입력  
- 포인터 앞뒤 교환(`xchg al, [edi]`)  
- `loop`로 중간까지 반복  

---

⑩ **Display Hex Table**  
0~255까지 정수를 16진수로 표시하는 표를 출력하라.  

🧠 **풀이**  
- `mov ecx,256`  
- `call WriteHex`, `call Crlf`  
- 16열마다 줄바꿈 처리  

---

⑪ **Colorful Pattern Output**  
여러 색상의 문자 패턴을 화면에 출력하라.  

🧠 **풀이**  
- `mov ecx, length`  
- 루프 내 `SetTextColor`, `WriteChar`  
- 배경색/전경색 번갈아 변경  

---

