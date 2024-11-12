<h1 class="text-center" style="position: relative;top: 50%;">Niveau 1</h1>
<p class="text-center" style="position: relative;top: 50%;">Flux de contrôle : conditions et boucles</p>

---
transition: slide-left
layout: default
---
## Instructions

- Condition (if, else, switch)
- Toute condition différent de 0 est vrai

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
layout: default
---

```cpp
int error = unefonction();
if (error) {
  printf("C'est ballo'");
}
```

```cpp
int error = unefonction();
if (error != 0)
    printf("C'est ballo'");
```

```cpp
int a = 0;
int b = 2;

if (a = b)
    printf("Magie magie");
    
if (a == b) {
    printf("Oups");
}
```

<!-- 
falle ssl
-->

---
transition: slide-left
layout: default
---

```cpp
if (condition) {
    if(unautre)
        ;
    else
        ;
}
else {
}


if (condition)
    ;
else if(unautre)
    ;
else
    ;
```

---
transition: slide-left
---

```cpp
int main() {
  double a, b, c;
  double discriminant, x1, x2;

  printf("Entrez les coefficients a, b et c de l'équation ax² + bx + c = 0 :\n");
  scanf("%lf %lf %lf", &a, &b, &c);

  if (fabs(a) < EPSILON) {
    printf("Ce n'est pas une équation du second degré (a ≈ 0).\n");
    return 1;
  }

  discriminant = b * b - 4 * a * c;

  if (discriminant > EPSILON) {
    x1 = (-b + sqrt(discriminant)) / (2 * a);
    x2 = (-b - sqrt(discriminant)) / (2 * a);
    printf("L'équation a deux solutions réelles distinctes :\n");
    printf("x1 = %.4f\n", x1);
    printf("x2 = %.4f\n", x2);
  } else if (fabs(discriminant) < EPSILON) {
    x1 = -b / (2 * a);
    printf("L'équation a une solution réelle double :\n");
    printf("x = %.4f\n", x1);
  } else {
    double realPart = -b / (2 * a);
    double imagPart = sqrt(-discriminant) / (2 * a);
    printf("L'équation a deux solutions complexes conjuguées :\n");
    printf("x1 = %.4f + %.4fi\n", realPart, imagPart);
    printf("x2 = %.4f - %.4fi\n", realPart, imagPart);
  }

  return 0;
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