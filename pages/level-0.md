# Niveau 0

Base du langage C

---
transition: slide-left
layout: image-right
image: /snippets/compilation.png
---
## Example de base

```cpp
#include <stdio.h>

#define PI 3.14159f;
#define SQUARE(x) ((x)*(x))

int main(int argc, const char *argv[]) {
    // Types de base
    unsigned int entier = 42;
    const float gravite = 9.81f;

    // Affichage des variables
    printf("PI = %.2f\n", PI);
    printf("PI^2 = %.2f\n", SQUARE(PI));
    printf("G = %.2f\n", gravite);

    // Calcul utilisant la macro SQUARE
    float rayon = 2.5f;
    float aire_cercle = PI * SQUARE(rayon);

    return 0;
}
```

---
transition: slide-left
---
## Directives du preprocesseur
- Guide le preprocesseur avant la compilation
- Examples
  - #include -> ajoute le contenue d'un autre fichier
  - #define / #undef -> Defini ou supprime une macros
  - #ifdef, #ifndef, #endif -> Compilation conditionel
  - #error "message" -> Erreur a la compilation
- Macro existante
  - \_\_FILE\_\_ \_\_LINE\_\_ \_\_DATE\_\_ \_\_TIME\_\_ \_\_TIMESTAMP\_\_
- Macro operators:
  - \# operator: Stringize -> transforme en chaine de charactere
  - \#\# operator: Token concatenation -> coller des chaines de charactere
- Directive de compilation
  - \#pragma warning (disable : 4018 )

---
transition: fade-out
---
```cpp
#include <stdio.h>
#include <stdlib.h>

#define PI 3.14159
#define SQUARE(x) ((x) * (x))
#define MAX(a, b) ((a) > (b) ? (a) : (b))
#define min(...) __START(MIN, __VA_ARGS__, 4, 3, 2, 1)
#define max(...) __START(MAX, __VA_ARGS__, 4, 3, 2, 1)

#define DEBUG

#ifdef DEBUG
    #define LOG(msg) printf("DEBUG: %s\n", msg)
#else
    #define LOG(msg)
#endif

#ifndef BUFFER_SIZE
    #define BUFFER_SIZE 1024
#endif

#define CONCATENATE(a, b) a##b
#define STRINGIFY(x) #x

#pragma warning(disable : 4996)  // Disable specific warning (MSVC)
```

<!--
https://rextester.com/l/c_online_compiler_gcc
-->

---
transition: slide-left
---
## Declaration
- <span style="color: green">Variables</span>
- Fonction -> signature
- Type
  - structure -> groupe de donnes
  - unions -> donnees paratagees
  - enums -> label

---
transition: slide-left
---

Types de base :
- Taille depend de l'os et du materiel
- char (1), short (2), int (2-4), long(4-8)
- float(4), double(8), long double (8-16)
- long long (8) (C99)
- void

Type specifique :
- size_t(4-8), ptrdiff_t(4-8), intptr_t(4-8), wchar_t(2-4), __m128(16)

Modificateur :
- signed, unsigned, const
- static, register

Enumeration


<!--
https://rextester.com/l/c_online_compiler_gcc
-->

---
transition: fade-out
---
```cpp
enum Days {
    MONDAY, TUESDAY, WEDNESDAY,
    THURSDAY, FRIDAY, SATURDAY, SUNDAY
};

typedef unsigned long long int uint64;
```

```cpp
const float PI = 3.14159f;
unsigned int i;
i = 15;
```

```asm
section .data
    PI: dd 3.14159       ; Define PI as a 32-bit float constant

section .text
global _start

_start:
    ; Initialize i to 15
    mov dword [i], 15    ; Store 15 in the memory location for i

section .bss
    i: resd 1            ; Reserve 4 bytes (32 bits) for unsigned int i
```

---
transition: slide-left
---
## Expression

- on manipule les variables via des operateurs
- on combine des variables et des constantes -> resultat

```cpp
    int a = 10, b = 5;

    // Arithmetic expressions
    printf("a + b = %d\n", a + b);
    printf("a - b = %d\n", a - b);
    printf("a * b = %d\n", a * b);
    printf("a / b = %d\n", a / b);
    printf("a %% b = %d\n", a % b);
    printf("a++ = %d\n", a++);
    printf("--b = %d\n", --b);

    // Logical expressions
    printf("a > b: %d\n", a > b); // True = 1
    printf("a < b: %d\n", a < b); // False = 0
    printf("a >= b: %d\n", a >= b);
    printf("a <= b: %d\n", a <= b);
    printf("a == b: %d\n", a == b);
    printf("a != b: %d\n", a != b);

    
```

---
transition: slide-left
---
```cpp
    // C89
    typedef unsigned int bool;
    #define true 1
    #define false 0

    // Logical expressions
    bool p = true, q = false;
    printf("\nLogical Expressions:\n");
    printf("p && q: %d\n", p && q);
    printf("p || q: %d\n", p || q);
    printf("!p: %d\n", !p);

    // Mask expression
    int a = 1, b = 2;
    printf("a & b: %d\n", a & b); // 0
    printf("a | b: %d\n", a | b); // 3
    printf("a << b: %d\n", a << 1); // 2
    printf("a >> b: %d\n", b >> 1); // 1
    printf("a >> b: %d\n", a ^ b); // 3

    // Assignment expressions
    int c = 15;
    printf("\nAssignment Expressions:\n");
    printf("c += 5: %d\n", c += 5);
    printf("c -= 3: %d\n", c -= 3);
    printf("c *= 2: %d\n", c *= 2);
    printf("c /= 4: %d\n", c /= 4);
    printf("c %%= 3: %d\n", c %= 3);
```
---
transition: fade-out
---
```cpp
    // Function call expressions
    printf("\nFunction Call Expressions:\n");
    printf("sin(PI/2): %f\n", sin(PI/2));
    printf("pow(2, 3): %f\n", pow(2, 3));

    // Compound expressions
    double result = (a + b) * (c - (d / 2.0)) / (1 - sin(PI/4));
    printf("\nCompound Expression:\n");
    printf("(a + b) * (c - (d / 2.0)) / (1 - sin(PI/4)): %f\n", result);
```

- En realiter toutes les boucles sont coder avec des goto en assembleur
- Role du compilateur

```asm
    xorl ebx, ebx
.start:
    push ebx
    push fmt
    call printf

    addl ebx, 1
    cmpl ebx, 5
    jl .start
```

<!--
- Ne pas utiliser pour l'instant
- Globalement une mauvaise pratique
- Permet certaines optimisation
- Pas d'autre solution dans certaines circonstances
-->

---
transition: slide-left
layout: image-right
image: /snippets/eco-c-compiler.jpeg
---
## Produire un executable

Compilers:
- GCC (GNU Compiler Collection)
- Clang / LLVM
- Microsoft Visual C++ (MSVC)
- Intel C++ Compiler (ICC)
- Beaucoup

En ligne :
- [rextester.com](https://rextester.com/l/c_online_compiler_gcc)
- [programiz.com](https://www.programiz.com/c-programming/online-compiler/)
- [onlinegdb.com](https://www.onlinegdb.com/online_c_compiler)

---
transition: slide-left
---
## Produire un executable
Command-line compilation:
- gcc -o output_file source_file.c
- clang -o output_file source_file.c

Makefile:
- Ecrire un fichier Makefile contenant les instruction de compilation
- Run `make` command

CMake:
- Ecrire un fichier CMakeLists.txt -> Generation du Makefile
- Run `mkdir buid ; cmake .. ; make ;
- Lnacer `clang-format` !

Build automation tools:
  - Ninja, SCons, Bazel, Buck

---
transition: slide-left
---
## IDE (Environnement de Développement Intégré)

Outils de développement:
- Visual Studio Code
- Code::blocks
- CodeLite
- Qt Creator
- Dev-C++

Outils integrer
- Gestion des ressources (.c)
- Compilation
- Debuggeurs
- Versionning (GIT, SVN)
- Terminal

<!--
- Pratique mais non-indispenssable
- N'aide pas a la comprehenssion
-->