# 📘 4.9.1 Short Answer — 간단 풀이 (필수 설명 포함)

1️⃣ **MOVSX로 WORD 부호확장**
movsx edx,one ; (a)
movsx edx,two ; (b)
- (a) EDX=FFFF8002h → 0x8002는 음수이므로 부호확장됨  
- (b) EDX=00004321h → 0x4321은 양수라 상위는 0으로 채워짐

---

2️⃣ **INC AX (하위 16비트만 증가)**  
mov eax,1002FFFFh  
inc ax  
- FFFF→0000으로 오버플로, 상위 16비트 그대로 → EAX=10020000h

---

3️⃣ **DEC AX (하위 16비트만 감소)**  
mov eax,30020000h  
dec ax  
- 0000→FFFF로 감소 → EAX=3002FFFFh

---

4️⃣ **NEG AX (부호 반전)**  
mov eax,1002FFFFh  
neg ax  
- FFFFh → 0001h로 바뀜 → EAX=10020001h

---

5️⃣ **Parity Flag (짝수비트 여부)**  
mov al,1  
add al,3  
- AL=04h (1비트만 1) → 홀수비트 → PF=0

---

6️⃣ **SUB 결과와 Sign Flag**  
mov eax,5  
sub eax,6  
- 결과 EAX=FFFFFFFFh (−1) → 부호비트 1 → SF=1

---

7️⃣ **Overflow Flag의 한계**  
mov al,-1  
add al,130  
- 음수+음수→결과도 음수 → OF=0  
- OF만으론 “+130 범위 초과”를 판정할 수 없음

---

8️⃣ **64비트 즉시수 이동**  
mov rax,44445555h  
- 32비트 즉시수는 부호확장 → RAX=0000000044445555h

---

9️⃣ **32비트 메모리→RAX 이동**  
mov rax,dwordVal  
- r/m32→r64 이동은 제로확장 → RAX=0000000084326732h

---

🔟 **상위 WORD 교체**  
mov ax,3  
mov WORD PTR dVal+2,ax  
mov eax,dVal  
- 상위 워드 1234h→0003h로 교체 → EAX=00035678h

---

11️⃣ **상위WORD 읽어 더한 후 하위WORD에 저장**  
mov  ax,WORD PTR dVal+2  
add  ax,3  
mov  WORD PTR dVal,ax  
mov  eax,dVal  
- 상위(1234h)+3=1237h → 하위로 복사 → EAX=12341237h

---

12️⃣ (+)+(−)은 OF 발생? → ❌  
13️⃣ (−)+(−)결과 양수면 OF? → ✅  
14️⃣ NEG가 OF 세움? (최솟값 시) → ✅  
15️⃣ SF와 ZF 동시=1 가능? → ❌ (0은 부호비트 0)

---

16️⃣ **명령어 유효성 (✅/❌)**  
|문장|결과|이유|
|----|----|----|
|mov ax,var1|❌|BYTE→WORD 크기 불일치|
|mov ax,var2|✅|WORD→AX 유효|
|mov eax,var3|❌|WORD→EAX 직접불가|
|mov var2,var3|❌|메모리↔메모리 불가|
|movzx ax,var2|❌|src는 8비트여야 함|
|movzx var2,al|❌|목적지는 레지스터만 가능|
|mov ds,ax|❌|보호모드에서 세그먼트 변경 불가|
|mov ds,1000h|❌|세그먼트레지스터에 즉시수 불가|

---

17️⃣ **var1 참조**
mov al,var1     ; AL=FCh (−4)  
mov ah,[var1+3] ; AH=01h (+1)

---

18️⃣ **var2, var3 참조**
mov ax,var2        ; 1000h  
mov ax,[var2+4]    ; 3000h  
mov ax,var3        ; FFF0h  
mov ax,[var3-2]    ; 4000h

---

19️⃣ **var4 참조**
mov   edx,var4       ; 1  
movzx edx,var2       ; 1000h  
mov   edx,[var4+4]   ; 2  
movsx edx,var1       ; FFFFFFFCh (−4)
