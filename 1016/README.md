# 📘 4.9.1 Short Answer 

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

---

# 📘 4.9.2 Algorithm Workbench 

1️⃣ **정수 덧셈**
.data  
val1 DWORD 10000h  
val2 DWORD 20000h  
sum  DWORD ?  
.code  
mov eax,val1  
add eax,val2  
mov sum,eax  
→ EAX=30000h, sum=30000h (단순한 32비트 덧셈)

---

2️⃣ **부호 있는 8비트 더하기**  
.data  
a SBYTE -4  
b SBYTE 10  
sum SBYTE ?  
.code  
mov al,a  
add al,b  
mov sum,al  
→ -4 + 10 = 6, sum=06h, OF=0, CF=0

---

3️⃣ **MOVZX vs MOVSX 차이**  
.data  
bVal BYTE 80h  
.code  
movzx eax,bVal   → EAX=00000080h  
movsx ebx,bVal   → EBX=FFFFFF80h  
→ MOVZX는 제로확장, MOVSX는 부호확장

---

4️⃣ **INC vs ADD 플래그 비교**  
mov eax,0FFFFFFFFh  
inc eax    ; OF=1, CF=0  
mov eax,0FFFFFFFFh  
add eax,1  ; OF=1, CF=1  
→ INC는 CF에 영향 없음, ADD는 CF도 변경

---

5️⃣ **두 WORD 덧셈 후 부호확장**  
.data  
num1 WORD 7FFFh  
num2 WORD 1  
sum  WORD ?  
.code  
mov ax,num1  
add ax,num2  
mov sum,ax  
movsx eax,ax  
→ AX=8000h(−32768), EAX=FFFF8000h, OF=1

---

6️⃣ **NEG: 음수 → 양수 변환**  
mov ax,-10  
neg ax  
→ AX=000Ah(10), CF=1 (0→1 변환 시 캐리 발생)

---

7️⃣ **두 BYTE의 차 구하기**  
.data  
x BYTE 8  
y BYTE 12  
diff BYTE ?  
.code  
mov al,y  
sub al,x  
mov diff,al  
→ 12−8=4, diff=04h, CF=0

---

8️⃣ **OFFSET 연산 (배열 접근)**  
.data  
array WORD 1000h,2000h,3000h  
.code  
mov esi,OFFSET array  
add esi,4  
mov ax,[esi]  
→ AX=3000h (array[2])

---

9️⃣ **PTR 연산으로 WORD 접근**  
.data  
dVal DWORD 12345678h  
.code  
mov ax,WORD PTR dVal  
mov bx,WORD PTR dVal+2  
→ AX=5678h(하위), BX=1234h(상위)

---

🔟 **배열 합계 구하기 (LOOP 사용)**  
.data  
arr DWORD 1,2,3,4  
sum DWORD ?  
.code  
mov ecx,4  
xor eax,eax  
mov esi,OFFSET arr  
L1: add eax,[esi]  
add esi,4  
loop L1  
mov sum,eax  
→ sum=0000000Ah (총합 10)

---

11️⃣ **64비트 MOV 제로확장 확인**  
.data  
dVal DWORD 87654321h  
.code  
mov rax,0FFFFFFFF00000000h  
mov rax,dVal  
→ RAX=0000000087654321h (제로확장됨)

---

12️⃣ **NEG에서 OF=1 케이스**  
mov al,80h  
neg al  
→ AL=80h, OF=1 (−128 → +128 표현 불가)

---

13️⃣ **부호 있는 덧셈 OF 검출**  
mov al,7Fh  
add al,1  
→ AL=80h(−128), OF=1 (양수→음수 전환)

---

14️⃣ **SUB에서 SF 확인**  
mov al,2  
sub al,5  
→ AL=FDh(−3), SF=1

---

15️⃣ **MOVSX / MOVZX 실습**  
.data  
bVal BYTE 0F0h  
.code  
movzx eax,bVal  → EAX=000000F0h  
movsx ebx,bVal  → EBX=FFFFFFF0h  
→ MOVZX는 0확장, MOVSX는 부호확장 (0xF0 = −16)

---

# 📘 4.10 Programming Exercises 

1️⃣ **두 정수의 합 계산 프로그램**
.data  
val1 DWORD 25  
val2 DWORD 30  
sum  DWORD ?  
.code  
mov eax,val1  
add eax,val2  
mov sum,eax  
→ 25+30=55 → sum=00000037h

---

2️⃣ **두 BYTE의 차이 구하기 (음수 포함)**
.data  
x SBYTE 20  
y SBYTE 35  
diff SBYTE ?  
.code  
mov al,x  
sub al,y  
mov diff,al  
→ 20−35=−15 → diff=F1h, SF=1

---

3️⃣ **MOVSX/MOVZX 활용 비교**
.data  
bVal BYTE 0C0h  
.code  
movzx eax,bVal   ; EAX=000000C0h  
movsx ebx,bVal   ; EBX=FFFFFFC0h  
→ MOVZX는 제로확장, MOVSX는 부호확장(−64)

---

4️⃣ **부호 없는 덧셈 시 캐리 검출**
.data  
a BYTE 0F0h  
b BYTE 30h  
sum BYTE ?  
.code  
mov al,a  
add al,b  
mov sum,al  
→ 0xF0+0x30=0x120 → AL=20h, CF=1 (오버플로)

---

5️⃣ **두 WORD의 곱 구하기 (MUL)**
.data  
a WORD 3000h  
b WORD 0002h  
prod DWORD ?  
.code  
mov ax,a  
mul b  
mov prod,dx:ax  
→ AX=6000h, DX=0000h, prod=00006000h

---

6️⃣ **두 부호 있는 WORD의 곱 (IMUL)**
.data  
x SWORD -2  
y SWORD 200  
prod DWORD ?  
.code  
mov ax,x  
imul y  
mov prod,dx:ax  
→ AX=FF38h(−400), DX=FFFFh, prod=FFFFFE70h

---

7️⃣ **나눗셈 (DIV) 예제**
.data  
num DWORD 100  
den BYTE 3  
quot DWORD ?  
rem  BYTE ?  
.code  
mov eax,num  
mov bl,den  
div bl  
mov quot,eax  
mov rem,ah  
→ 몫=33, 나머지=1

---

8️⃣ **부호 있는 나눗셈 (IDIV)**
.data  
num SBYTE -13  
den SBYTE 5  
.code  
mov al,num  
cbw  
idiv den  
→ AL=−2, AH=−3 → 몫=−2, 나머지=−3

---

9️⃣ **배열의 합 구하기 (LOOP)**
.data  
arr DWORD 10,20,30,40  
sum DWORD ?  
.code  
mov ecx,4  
xor eax,eax  
mov esi,OFFSET arr  
L1: add eax,[esi]  
add esi,4  
loop L1  
mov sum,eax  
→ sum=00000064h (100)

---

🔟 **최댓값 찾기**
.data  
arr SBYTE 4,7,2,9,3  
max SBYTE ?  
.code  
mov esi,OFFSET arr  
mov al,[esi]  
mov ecx,4  
L1: inc esi  
cmp al,[esi]  
jge skip  
mov al,[esi]
skip: loop L1  
mov max,al  
→ max=9

---

11️⃣ **음수 개수 세기**
.data  
nums SBYTE -2,5,-7,3,-1  
count BYTE 0  
.code  
mov ecx,5  
mov esi,OFFSET nums  
xor al,al  
xor bl,bl
L1: mov al,[esi]  
inc esi  
cmp al,0  
jge skip  
inc bl
skip: loop L1  
mov count,bl  
→ count=3

---

12️⃣ **배열의 합과 평균**
.data  
arr BYTE 5,10,15,20  
sum WORD ?  
avg BYTE ?  
.code  
mov ecx,4  
xor ax,ax  
mov esi,OFFSET arr  
L1: add al,[esi]  
inc esi  
loop L1  
mov sum,ax  
mov bl,4  
div bl  
mov avg,al  
→ sum=50, avg=12 (정수 나눗셈)

---

13️⃣ **배열 뒤집기**
.data  
arr BYTE 1,2,3,4,5  
.code  
mov esi,OFFSET arr  
mov edi,OFFSET arr+4  
mov ecx,2  
L1: mov al,[esi]  
xchg al,[edi]  
mov [esi],al  
inc esi  
dec edi  
loop L1  
→ arr = 5,4,3,2,1

---

14️⃣ **문자열 길이 세기**
.data  
msg BYTE "HELLO",0  
len BYTE ?  
.code  
mov esi,OFFSET msg  
xor cl,cl  
L1: mov al,[esi]  
cmp al,0  
je done  
inc cl  
inc esi  
jmp L1
done: mov len,cl  
→ len=5

---

15️⃣ **대문자를 소문자로 변환**
.data  
msg BYTE "ABC",0  
.code  
mov esi,OFFSET msg  
L1: mov al,[esi]  
cmp al,0  
je done  
add al,20h  
mov [esi],al  
inc esi  
jmp L1
done:  
→ msg="abc"

