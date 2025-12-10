# Assembly + C 결합 프로젝트 (정적 라이브러리 · DLL · C 연동)

---

## 1. Assembly 단독 실행 프로그램 (.exe)

- main.asm 내부에서 push 10, push 20 전달
- AddTwoNums 함수 실행
- Irvine32 라이브러리로 결과 출력

### 📄 main.asm

```asm
INCLUDE Irvine32.inc

EXTERN AddTwoNums:PROC   ; 정적 라이브러리 함수 참조

.data
msg BYTE "Result from static library = ",0

.code
main PROC
    push 20
    push 10
    call AddTwoNums
    add esp, 8

    mov edx, OFFSET msg
    call WriteString
    call WriteDec
    call Crlf

    exit
main ENDP

END main
```

---

## 2. 정적 라이브러리(.lib) 생성

AddTwoNums.asm 파일을 정적 라이브러리(AddLib.lib) 형태로 생성하여  
AsmExe 프로그램에서 링크하여 사용한다.

### 📄 AddTwoNums.asm (정적 라이브러리 & DLL 공통)

```asm
.386
.model flat, stdcall
.stack 4096

PUBLIC AddTwoNums

.code
AddTwoNums PROC
    mov eax, [esp+4]      ; 첫 번째 정수
    add eax, [esp+8]      ; 두 번째 정수
    ret 8                 ; stdcall 호출 규약
AddTwoNums ENDP

END
```

---

## 3. DLL 프로젝트 생성

동일한 AddTwoNums.asm을 DLL로 빌드하여  
C 프로그램에서 동적으로 불러 사용할 수 있도록 구성한다.

### 🔹 DLL Export 설정 (DEF 파일 사용)

### 📄 AsmDll.def

```def
LIBRARY AsmDll
EXPORTS
    AddTwoNums
```

---

## 4. C 프로그램에서 DLL 호출

C 실행 파일(CDllExe)은 LoadLibrary / GetProcAddress 함수를 사용하여  
Assembly로 작성된 DLL의 AddTwoNums 함수를 호출한다.

### 📄 main.c

```c
#include <stdio.h>
#include <windows.h>

typedef int (__stdcall *AddTwoNumsFunc)(int, int);

int main(void)
{
    HMODULE hDll = LoadLibraryA("AsmDll.dll");
    if (!hDll) {
        printf("DLL load failed\n");
        return 1;
    }

    AddTwoNumsFunc AddTwoNums =
        (AddTwoNumsFunc)GetProcAddress(hDll, "AddTwoNums");

    if (!AddTwoNums) {
        printf("Function not found\n");
        return 1;
    }

    int result = AddTwoNums(10, 20);
    printf("Result from DLL (ASM) = %d\n", result);

    FreeLibrary(hDll);
    return 0;
}
```

---

## 📌 프로젝트 전체 구조

```
Project1
 ├─ AddLib
 │   └─ AddTwoNums.asm
 ├─ AsmExe
 │   └─ main.asm
 ├─ AsmDll
 │   ├─ AddTwoNums.asm
 │   └─ AsmDll.def
 └─ CDllExe
     └─ main.c
```


