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
const int *c_ptr = &a; // valeur non modifiable
int * const ptr_c = &a; // ptr non modifiable
*c_ptr = 554;      // erreur
*ptr_c = &b;      // erreur

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

int length = end-start; // 10 mémoire contigue -> difference de mémoire = taille
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
- ptr\[n\] et *(ptr+n) sont équivalent
    - ptr\[n\] = n\[ptr\] <=> *(ptr+n) = *(n+ptr)
- void* est un pointeur vers un truc ... "générique"

---
transition: fade-out
---

## Quelques fontions utiles et générique

- void * memcpy ( void * destination, const void * source, size_t num );
- void * memset ( void * ptr, int value, size_t num );
- void * memmove ( void * destination, const void * source, size_t num );
- int memcmp ( const void * ptr1, const void * ptr2, size_t num );

```cpp
#include <stdio.h>
#include <string.h>

int main ()
{
  char buffer1[] = "DWgaOtP12df0";
  char buffer2[] = "DWGAOTP12DF0";

  int n = memcmp ( buffer1, buffer2, sizeof(buffer1) );

  if (n>0) printf ("'%s' is greater than '%s'.\n",buffer1,buffer2);
  else if (n<0) printf ("'%s' is less than '%s'.\n",buffer1,buffer2);
  else printf ("'%s' is the same as '%s'.\n",buffer1,buffer2);

  return 0;
}
```

---
transition: fade-out
---

## Pointeurs sur fonctions

- Une fonction est un pointeur vers la première instruction exécutable
- On peut prendre ce pointeur pour l'envoyé en paramètre d'une fonction
- Permet de délégué des partie d'un algorithme
- ex : `qsort( void* ptr, size_t count, size_t size, int (*comp)(const void*, const void*) );`
    - `comp` est un pointeur sur fonction 
- Cette fonction attent une fonction aillant la même signature que spécifier en 3ieme arg
- Ex :
    - `int compare_int(int a, int b) { return a-b; }`
    - `qsort(buffer, sizeof(buffer), sizeof(buffer), &compare_int);`
- privilégier les `typedef` :
    - `typedef int (*Comparator)(const void*, const void*);`
    - `Comparator cmp = &compare_int;`
```cpp
int sum(int a, int b) { return a+b; }
int (*p_add)(int, int) = & sum;
p_add(1,5);
// on peut aussi faire des tableaux de ptr sur fonction
int (*p_binop[])(int, int) = {sum, sub, mul, div};
// elle doivent toutes avoir la même signature
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
        - retourne le ptr de la mémoire ou NULL
    - `void* calloc`
        - idem a malloc + memset(ptr, s, 0)
    - attention de libéré la mémoire
- Reallocation
    - `void* realloc`
    - retourne le ptr de la mémoire ou NULL (sans changement)
- Libération
    - `void free(void *ptr)`
    - attention de ne pas libéré une mémoire toujours utilisée
    - ne pas perdre de pointeur en route ! free impossible

Indispenssable pour des strutures de données avancées !

---
transition: fade-out
---

## Exemple linked-list

```cpp
typedef struct Node {
    int data;
    struct Node* next;
} Node;

Node *root = malloc(sizeof(Node)); // transtypepage implicite
root->data = 5;
root->next = NULL; // pas de sucesseur

Node *second = malloc(sizeof(Node));
second->data = 5;
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
---

## Résumé

- Pointeur = variable contenant l'adresse d'une autre variable
- &var donne l'@
- *var donne la valeur
- Même arithmétique aue les tableaux (déplacement mémoire en fontion du type)
- Peuvent être utilisés pour modifier des paramètres d'entré de fonction (ou retour)
- Permettent de manipulé des objets dans la heap

---
transition: fade-out
---

# Persistence et flux de données

- Les fichier :
    - ouverture : `FILE* fopen (char *fname, char *mode)` (rwb)
    - fermeture : `fclose(FILE*)`
- Sorti d'autres processus :
    - ouverture : `FILE* popen (char *fname, char *mode)` (rb)
    - fermeture : `pclose(FILE*)`
    - ex : `popen("head -n 10 /proc/cpuinfo | grep name", "r");`
- Les sockets
    - voir en programation système
    - ouverture : `sockfd = socket(AF_INET, SOCK_STREAM, 0);`
    - `close(sockfd);`
- Les interface system `/dev`
    - ouverture : `device = fopen("/dev/sda1", "rb");`
    - voir en programation système

---
transition: fade-out
---

- Leture
    - `int fscanf(FILE*, char *format, ...)` (texte)
    - `int fgets(char*, size_t len, FILE *fd)` (texte)
    - `size_t fread(void *buffer, size_t size, size_t count, FILE *stream );`
- Écriture : 
    - `int fprintf(FILE*, char *format, ...)` (texte)
    - `int fputs(char* str, FILE *fd)` (texte)
    - `size_t fwrite( const void* buffer, size_t size, size_t count, FILE* stream );`
- Détetion d'erreur
    - `int ferror(FILE*)`
    - 0 = ok
    - !0 = code d'erreur
- Position
    - `int ftell(FILE*)` // retourne la position actuel
    - `int fseek(FILE*, long offset, int origin)` // change la position
- Flush
    - `int fflush(FILE*)`
    - force l'écriture de la mémoire tampon dans le fichier
    - "synchronisation"