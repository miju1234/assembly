---


# Assembly Language — Basic Concepts

## 1. 어셈블리와 저수준 언어
- **어셈블러(Assembler)**: 사람이 읽는 어셈블리 코드를 기계어로 변환  
- **링커(Linker)**: 여러 오브젝트 파일을 묶어 실행 파일 생성  
- **MASM**으로 작성할 수 있는 프로그램: 시스템 프로그래밍, 하드웨어 제어, 성능 최적화 프로그램 등

## 2. 가상 머신 개념
- **하드웨어 계층**: CPU, 메모리, 레지스터 → 0과 1을 전기 신호로 처리  
- **기계어(Machine language)**: CPU가 직접 실행 가능한 이진 명령  
  - 예) `10110000 01100001` → AL 레지스터에 97 저장  
- **어셈블리어(Assembly language)**: 기계어를 사람이 읽기 쉽게 표현  
  - 예) `MOV AL, 97`

## 3. 데이터 표현 (Data Representation)
- **수 체계 변환**: 2진수, 10진수, 16진수 상호 변환  
- **정수, 문자 저장 방식** 이해 필요  

### 문자 인코딩
- **ASCII (7비트, 128자)**: 영문자, 숫자, 기호, 제어문자  
- **확장 ASCII (8비트, 256자)**: 추가 기호/유럽 언어 일부 지원  
- **Unicode**: 전 세계 문자 지원 (UTF-8, UTF-16, UTF-32)  
  - UTF-8: 1~4바이트, 웹 표준, ASCII 호환  
  - UTF-16: 2/4바이트, Windows/Java 사용  
  - UTF-32: 4바이트 고정, 단순하지만 메모리 비효율적

## 4. 불 표현식 (Boolean Expressions)
- 불 대수(AND, OR, NOT) 기반  
- 디지털 회로 및 조건 연산의 핵심  
- 드모르간 법칙: ¬(A ∨ B) ≡ (¬A ∧ ¬B)

---


**Kip Irvine, Assembly Language for x86 Processors (7th Edition)** 교재
## 1.7.1 Short Answer (28문제)

---


1. **8비트 이진수의 MSB**  
   → 가장 왼쪽 비트 (비트 7)

2. **부호없는 이진수 → 10진수**  
   - 00110101 = 53  
   - 10010110 = 150  
   - 11001100 = 204

3. **이진수 덧셈**  
   - 10101111 + 11011011 = 1 1000 1010 (0x18A = 394)  
   - 10010111 + 11111111 = 1 1001 0110 (0x196 = 406)  
   - 01110101 + 10101100 = 1 0010 0001 (0x121 = 289)

4. **이진수 뺄셈**  
   - 00001101 − 00000111 = 00000110 (6)

5. **자료형 비트 수**  
   - word = 16비트  
   - doubleword = 32비트  
   - quadword = 64비트  
   - double quadword = 128비트

6. **주어진 10진수를 표현하는 최소 비트 수**  
   - 4095 → 12비트  
   - 65534 → 16비트  
   - 42319 → 16비트

7. **이진수 → 16진수**  
   - 0011 0101 1101 1010 → 0x35DA  
   - 1100 1110 1010 0011 → 0xCEA3  
   - 1111 1110 1101 1011 → 0xFEDB

8. **16진수 → 이진수**  
   - 0126F9D4 → 0000 0001 0010 0110 1111 1001 1101 0100  
   - 6ACDFA95 → 0110 1010 1100 1101 1111 1010 1001 0101  
   - F69BDC2A → 1111 0110 1001 1011 1101 1100 0010 1010

9. **16진수(부호없음) → 10진수**  
   - 3A → 58  
   - 1BF → 447  
   - 1001 → 4097

10. **16진수(부호없음) → 10진수**  
    - 62 → 98  
    - 4B3 → 1203  
    - 29F → 671

---


11. **부호있는 10진수 → 16비트 2의 보수(16진)**  
    - −24 → FFE8  
    - −331 → FEB5

12. **부호있는 10진수 → 16비트 2의 보수(16진)**  
    - −21 → FFEB  
    - −45 → FFD3

13. **16진수(16비트, 부호있음) → 10진수**  
    - 6BF9 → 27641  
    - C123 → −16093

14. **16진수(16비트, 부호있음) → 10진수**  
    - 4CD2 → 19666  
    - 8230 → −32208

15. **부호있는 8비트 이진수 → 10진수**  
    - 10110101 → −75  
    - 00101010 → 42  
    - 11110000 → −16

16. **부호있는 8비트 이진수 → 10진수**  
    - 10000000 → −128  
    - 11001100 → −52  
    - 10110111 → −73

17. **부호있는 10진수 → 8비트 2의 보수(이진)**  
    - −5 → 11111011  
    - −42 → 11010110  
    - −16 → 11110000

18. **부호있는 10진수 → 8비트 2의 보수(이진)**  
    - −72 → 10111000  
    - −98 → 10011110  
    - −26 → 11100110

19. **16진수 덧셈**  
    - 6B4 + 3FE = 0xAB2  
    - A49 + 6BD = 0x1106

20. **16진수 덧셈**  
    - 7C4 + 3BE = 0xB82  
    - B69 + 7AD = 0x1316

---


21. **ASCII ‘B’**  
    - 16진: 0x42  
    - 10진: 66

22. **ASCII ‘G’**  
    - 16진: 0x47  
    - 10진: 71

23. **129비트 부호없는 정수의 최댓값**  
    - 2^129 − 1

24. **86비트 부호있는 정수의 최댓값**  
    - 2^85 − 1

25. **불 함수 ¬(A ∨ B) 진리표**

| A | B | A∨B | ¬(A∨B) |
|---|---|-----|--------|
| 0 | 0 |  0  | 1 |
| 0 | 1 |  1  | 0 |
| 1 | 0 |  1  | 0 |
| 1 | 1 |  1  | 0 |

26. **불 함수 (¬A ∧ ¬B) 진리표**  
   - 출력 값이 25번과 동일 → **드모르간 법칙 성립** (¬(A∨B) ≡ ¬A ∧ ¬B)

27. **입력 4개 불 함수의 진리표 행 수**  
   - 2^4 = 16

28. **4-입력 멀티플렉서의 선택자 비트 수**  
   - 2비트 (2^n = 4 → n=2)

---

# Chapter 1 — 1.7.2 Algorithm Workbench (Solutions)

> Kip Irvine, *Assembly Language for x86 Processors, 7th Ed.*  
> 9 Problems solved in C++ (no sprintf/sscanf 등 자동 변환 함수 사용)

```cpp
#include <bits/stdc++.h>
using namespace std;

int val(char c){ return isdigit(c)?c-'0':10+(toupper(c)-'A'); }
char dig(int v){ return v<10?'0'+v:'A'+v-10; }

// 1) 16-bit Binary String → Integer
uint32_t parseBin16(string s){ uint32_t x=0; for(char c:s) x=(x<<1)+(c-'0'); return x; }

// 2) 32-bit Hex String → Integer
uint32_t parseHex32(string s){ uint32_t x=0; for(char c:s) x=(x<<4)+val(c); return x; }

// 3) Integer → Binary String
string toBin(uint32_t n){ if(!n) return "0"; string r; while(n){ r.push_back((n&1)+'0'); n>>=1; } reverse(r.begin(),r.end()); return r; }

// 4) Integer → Hex String
string toHex(uint32_t n){ if(!n) return "0"; string r; while(n){ r.push_back(dig(n&15)); n>>=4; } reverse(r.begin(),r.end()); return r; }

// 5) Add Two Base-b Strings (2≤b≤10)
string addBase(string A,string B,int b){ int i=A.size()-1,j=B.size()-1,c=0; string r;
  while(i>=0||j>=0||c){ int x=i>=0?A[i--]-'0':0, y=j>=0?B[j--]-'0':0; int s=x+y+c; r.push_back(dig(s%b)); c=s/b; }
  reverse(r.begin(),r.end()); return r;
}

// 6) Add Two Hex Strings
string addHex(string A,string B){ int i=A.size()-1,j=B.size()-1,c=0; string r;
  while(i>=0||j>=0||c){ int x=i>=0?val(A[i--]):0, y=j>=0?val(B[j--]):0; int s=x+y+c; r.push_back(dig(s%16)); c=s/16; }
  reverse(r.begin(),r.end()); return r;
}

// 7) Multiply Hex Digit × Hex String
string mulHex(char d,string S){ int m=val(d),c=0; string r;
  for(int i=S.size()-1;i>=0;--i){ int p=val(S[i])*m+c; r.push_back(dig(p%16)); c=p/16; }
  if(c) r.push_back(dig(c)); reverse(r.begin(),r.end()); return r;
}

// 8) Java Disassembly (설명)
//   int Y; int X = (Y + 4) * 3;
//   → javap -c 결과에서: iload, iconst_4, iadd, iconst_3, imul, istore 등으로 분해됨.
//   각 바이트코드 = 변수 로드, 상수 적재, 산술연산, 결과 저장.


// 9) Unsigned Binary Subtraction
// 방법: 보수법(2의 보수) 또는 자리 내림 빼기
string subBin(string A,string B){ // assume A>=B
  int i=A.size()-1,j=B.size()-1,borrow=0; string r;
  while(i>=0){ int x=A[i]-'0'-borrow; int y=j>=0?B[j--]-'0':0; 
    if(x<y){ x+=2; borrow=1; } else borrow=0;
    r.push_back((x-y)+'0'); i--; }
  while(r.size()>1 && r.back()=='0') r.pop_back();
  reverse(r.begin(),r.end()); return r;
}

int main(){
  cout<<parseBin16("1111000011110000")<<"\n";     // 0xF0F0
  cout<<parseHex32("1A2B3C")<<"\n";              // 0x1A2B3C
  cout<<toBin(42)<<" "<<toHex(255)<<"\n";        // 101010 FF
  cout<<addBase("1011","1101",2)<<"\n";          // 11000
  cout<<addHex("FFFF","1")<<"\n";                // 10000
  cout<<mulHex('A',"FF")<<"\n";                  // 9F6
  cout<<subBin("10001000","00000101")<<"\n";     // 10000011
}


