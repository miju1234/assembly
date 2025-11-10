## 🧠 Q6. AddVariables Program with 64-bit Variables (★★★)

Modify AddVariables to use 64-bit (QWORD) variables.

```asm
INCLUDE Irvine32.inc
.data
val1 QWORD 12345678h
val2 QWORD 87654321h
sum  QWORD ?
.code
main PROC
  mov eax, DWORD PTR val1
  add eax, DWORD PTR val2
  mov DWORD PTR sum, eax
  mov eax, DWORD PTR sum
  call WriteInt
  call Crlf
  exit
main ENDP
END main
