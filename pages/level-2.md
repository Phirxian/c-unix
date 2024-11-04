# Niveau 2

Fonctions et types composés

---
transition: slide-left
---
## Declaration
- Fonction -> signature
- Type
  - structure -> groupe de donnes
  - unions -> donnees paratagees

---
transition: fade-out
---
```cpp
const float PI = 3.14159f;

enum Days {
    MONDAY, TUESDAY, WEDNESDAY,
    THURSDAY, FRIDAY, SATURDAY, SUNDAY
};

/*
struct Person {
    char name[50];
    int age;
    float height;
};

union Data {
    int i;
    float f;
};
*/

typedef unsigned long long int uint64;


// Function declaration (prototype)
void print_person(struct Person p);
int pow(int i, int k);
float pow(float i, float k);
```