🧩 Chapter 6 — Conditional Processing

🔹 6.10 Review Questions and Exercises

🧠 Q4. Find Maximum Value  
10개의 정수 중 최댓값을 찾는 프로그램.

------------------------------------------------------------
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
------------------------------------------------------------

설명:
- 배열의 첫 번째 값을 기준으로 반복 비교하며 큰 값을 갱신.
- 루프 종료 시 eax에 최댓값이 저장됨.
- 문자열 "Maximum value:" 출력 후 eax의 값을 WriteInt로 출력.

✅ 정답: 배열 내 최댓값을 eax에 저장 후 출력.
------------------------------------------------------------

🧠 Q5. Encrypt/Decrypt Text (XOR)  
입력 문자열을 XOR로 암호화 후 복호화하는 프로그램.

------------------------------------------------------------
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
------------------------------------------------------------

설명:
- KEY 상수를 이용한 XOR 기반 암호화.
- 동일 루틴으로 복호화 가능 (XOR 연산의 대칭성).
- 입력된 문자열을 XOR 변환 → 다시 XOR 변환 시 원문 복원.

✅ 정답: XOR 암호화와 복호화는 동일 루틴으로 수행 가능.
------------------------------------------------------------
