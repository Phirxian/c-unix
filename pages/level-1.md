<h1 class="text-center" style="position: relative;top: 50%;">Niveau 1</h1>
<p class="text-center" style="position: relative;top: 50%;">Flux de contrôle et types de données</p>

---
transition: slide-left
layout: default
---
## Instructions

- Condition (if, else, switch)

```cpp
    int age = 20;
    if (age >= 18)
        printf("You are an adult.\n");
    else if(age >= 14)
        printf("You are a teenager.\n");
    else
        printf("You are a minor.\n");

    
    int day = 3;
    switch (day) {
        case 1:
            printf("Monday\n");
            break;
        case 2:
            printf("Tuesday\n");
            break;
        case 3:
            printf("Wednesday\n");
            break;
        default:
            printf("Other day\n");
    }

```

---
transition: slide-left
---
- Boucles (for, while, do-while)

```cpp
    printf("While loop:\n");
    int j = 0;
    while (j < 5) {
        printf("%d ", j);
        j++;
    }
    printf("\n");

    printf("Do-while loop:\n");
    int k = 0;
    do {
        printf("%d ", k);
        k++;
    } while (k < 5);
    printf("\n");

    printf("For loop:\n");
    for (int i = 0; i < 5; i++)
        printf("%d ", i);
    printf("\n");
```

---
transition: slide-left
---
- Jump (break, continue, goto, return)

```cpp
    printf("Break in a loop:\n");
    for (int i = 0; i < 10; i++) {
        if (i == 5) {
            break;
        }
        printf("%d ", i);
    }
    printf("\n");

    printf("Continue in a loop:\n");
    for (int i = 0; i < 10; i++) {
        if (i % 2 == 0) {
            continue;
        }
        printf("%d ", i);
    }
    printf("\n");

```

---
transition: fade-out
---

Proscrit / expert

```cpp
void demonstrate_goto(void) {
    int i = 0;
start:
    printf("%d ", i);
    i++;
    if (i < 5)
        goto start;
}
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