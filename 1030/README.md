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
