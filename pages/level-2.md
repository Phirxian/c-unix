<h1 class="text-center" style="position: relative;top: 50%;">Niveau 2</h1>
<p class="text-center" style="position: relative;top: 50%;">Fonctions et types composés</p>

---
transition: slide-left
---
## Declaration
- Fonction -> signature
- Type
  - array 
  - structure -> groupe de donnes
  - unions -> donnees paratagees

---
transition: fade-out
---
```cpp
// tableau local
// type name[size];
int myarray[10];

// initialisation
int myarray[3] = {1,2,3}; // C89
int myarray[] = {5,6};    // C99 déduction

// access -> rw
myarray[0] // 5 (int) -> cannot have != type
myarray[2] // segmentation fault : overflow

char text[] = {'A', 'r', 'r', 'g', '.', '.', '.'};
char text[] = "Follow the white rabbit"; // \0
```

- Les données d'un tableau sont continue en mémoire (garantit par l'os)
- Taille N -> indice [0, N-1]
- Le tableau ne connait pas sa taille (tableau élémentaire != java)
- Les données peuvent être "mixé" a condition quelles aient toutes la même tailles (attention)
- Déduction de taille a la compilation : sizeof(text) : sizeof(myarray)/sizeof(int)
- Prend de la place dans le tas -> taille limitéq
- C'est une adresse mémoire vers le premier élément

---
transition: fade-out
---

```cpp
struct Vec2i {
    int x, y; // 4+4 byte
};

typedef struct Person {
    char name[50]; // 50 byte -> it's a field
    int age;       // 54 byte
    float height;  // 58 byte
    // struct Vec2i pos;
};                 // total size ... depend on the compiler ~64

union Data {
    int i;      // 4 byte
    float f;    // 4 byte
    char c[4];  // 4 byte
};              // total size ... 4 byte

typedef struct Vec2i Vec2i;
typedef union Data Data;

Vec2i pos; // random
Vec2i pos = {0, 1};
Vec2i pos = {.x =0, .y=1};
```

- Même principe qu'un tableau, (mémoire)
- `((int*)pos)[0])`


---
transition: fade-out
---

```cpp
struct Scene {
    Entity player;
    Entity skydome;
    Entity enemies[3];
};
typedef struct Scene Scene;

Scene scene = {
    {"player.jpg", {0,0}, ...},
    {"skydome.jpg", {0,0}, ...},
    {
        {"lucien.jpg", {10,0}, ...},
        {"harry_potter.jpg", {10,25}, ...},
        {"choucroutte.jpg", {10,25}, ...},
    }
};

// attention l'affectation recopie toutes les données
scene[0] = scene[1]; // clone
```

---
transition: fade-out
---

## Fonctions

- Utiliser une fonction nécéssite qu'elle soit déclarer
- Déclaration != Implémentation
- Plusieurs fonction avec le mếme nom possible (signature)
- Nécéssairement un type de retour
    - void -> procédure

```cpp
// forward declaration -> in .h
// indique l'existence d'une fonction
float pow(float, float);
float pow(float);

// implémentation -> in .c (otherwise multiple-definision)
float pow(float i) { return i*i; }

// special case, optimised function
inline float pow(float i) { return i*i; }

// utilisation classique
pow(2,2); // comme "instruction" -> résultat perdu
float delta = pow(b) - 4 * a * c; // utilise la valeur retourner
```

---
transition: fade-out
---

- Les paramètres d'une fonction sont **copié** au moment de l'appel.
- Ils deviennent des variables **local** -> pas d'impact sur ceux appelé
- Les paramètres d'une fonction sont passé par valeur
    - types de bases, struct, union, pointer

```cpp
void swap(int i, int j) {
    int tmp = j;
    j = i;
    i = tmp;
    // échange des variables local
}

int a = 8, b = 10;
swap(a, b);
// aucun impact sur @a et @b
```

- utilisé les pointeurs ! @adresse

---
transition: fade-out
---

## Fonction récursive

- Globalement à ÉVITER
- Chaque appel nécéssite de réserver de la place mémoire dans la stack
- Chaque appel nécéssite de recopier les valeurs d'appel
- Toute fonction récursive peut-être transformé en boucle+struct
- Une fonction récursive doit éfectuer un test de terminaison
- La profondeur d'appel est **limité** (stackoverflow unless -fstack-usage (lourd - performant))

```cpp
long long int fact(long long int n) {
    if(n>0)
        return n*fact(n-1);
    return 1;
}

fact(18); // = beaucoup
fact(30); // négatif
fact(99999); // stackoverflow - segfault
```

- https://steflan-security.com/complete-guide-to-stack-buffer-overflow-oscp/
- http://www.herikstad.net/2019/03/prevent-and-mitigate-stack-overflow.html

---
transition: fade-out
layout: center
---

<img src="/snippets/stack.png" width="100%"/>
virtual address space != ram -> memmap:os

https://stackoverflow.com/questions/32418750/stack-and-heap-locations-in-ram

---
transition: fade-out
---

## Passage de struct en paramètre

```cpp
void print(Vece2i pos) { printf("{%d,%d}", pos.x, pos.y) };
print({0,5});
```

- Attention passé une structure en paramètre peut prendre beaucoup de place
- Idem pour un tableau -> exponentiel(récursif)
- Préféré l'utilisation de pointeur ! `sizeof(Vec2i*) = sizeof(intptr_t)`
    - Moin de mémoire - Moin de temps
    - Cout de déréférencement

```cpp
// const to tell that the data inside @pos is not modified
void print(const Vece2i *pos) { printf("{%d,%d}", pos->x, pos->y) };
Vec2i pos = {0,5};
print(&pos); // & -> récupère l'adresse mémoire
```