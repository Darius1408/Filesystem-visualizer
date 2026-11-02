# Filesystem Visualizer

Un manager de fișiere minimalist în consolă pentru sisteme de operare de tip Unix/Linux, dezvoltat în C utilizând biblioteca **ncurses** și apeluri de sistem **POSIX**. 

Proiectul este o aplicație TUI (*Text User Interface*) inspirată din utilitare precum *Midnight Commander*, oferind o interfață interactivă pentru navigarea și gestionarea fișierelor.

---

## 🏗️ Arhitectura Proiectului

Aplicația este structurată pe **4 module principale**:

```
+-------------------------------------------------------+
|                    BUCLA PRINCIPALĂ                   |
|           (Preluare taste & dispatcher)               |
+---------------------------+---------------------------+
                            |
         +------------------+------------------+
         |                                     |
         v                                     v
+-----------------------+           +-----------------------+
|  1. Modul UI          |           |  2. Modul Filesystem  |
|  (ncurses)            |           |  (POSIX Engine)       |
| - Randare fereastră   |           | - Citire directoare   |
| - Evidențiere cursor  |           | - Preluare metadate   |
+-----------------------+           +-----------------------+
                                               |
                                               v
                                    +-----------------------+
                                    |  3. Modul Operații    |
                                    | - Copiere (open/read) |
                                    | - Mutare (rename)     |
                                    | - Ștergere (unlink)   |
                                    +-----------------------+
```

---

## 🧩 Descrierea Modulelor

### 1. Structura de Date (Model)
Păstrează informațiile despre conținutul directorului curent în memorie:
* Tablou de structuri `FileItem` (nume, dimensiune, tip: fișier/director).
* Indexul elementului selectat curent (`selected_index`).

### 2. Modulul Filesystem (`opendir`, `readdir`, `stat`)
* Parcurge directorul curent și populează lista de fișiere.
* Extrage dimensiunea și tipul fiecărui fișier folosind apelul `stat()`.

### 3. Interfața Grafică (`ncurses`)
* Desenează cadrul ferestrei și calea directorului curent.
* Afișează lista de fișiere și evidențiază linia pe care se află cursorul.
* Afișează bara de instrucțiuni/comenzi în partea de jos a ecranului.

### 4. Modulul de Operații (Sistem de fișiere)
* **Copiere**: Citire și scriere în bucăți (buffers) folosind `open()`, `read()`, `write()`.
* **Mutare**: Redenumirea sau mutarea căii folosind `rename()`.
* **Ștergere**: Eliminarea fișierelor/directoarelor goale folosind `unlink()` sau `remove()`.

---

## 🔄 Fluxul de Execuție

1. **Inițializare**: Configurare `ncurses` (`initscr()`, `noecho()`, `cbreak()`, `keypad()`).
2. **Încărcare**: Citirea conținutului din directorul curent.
3. **Buclă de Evenimente (`Event Loop`)**:
   * Desenează ecranul (`draw_ui()`).
   * Așteaptă o tastă de la utilizator (`getch()`):
     * **Săgeți Sus / Jos**: Schimbă fișierul selectat.
     * **Enter**: Deschide un director selectat.
     * **Tasta C**: Inițiază copierea fișierului selectat.
     * **Tasta M**: Inițiază mutarea fișierului selectat.
     * **Tasta D**: Șterge fișierul selectat.
     * **Tasta Q**: Închide aplicația.
4. **Finalizare**: Eliberare resurse și restaurare terminal (`endwin()`).

---

## 🛠️ Compilare și Rulare

### Cerințe
* Compilator C (`gcc` sau `clang`)
* Biblioteca `ncurses` (preinstalată pe macOS/Linux)

### Compilare
```bash
gcc -Wall main.c -o visualizer -lncurses
```

### Rulare
```bash
./visualizer
```