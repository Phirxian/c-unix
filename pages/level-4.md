<h1 class="text-center" style="position: relative;top: 50%;">Niveau 4</h1>
<p class="text-center" style="position: relative;top: 50%;">Persistence et flux de données</p>

---
transition: fade-out
---

- 🚣‍♂️ Les fichiers :
    - ouverture : `FILE* fopen (char *fname, char *mode)` (rwb)
    - fermeture : `fclose(FILE*)`
- 🌊 Sorti d'autres processus :
    - ouverture : `FILE* popen (char *fname, char *mode)` (rb)
    - fermeture : `pclose(FILE*)`
    - exemple : `popen("head -n 10 /proc/cpuinfo | grep name", "r");`
- 🚢 Les sockets:
    - ouverture : `sockfd = socket(AF_INET, SOCK_STREAM, 0);`
    - fermeture : `close(sockfd);`
    - (Plus de détails en programmation système)
- 🕵️‍♂️ Les interface system `/dev` :
    - ouverture : `device = fopen("/dev/sda1", "rb");`
    - (Plus de détails en programmation système)

---
transition: fade-out
layout: center
---

> <h3>Qu'est-ce qui ne va pas dans ce code ?</h3>

```cpp
FILE* file = fopen("important_data.txt", "w");
fprintf(file, "Données critiques");
```

---
transition: fade-out
---

- Lecture
    - Texte formaté : `int fscanf(FILE*, char *format, ...)`
    - Ligne de texte : `int fgets(char*, size_t len, FILE *fd)`
    - Données binaires : `size_t fread(void *buffer, size_t size, size_t count, FILE*)`
- Écriture : 
    - Texte formaté : `int fprintf(FILE*, char *format, ...)`
    - Ligne de texte : `int fputs(char* str, FILE *fd)`
    - Données binaires : `size_t fwrite(const void* buffer, size_t size, size_t count, FILE*)`
- Détetion d'erreur
    - `int ferror(FILE*)`
    - Retourne 0 si tout va bien ou une valeur non nulle en cas d'erreur

<br>

> fscanf() et fprintf() sont comme des serveurs qui prennent votre commande spécifique 🍽️ <br>
fgets() et fputs() sont le tapis roulant de sushi, vous prenez une ligne à la fois 🍣 <br>
fread() et fwrite() sont le buffet à volonté, vous vous servez autant que vous voulez 🍱 <br>

---
transition: fade-out
---

## Attention 

- Lecture
    - Lire 100 octets : Que se passe-t-il si le fichier source est plus petit ? 🤔
    - Avez vous les droits pour lire le fichier ? Existe t'il ? 🤔
- Écriture
    - Que se passe-t-il s'il n'y a plus de place sur le device ? 🤔
    - Avez vous les droits pour écrire dans le fichier ou le crée ? 🤔
- Position
    - Position actuelle : `long ftell(FILE* stream)`
    - Changement de position : `int fseek(FILE* stream, long offset, int origin)`
- Flush
    - `int fflush(FILE*)`
    - Force l'écriture du tampon dans le fichier
    - Assure la synchronisation des données