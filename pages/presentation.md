
# Programme

- Niveau -1 : Présentation
- Niveau 0 : Base du langage C et types de données
- Niveau 1 : Flux de contrôle : conditions et boucles
- Niveau 2 : Fonctions et types composés
- Niveau 3 : Pointeurs et gestion de la mémoire
- Niveau 4 : Performance avancées du langage C
  
---
transition: fade-out
hideInToc: true
layout: image-right
image: /snippets/dennis-ritchies.webp
---

# Présentation

- Dennis Ritchie
- Conception 1969-1973 Bell Labs (UNIX)
- Populaire
  - 1978 - "The C Programming Language" book by Ritchie & Kernighan
  - plusieurs standard, extensions, etc
  - 1983 - ANSI committee formed to standardize C
  - 1989 - ANSI C standard (C89) released
  - 1990 à 2024 -> C90/C99/C11/C17/C23

Encore largement utilisés dans les systèmes !
Embarqués et performances critiques.
  
---
transition: fade-out
hideInToc: true
layout: image-left
image: /snippets/bjarne-straustroup.jpg
---
## Présentation

- Influence de nombreux language
- C++ (Bjarne Straustroup)
  - Conception 1979 - 1982
  - C with class 1982 (AT&T Bell Labs)
  - 1985 (The C++ Programming Language)
  - 1998 (ISO C++98)
  - ...
  - 2023 (ISO C++23)
  - Développement parallele à C
  - Syntaxe de base proche, comportement !=
- Paradigme : Java, Rust, Python, Go, C#, ...

Comprendre le C et les méchansimes soujacent permet de maitriser plus rapidement tout ces languages
  
---
transition: fade-out
---
## Charactéristique

- Language structuré et fonctionnel
- Produit des programmes efficasses
  - Merci le compilateur
- Déclaratif
  - Déclaré avant utiliser (\.h header)
  - possible de manipulé les données brutes
- Modulaire et compilé
  - Compilation sépararer (\*.c -> \*.o)
  - Bibliothèque statique (\*.a)
  - Bibliothèque dynamique (\*.so, \*.dll)
- Souple de permissif
  - Vérification la charge du programmeur
    - gestion de la mémoire, des tailles, des access, etc
- Portable, disponible globalement partout

---
transition: fade-out
hideInToc: true
---
## Pourquoi apprendre le C

- Comprendre la gestion de la mémoire
  - Pas de garbage collector
  - Cas de programme Java !
- Bas-niveau
  - Pouvoir manipuler les données élémentaires, déplacement, etc -> overflow/underflow -> hack
  - Possibilité d'inclure du code assembleur
  - Pow(SIMD, Thread) !!!
- Indispenssable pour des programme fluide, robuste, performant
  - UNIX, Lunix, BSD, OSX, Android, Haiku, Casio, Embarqué et ... Windows
  - Unreal Engine, Cry Engine, Godot, Unity, Opengl, DirectX, ...
  - GTK, Qt, Firefox, Chrome, ...
  - 90% de l'informatique est basé en C/C++ (Java, Python, C#, ...)
  - 80% serveur unix

---
transition: fade-out
---
## Example

```c++
/* This may look non sense, but dont' pannic ! */
#include <stdlib.h>
#include <stdio.h>
#include "follow_the_white_rabbit.h"

#define MAX_SIZE 10
#define SQUARE(x) ((x) * (x))

#ifdef __linux
#define MAX_SIZE 12
#endif

int main(int argc, const char *argv[])
{
    // Declaration
    unsigned int i;
    
    /* Array initialization and loop */
    for (i = 0; i < MAX_SIZE; i++)
        printf("square of %d is %d", i, SQUARE(i));
        
    // Doest the program got an error ? 0=False
    return EXIT_SUCESS;
}
```

---
transition: fade-out
layout: image-left
image: /snippets/software-engineering-coding2.png
---
## Convention d'ecriture

Allman

```cpp
void drawCar()
{
    if (carVisible)
    {
        rect(x, y, w, h);
		    ...
    }
}
```

K&R
```cpp
void drawCar() {
    if (carVisible) {
        rect(x, y, w, h);
		    ...
    }
}
```
Convention a votre choix, y compris votre convention !

---
transition: slide-left
---

## Convention d'ecriture
- Style consistant !
  - Vous pouvez utiliser `clang-format`
  - Nommer correctement les fonction et variables (semantique)
  - Indentation (bloque)
  - Aérer les lignes du programme
  - Facilite la lecture !!!
- Pas de variables globales
- Commenter vos programmes
  - Pas de commentaire initile
- Attention sanction sur les notes de TP
- Example https://github.com/MaJerle/c-code-style

---
transition: fade-out
---
### -2
```cpp
#include<stdio.h>
#define MAX 1000
int a,b,c;float d;
char e[MAX];void F(int x){
if(x>0){printf("Positive");
}else{if(x<0){
printf("Negative");}else{printf("Zero");}}}
int main()
{
a=10;b=20;
c=a+b;
d=3.14159;
// This is a useless comment
printf("Hello World!");F(c);
for(int i=0;i<MAX;i++){e[i]='A';}
    if(d>3)
{
        printf("d is greater than 3");
    }
    else
        {
        printf("d is not greater than 3");
        }
return 0;
}
```

---
transition: slide-left
---

## Commentaires C89

```cpp
/* ceci est un commentaire */
/** ceci est une documentation / doxygen */
/*! ceci est une information (ex: parametre optionel) */
```
```cpp
/**********************
ceci est un commentaire
**********************/
/*
 * ceci est un commentaire
 */

/*/* ceci est un commentaire */
/* /* blabla */ ceci n'est pas un commentaire */
```

## Commentaires C99 et C++
```cpp
//  ceci est un commentaire
/// ceci est une documentation
//! ceci est une information (ex: parametre optionel)
```
```cpp
//
// ceci est un commentaire
//
```

---
transition: fade-out
layout: iframe
url: http://micro-os-plus.github.io/develop/doxygen-style-guide/
---