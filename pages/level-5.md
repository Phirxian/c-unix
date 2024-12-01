<h1 class="text-center" style="position: relative;top: 50%;">Niveau 5</h1>
<p class="text-center" style="position: relative;top: 50%;">Aller plus loin</p>

---
transition: fade-out
---

## Fonction variadic

Des fonctions ave un nombre variable d'argument :

```cpp
#include <stdarg.h>

enum TYPE { VAT_INT, VAT_FLOAT, VAT_END };

void sum(...) {
    va_list list;
    int type;

    while((type = va_arg(list, int))) {
        if(type == VAT_INT)
            printf("%d", va_arg(list, int));
        if(type == VAT_END)
            break;
    }

    va_end(list);
}
```

# Alignement

Le compilateur rajoute presque toujours des octets suplémentaires. Alignement vers des mots machines -> optimisation.

```cpp
struct Bidule {
    char a; // 1
    int b;  // 4
    char c; // 1
};

sizeof(struct Bidule) = 12

struct ManMadeAlignement {
    char a;
    char _unused_field_1;
    char _unused_field_2;
    char _unused_field_3;
    int floor;
    char b;
    char _unused_field_1;
    char _unused_field_2;
    char _unused_field_3;
};
// permet de garantir le même alignement sur tout les compilateur
```