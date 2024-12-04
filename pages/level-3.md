<h1 class="text-center" style="position: relative;top: 50%;">Niveau 3</h1>
<p class="text-center" style="position: relative;top: 50%;">Pointeurs et gestion de la mémoire</p>

---
transition: fade-out
layout: center
---

```cpp
int *ptr;      // Déclaration d'un pointeur vers un entier
int a = 42;    // Une variable entière, notre trésor caché
ptr = &a;      // Adresse mémoire de a "alias" / "numéro d'appel"
*ptr = 8080;   // Déréférence ptr, je téléphone a @a pour changer sont contenue
int b = *ptr;  // Déréférence ptr, je téléphone a @a connaitre le contenue

const int *c_ptr = &a;   // La valeur pointée est top secrète (non modifiable)
int * const ptr_c = &a;  // L'agent ne peut pas changer de cible
*c_ptr = 554;      // Erreur : Tentative de modifier un document classifié 
ptr_c = &b;      // Erreur : Un agent fidèle ne change pas de mission

int tab[] = {1,2,3}; // Tableau est un pointeur @local (stack)
int *ptr = tab;      // Copie de l'adresse
printf("%d\n", ptr[0]);   // 1
printf("%d\n", *ptr);     // 1

// Opérateur arithmétique/assignation sur les pointeurs possible (++, --, ...)
printf("%d\n", *(ptr+1)); // 2 (next adress)
printf("%d\n", *(++ptr)); // 2 (next adress)
```

---
transition: fade-out
layout: center
---

## Attention aux dangers

```cpp
int *ptr; // numéro de téléphone au pif
int *ptr = NULL; // numéro 0 réservé au système (boot strap code, probablement pas dans la vtable)
*ptr = 42;  // Boum ! Segmentation fault ! Ouf merci le système

int tab[] = {1,2,3}; // Tableau est un pointeur @local (stack)
ptr = tab;
printf("%d\n", *((ptr+1)-2)); // Aie underflow, ça va exploser !
```

> C'est comme essayer d'envoyer un colis à une adresse qui n'existe pas. <br> Le facteur (votre programme) va partir au champignions !

https://docs.kernel.org/mm/page_tables.html

---
transition: fade-out
layout: center
---

## Arithmétique

```cpp
int array[10]; //! stack memory (libéré a la fin de l'appelle de la fonction)
int *start = array;
int *end = array+10;
int length = end-start; // 10 mémoire contigue -> difference de mémoire = taille
```

## Le compilateur utilise le type pour déduire la taille

```cpp
int length = (char*)end-(char*)start; // 40 ?
```

C'est comme compter en pas de bébé (octets) au lieu de pas d'adulte (ints) !
- 1 int = 4 octets (généralement)
- 1 char = 1 octet
- 10 ints * 4 octets/int = 40 octets


---
transition: fade-out
layout: center
---

## Que fait cette boucle ?

```cpp
int *start = array;
int *end = array+10;
for(; start != end; ++start)
    *start = 0;
```

- A) Elle compte le nombre d'éléments dans le tableau
- B) Elle remplit le tableau de zéros
- C) Elle déplace tous les éléments du tableau vers la droite

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
swap(&a, &b); // Envoie les adresses de a et b
// b=22 a=80
int tab[] = {1, 2};
swap(&tab[0], &tab[1]); // Parfaitement légal !

Vec2i pos;
Vec2i *adresse = &pos;
(*adresse).x // Déréférence : on ouvre la boîte
adresse->x   // Indirection : raccourci magique !

Vec2i **azerty = &adresse; // L'adresse de l'adresse (utile pour des table ND)
```

> En C, passer par adresse, c'est comme donner à quelqu'un les clés de votre maison plutôt que de lui faire visiter. <br> C'est plus rapide, mais attention à qui vous les confiez !

---
transition: fade-out
---

## Transtypage

Des pointeurs de type inconnue

```cpp
int a = 42;
void *ptr = (void*)&a; // Perte de la connaissance explicite de la taille des éléments
void *ptr = &a; // OK, conversion implicite
```

Le `void*` est comme un déguisement universel pour les pointeurs !

```cpp
*ptr = 8; // Erreur ! On ne sait pas quelle taille modifier
ptr[0] = 19 ; // Erreur inférence de sizeof(ptr[0]) impossible
*(int*)ptr = 8; // Ok, on donne la taille explicite
```

<!--
*(int*)ptr = 8; // OK (cast) -> a = 8 (4 octets modifié 0000 ... 1000)
-->

C'est comme essayer d'enfiler différentes tailles de chaussures à un pied invisible !

---
transition: fade-out
layout: center
---

> <h3>Que fait font ces lignes de code ?</h3>

```cpp
int a = 0xffffffff;
void *ptr = (void*)&a;
*(char*)ptr = 8; 
```

- A) Elle change la valeur de a (int) à 8 (donc 4 octets modifier)
- B) Elle change le premier octet de a : `0x08ffffff`
- C) Elle change le dernier octet de a : `0xffffff08`
- D) Elle provoque une erreur de compilation
- E) Autre chose ?

<!--
// (little endian !!!!) -> (int)a = 0x08ffffff (1 octet modifié) 
// (big endian !!!!) -> (int)a = 0xffffff08 (1 octet modifié) 
-->

---
transition: fade-out
layout: center
---

# Résumé

- Opérateur `&` retourne l'addresse de la mémoire
- Opérateur `*` appliqué a un pointeur retourne la variable stocké à @
- Ces deux opérateur sont l'inverses de l'autre
- `ptr[n]` et `*(ptr+n)` sont équivalent
    - `ptr[n]` = `n[ptr]`
    - `*(ptr+n)` = `*(n+ptr)`
- Opérateur arithmétique sur les pointeurs
    - Connaissance de taille associé grace au type
- `void*` est un pointeur vers un truc ... "générique"
    - pas de connaissance apriori sur la taille

---
transition: fade-out
---

## Quelques fontions utiles et générique

```cpp
void* memcpy(void * destination, const void * source, size_t length);
void* memset(void * ptr, int value, size_t length);
void* memmove(void * destination, const void * source, size_t length);
int memcmp(const void * ptr1, const void * ptr2, size_t length);
```

Pas besoin d'écrire la fonction pour chaque type !

```cpp
#include <stdio.h>
#include <string.h>

int main ()
{
    char buffer1[] = "DWgaOtP12df0";
    char buffer2[] = "DWGAOTP12DF0";

    int n = memcmp(buffer1, buffer2, sizeof(buffer1));

    if (n>0) printf ("'%s' is greater than '%s'.\n", buffer1, buffer2);
    else if (n<0) printf ("'%s' is less than '%s'.\n", buffer1, buffer2);
    else printf ("'%s' is the same as '%s'.\n", buffer1, buffer2);

    return 0;
}
```

---
transition: fade-out
---

## Pointeurs sur fonctions : délégué un comportement

- Une fonction est un pointeur vers sa première instruction exécutable
- On peut capturer ce pointeur pour l'envoyer en paramètre d'une fonction
- Permet de déléguer des parties d'un algorithme
- Nécéssite de spécifier la signature de la fonction

```cpp
#include <stdlib.h>

void qsort(void *array, size_t count, size_t size, int (*compareFunction)(const void*, const void*));

int compare_int(const void *first, const void *second) {
    const int firstInt = *(const int *)first;
    const int secondInt = *(const int *)second;
    return firstInt - secondInt;
}

int compare_self(const void *self) {} // Mauvaise signature
int compare_float(const float *first, const float *second) {} // Mauvaise signature

int main() {
    int intArray[] = { 10, 50, 30, 20, 40, 60 };
    qsort(intArray, sizeof(intArray)/sizeof(int), sizeof(int), compare_int);
    return 0;
}
```

---
transition: fade-out
layout: center
---

## Exemple pratique

Utilisation de typedef pour plus de clarté :

```cpp
typedef int (*Comparator)(int, int);
Comparator cmp = &compare_int;

int my_function_with_func_ptr(void * blablabla, Comparator my_comparator) {
    return my_comparator(1,2);
}
```

Les interface graphique utilise des callbacks (pointeur sur fonction)

```cpp
void key_callback(GLFWwindow* window, int key, int scancode, int action, int mods)
{
    if (key == GLFW_KEY_E && action == GLFW_PRESS)
        activate_airship();
}

glfwSetKeyCallback(window, key_callback);
```

---
transition: fade-out
layout: two-cols
---

```cpp
typedef enum {
    STATE_IDLE,
    STATE_RUNNING,
    STATE_PAUSED,
    STATE_FINISHED
} State;

typedef State (*StateFunction)(void);

State idle_state(void) {
    printf("Idle: Waiting for input...\n");
    return STATE_RUNNING;
}
```

::right::


```cpp
StateFunction state_table[] = {
    idle_state,
    finished_state
};

void run_state_machine(void) {
    State current_state = STATE_IDLE;
    while (1) {
        current_state = state_table[current_state]();
        if (current_state == STATE_FINISHED)
            break;
    }
}
```

---
transition: fade-out
layout: center
---

# Allocation dynamique !!!

---
transition: fade-out
---

- Allocation
    - `void* malloc(size_t s)`
        - Retourne un pointeur vers la mémoire allouée ou NULL
        - C'est comme réserver une table au fastfood, c'est pas toujours propre
    - `void* calloc(size_t s)`
        - idem a malloc + memset(ptr, s, 0)
        - C'est comme réserver une table au restaurant, là c'est propre !
- Reallocation
    - `void* realloc(void* ptr, size_t s)`
    - Modifie la taille d'un bloc de mémoire déjà alloué
    - Retourne le nouveau pointeur ou NULL si échec (pas de changement sur ptr)
    - C'est comme demander une table plus grande (ou plus petite) au milieu du repas !
- Durée de vie
    - Dans la stack `int buffer[10];` -> n'existe plus après l'appelle 
    - Dans la heap `int *buffer = malloc(sizeof(int)*10);` -> jusqu'a un free

---
transition: fade-out
---

Attention aux Pièges !
- Libèrer la mémoire allouée
    - `void free(void *ptr)`
    - C'est comme rendre les clés de votre chambre d'hôtel
    - Ne pas oublier de libérer la mémoire (fuites de mémoire)
    - Ne pas libérer une mémoire toujours utilisée (erreurs de segmentation)
    - Ne pas perdre de pointeur en route (mémoire orpheline)
- Allocation / Désallocation prennet du temp
    - Possible de géré votre mémoire

---
transition: fade-out
layout: center
---

> <h3>Que fait free() deux fois sur le même pointeur ?</h3>

- A) Rien, c'est sécurisé
- B) Comportement indéfini
- C) Le programme libère deux fois plus de mémoire
- D) Segmentation fault
- E) Bluescreen

---
transition: fade-out
---

## Exemple linked-list

```cpp
typedef struct Node {
    int data;
    struct Node* next;
} Node;

Node *root = malloc(sizeof(Node));
root->data = 21;
root->next = NULL; // pas de sucesseur

Node *second = malloc(sizeof(Node));
second->data = 42;
second->next = NULL;

root->next = second; // changement du successeur

while(root) {
    Node *tmp = root->next;
    free(root);
    root = tmp;
}
```

---
transition: fade-out
layout: center
---

# Comment simplifier la gestion mémoire ?

> L'allocation prend du temps ! <br> possibilité de retenir/réutiliser des segments mémoires

<br>

> Suivre l'utilisation d'une mémoire est difficile <br> alors compté les utilisations !

---
transition: fade-out
---

## Gestion des segments mémoire

```cpp
// Double linked list
typedef struct DataSegment {
    void *ptr;
    size_t len;
    struct Node* next;
    struct Node* prev;
} DataSegment;

DataSegment *used, *ununsed;

void move_to_used(DataSegment *ptr) {
    DataSegment *segment = ptr;
    DataSegment *root = used;

    if(ptr->prev)
        ptr->prev->next = ptr->next;

    if(ptr->next)
        ptr->next->prev = ptr->prev;

    used = ptr;
    ptr->prev = NULL;
    ptr->next = root;
}
```

---
transition: fade-out
---

```cpp
void *rc_malloc(size_t size) {
    int *ptr = malloc(size + sizeof(int));
    if (!ptr)
        return NULL;
    *ptr = 1;  // Initialize reference count to 1
    return ptr + 1;  // Return address after the count
}

void rc_incref(void *ptr) {
    if (ptr) {
        int *count = (int*)ptr - 1;
        (*count)++;
    }
}

void* rc_decref(void *ptr) {
    if (ptr) {
        int *count = (int*)ptr - 1;
        (*count)--;
        if (*count == 0) {
            free(count);  // Free the entire block
            return NULL;
        }
    }
    return ptr;
}
```

---
transition: fade-out
layout: two-cols
---

## On peu aussi mixé les deux !



```cpp
typedef struct DataSegment {
    size_t len;
    int ref_count;
    struct DataSegment* next;
    struct DataSegment* prev;
} DataSegment;

DataSegment *used = NULL, *unused = NULL;

void* allocate_segment(size_t size) {
    DataSegment *new_segment = (DataSegment*)calloc(
        sizeof(DataSegment) + size);

    if (!new_segment)
        return NULL;

    new_segment->len = size;
    new_segment->ref_count = 1;
    new_segment->next = used;
    
    if (used)
        used->prev = new_segment;

    used = new_segment;
    return (void*)(new_segment + 1);
}
```

::right::

<img src="/snippets/garbage-colector.webp" width="100%"/>

```cpp
void rc_decref(void *ptr) {
    DataSegment *segment = (DataSegment*)ptr - 1;
    
    if (--segment->ref_count > 0)
        return;

    if (segment->prev) segment->prev->next = segment->next;
    if (segment->next) segment->next->prev = segment->prev;
    if (segment == used) used = segment->next;
    if (ununsed) ununsed->prev = segment;
    segment->prev = NULL;
    segment->next = unused;
    unused = segment;
}
```
---
transition: fade-out
---

## Résumé

- 1 pointeur c'est une variable une adresse 
    - d'une autre variable
    - d'une fonction
    - &var donne donne l'adresse (le "où")
    - *var donne la valeur (le "quoi")
- Même arithmétique que les tableaux (déplacement mémoire en fontion du type)
    - Se déplace en mémoire selon le type pointé
- Peuvent être utilisés pour modifier des paramètres d'entré de fonction (ou plusieurs retour)
    - `int solution_second_degree(int a, int b, int c, int *sol1, int *sol2);`
- Permettent de manipulé des objets dans la heap
    - Possibilité d'allouer plus de mémoire que possible
    - Le système peut allouer un peu plus que demandé
    - Permet de crée des liens complexes (graph de dépendence)
