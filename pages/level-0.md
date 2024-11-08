<h1 class="text-center" style="position: relative;top: 50%;">Niveau 0</h1>
<p class="text-center" style="position: relative;top: 50%;">Base du langage C</p>

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


<!--
AST -> Abre Syntaxique -> Token / Type
-->

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
## Instructions
- Enssemble de token terminé par un `;`
- Opérateur pour évaluer plusieurs expressions `,`
- Les instructions sont presque _toujours_ déclarées dans des fonctions

```cpp
int i; // 1 instruction
int j = 0; // 1 instruction (2 asm), 1 block non-secable
i += 42; // 1 instruction
j = 5, p = 8; // 1 instruction (2 asm), 1 block non-secable
j = 5; p = 8; // 2 instructions

// un block (non-secable)
{
    int k;
    k = (i+j)/2;
}

// instruction vide (passtrough)
;
```

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
- Porté
  - locale = dans une fonction
    - $<C99$ declaration des variables en premiers
    - pas d'initialisation automatique
  - global = en dehors d'une fonction (ou d'un block)
    - initialisation avant l'initialisation du program `_start`
    - copie de `section .data`

---
transition: slide-left
---

- Types de base :
  - Taille depend de l'os et du materiel
  - `char` (1), `short` (2), `int` (2-4), `long` (4-8)
  - `float`(4), `double`(8), `long double` (8-16)
    - `mini` (1) `half` (2) .. `octuple` (32)
    - https://en.wikipedia.org/wiki/IEEE_754
    - Gestion des exceptions (nan, +inf)
  - long long (8) (C99)
  - void
- Type specifique :
  - size_t(4-8), ptrdiff_t(4-8), intptr_t(4-8), wchar_t(2-4), __m128(16)
- Verification de taille : `sizeof(char) == 1`
- Modificateur :
  - signed, unsigned -> entier
  - const, static, register, volatile
- Enumeration

<!--
https://rextester.com/l/c_online_compiler_gcc
- `char` type signed/unsigned depend du compilateur
- volatile is a hint to the implementation to avoid aggressive optimization involving the object
- Precision fini -> approximation de R
- Erreur d'arrondi
- Garantit que certains types de données auront toujours le même format
- Garantit que les calculs effectués sur un type de donnée donneront toujours le même résultat.
- Gère les cas exceptionnels, comme les infinis, les NaN (pour Not a Number – ex: division 0/0), etc.
-->

---
transition: slide-left
---
### Valeurs min-max

<div style="display: flex">
<img src="/snippets/signed-unsigned.jpeg" width="50%">
<img src="/snippets/signed-unsigned-min-max.jpeg" width="50%">
</div>

---
layout: iframe
url: https://en.cppreference.com/w/c/types/limits
---

---
transition: slide-left
---
### Attention a l'architecture !

<img src="/snippets/endian.png" width="50%" style="position: relative; left:20%">

```cpp
#if __BYTE_ORDER__ == __ORDER_LITTLE_ENDIAN__
...
#elif __BYTE_ORDER__ == __ORDER_BIG_ENDIAN__
...
#else
// __ORDER_PDP_ENDIAN__
#endif
```
```cpp
if(*(char *)(int[]){1}) {
    /* little endian code */
} else {
    /* big endian code */
}
```


---
transition: slide-left
layout: two-cols
---
```cpp
enum Days {
    MONDAY, TUESDAY, WEDNESDAY,
    THURSDAY, FRIDAY, SATURDAY, SUNDAY
};

typedef unsigned long long int uint64;
```

```cpp
const char a = 'A'; // 97
const float PI = 3.14159f;
unsigned int i;
i = 15u;
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

::right::

- `'\n'` newline
- `'\r'` tabulation
- `'\b'` backspace
- `'\''` single quote
- `'\"'` double quote
- `'\?'` question mark
- `'\r'` carriage return
- `'\a'` alert (bell - buzzer)
- `'\\'` backslach
- `'a' = 97 ; 'a'+1 = 'b' = 98`
- `'0' = 48 ; '0'+1 = '1' = 49`
- `"Le pouvoir réside là où on se l'imagine. Même un petit homme peut projeter une grande ombre."`
- `L"権力は私たちが想像するところに宿る。"`

<!--
windows `\r\n`
-->

---
transition: fade-out
layout: two-cols 
---

## Identificateurs

- Identificateur commence par une lettre ou "_"
  - variables
  - fonctions
  - macros
- Est alphanumerique [a-Z + 0-9]
- Sensible a la casse :
  - a != A
  - sqrt != SQRT
- "_ _" : system/compilateur
- Mots réservés du langage

::right::
### Notation

- decimal : 12345678
- octale : `0407`
- hexadecimal : `0x4545FF`
- postfixage:
  - double : `0.5` / float : `0.5f`
  - unsigned : `65u`
  - long : `65l` / long long `65ll`
  - combinaison : `65ul`


<!--
identificateur = variable
-->

---
transition: fade-out
layout: iframe
url: https://en.cppreference.com/w/c/keyword
---

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
    printf("a++ = %d\n", a++); // 10
    printf("--b = %d\n", --b); // 4

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
    // C99 -> stdbool.h
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
    printf("~a: %d\n", ~a); // -2
    // Assignment expressions
    int c = 15;
    printf("\nAssignment Expressions:\n");
    printf("c += 5: %d\n", c += 5); // 20
    printf("c -= 3: %d\n", c -= 3); // 17
    printf("c *= 2: %d\n", c *= 2); // 34
    printf("c /= 4: %d\n", c /= 4); // 8
    printf("c %%= 3: %d\n", c %= 3); // 2
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

Portable :
- [w64devkit](https://github.com/skeeto/w64devkit)

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
- Run `mkdir buid ; cmake .\. ; make ;
- Lancer `clang-format` ! Ou `unittest`

Build automation tools:
  - Ninja, SCons, Bazel, Buck

---
transition: fade-out
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

---
transition: slide-left
---

## Bibliotheques standards

- stdlib (la base -> C89)
  - Pas si standard (embarqué, pc, supercalculateurs, .. microcontroler)
  - Probleme de portabilité
  - Tres restreint
    - Pas de reseaux, thread, gui
- posix (programmation system)
  - Processus, threads, signaux, pipes
  - Synchronisation mutex, semaphore
  - clocks & timer
  - system unix et windows

<!--
Cannal de discution
-->

---
transition: slide-left
---

### STDLIB

Rôle des headers les plus courantes
- stdio.h permet la gestion des E/S (printf, scanf, ...)
- string.h permet la gestion des chaines (strcpy, strcmp, strchr, ...)
- stdlib.h ajoute des fonctions générales
  - conversion (atoi, atof, ...)
  - gestion dynamique de la mémoire (malloc, realloc, free, ...)
  - aléatoire (rand, srand, ...)
  - arithmétique des entiers, tri (qsort, ...)
- math.h ajoute des fonctions mathématiques (sqrt, ...)
- time.h permet de manipuler l’information temporelle (clock, time, ...)
- stdarg.h gestion des arguments passés en paramètre au programme
- errno.h gestion sommaire des erreurs
- http://www.cplusplus.com/reference/clibrary
- https://en.cppreference.com/w/c/header
---
transition: fade-out
---

### stdio.h

Affichage d’information sur la console
```cpp
putc('\n');
puts("Erreur "__FILE__);
printf("La valeur de l'entier est %d\n", 1);
printf("La valeur du floatant est %f\n", 3.14159f);
printf("Le message est \"%s\"\n", "bidule");
```
Lecture d’une information au clavier

```cpp
char str[1000];
float nombre;
char c = getchar(stdin);
int error = gets(str);
int error = scanf("%f", &nombre); // Le & du 2ème argument est indispensable (adresse memoire)
```

---
transition: slide-left
---

## Excercices

<br>

### 1 - Prise en main du compilateur  :

<br>

- Ecrire main.c

```cpp
#include <stdio.h>
int main() { printf("Hello World !"); return 0; }
```

- Compiler (gcc), executer

<br>

### 2 - Ecrire une resolution partiel ax+by+c :

<br>

- `math.h -> sqrt`
- `a=5 ; b=9 ; c=3 -> x1=-0.4417 x2=-1.3583`
- `printf("une variable %f\n", a)`

---
transition: slide-left
---

## Correction

```cpp
#include <stdio.h>
#include <math.h>

int main()
{
    float a = 5, b = 9, c = 3;

    printf("Les valeurs sont :\n");
    printf("a = %f\n", a);
    printf("b = %f\n", b);
    printf("c = %f\n", c);

    float discriminant = b * b - 4 * a * c;
    float x1 = (-b + sqrt(discriminant)) / (2 * a);
    float x2 = (-b - sqrt(discriminant)) / (2 * a);

    printf("L'équation a deux solutions réelles distinctes :\n");
    printf("x1 = %.4f\n", x1);
    printf("x2 = %.4f\n", x2);
        
    return 0;
}
```