<h1 class="text-center" style="position: relative;top: 50%;">Niveau 3</h1>
<p class="text-center" style="position: relative;top: 50%;">Pointeurs et gestion de la mémoire</p>

---
transition: fade-out
layout: center
---

```cpp
int *ptr;      // lkqsmldk
int a = 42;    // variable 
ptr = &a;      // adresse mémoire de a "alias" "numéro d'appel"
*ptr = 8080;   // déréférence a, change le contenue de a
int b = *ptr;  // déréférence b, récupère le contenue et copier
const *c_ptr = &a; // pointeur non modifiable
*c_ptr = 554;      // erreur

int tab[] = {1,2,3}; // tableau est déjà un pointeur @local
int *ptr = tab;      // copie de l'adresse
printf("%d\n", ptr[0]);   // 1
printf("%d\n", *ptr);     // 1
printf("%d\n", *(ptr+1)); // 2 (next adress)
printf("%d\n", *((ptr+1)-2)); // aie underflow

// opérateur arithmétique/assignation sur les pointeurs possible (++, --, ...)
// comparaison, mask, ... address = entier
char *ptr = (char*)&a; // attention endian ! (ptr+3)=42
```

---
transition: fade-out
layout: center
---

```cpp
int array[10];
int *start = array;
int *end = array+10;

int length = end-start; // 10 merci contigue -> difference de mémoire = taille
int length = (char*)end-(char*)start; // 40 ? pourquoi ? :)

for(; start != end; ++start)
    *start = 0;

```

---
transition: fade-out
---

## Passage par adresse

```cpp
void swap(int *i, int *j) {
    int tmp = *j;
    *j = *i;
    *i = tmp;
}

int a = 22, b = 80;
swap(&a, &b); // envoie des adresses de @a et @b
// b=22 a=80
int tab[] = {1, 2};
swap(&tab[0], &tab[1]); // ok

Vec2i pos;
Vec2i *adresse = &pos;
(*adresse).x // déréférence
adresse->x   // indirection

Vec2i **azerty = &adresse; // l'adresse de l'adresse (utile pour des table ND)
```
---
transition: fade-out
---

## Transtypage

Des pointeurs de type inconnue

```cpp
int a = 42;
void *ptr = (void*)&a; // perte de la taille de l'élément N
void *ptr = &a; // OK
*ptr = 8; // ????
*(int*)ptr = 99; // OK (cast) -> a = 99
*(char*)ptr = 99; // OK (cast) -> a = (99<<24)
ptr[0] = 19 ; // erreur inférence de sizeof(ptr[0]) impossible
```

- opérateur `&` retourne l'addresse de la mémoire
- opérateur `*` appliqué a un pointeur retourne la variable stické à @
- ces deux opérateur sont l'inverses de l'autre
- ptr[n] et *(ptr+n) sont équivalent
- void* est un pointeur vers un truc ... "générique"