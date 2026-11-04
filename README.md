Filesystem Visualizer

Descriere Proiect

Aplicație de tip TUI (Text User Interface) dezvoltată în limbajul C pentru sisteme de operare Unix/Linux, creată ca proiect la disciplina Sisteme de Operare.

Proiectul oferă un manager de fișiere minimalist în consolă (inspirat de utilitarul Midnight Commander, dar simplificat la un singur panou de vizualizare). Aplicația permite explorarea interactivă a directoarelor și executarea operațiunilor de bază asupra fișierelor: copiere, mutare și ștergere.

Rezumatul Etapelor de Implementare

Configurarea mediului și inițializarea interfeței ncurses

Scanarea și citirea structurii sistemului de fișiere (POSIX Engine)

Randarea componentei vizuale și gestionarea navigării (UI & Events)

Implementarea operațiunilor de bază pe fișiere (Copiere, Mutare, Ștergere)

Tratarea erorilor, curățarea resurselor și testarea pe Linux

Detalierea Etapelor de Implementare

Etapa 1: Setup Proiect & Inițializare Interfață Terminal

În ce constă task-ul: Pregătirea mediului de dezvoltare, structurarea proiectului și configurarea stării inițiale a terminalului pentru modul grafic în consolă.

Cum plănuiesc să îl realizez: Voi crea o aplicație C de bază care preia controlul ecranului de terminal, dezactivează afișarea automată a tastelor apăsate (echo) și activează citirea tastelor speciale (săgețile direcționale). La finalul execuției, terminalul va fi restaurat la starea inițială.

Instrumente și funcții utilizate:

Compilator C (gcc / clang).

Biblioteca ncurses: initscr(), noecho(), cbreak(), keypad(), endwin().

Etapa 2: Citirea și Preluarea Metadatelor Fișierelor

În ce constă task-ul: Inspectarea directorului curent din sistemul de fișiere, parcurgerea elementelor și memorarea informațiilor despre fișiere și directoare.

Cum plănuiesc să îl realizez: Voi defini o structură C (FileItem) pentru a stoca numele, dimensiunea și tipul fiecărui element (fișier obișnuit vs. director). Voi parcurge conținutul directorului curent și voi popula un tablou dynamic sau static cu aceste structuri.

Instrumente și funcții utilizate:

Inspecție directoare (<dirent.h>): opendir(), readdir(), closedir().

Metadate fișiere (<sys/stat.h>): stat() sau lstat(), macro-ul S_ISDIR pentru detectarea directoarelor.

Etapa 3: Interfața Grafică și Navigarea Interactivă

În ce constă task-ul: Desenarea meciului/listei pe ecran, adăugarea unui cursor vizual și gestionarea navigării din tastatură.

Cum plănuiesc să îl realizez: Voi crea o funcție de randare draw_ui() care curăță ecranul, afișează calea directorului curent în partea de sus și listează fișierele linie cu linie. Elementul aflat la poziția cursorului va fi evidențiat prin culori inversate. Într-o buclă principală de evenimente, voi asculta tastele introduse: Săgeți Sus/Jos pentru deplasarea cursorului și Enter pentru intrarea într-un subdirector sau schimbarea directorului.

Instrumente și funcții utilizate:

Afișare ncurses: printw(), mvprintw(), attron(A_REVERSE), attroff(), getch(), clear(), refresh().

Schimbare director POSIX (<unistd.h>): chdir().

Etapa 4: Implementarea Operațiilor pe Fișiere

În ce constă task-ul: Implementarea funcționalităților de manipulare a fișierului selectat (ștergere, mutare, copiere) apelabile prin comenzi rapide de la tastatură.

Cum plănuiesc să îl realizez:

Ștergere (Tasta D): Preluarea numelui fișierului selectat și eliminarea lui de pe disc, urmată de reîmprospătarea listei.

Mutare / Redenumire (Tasta M): Citirea noii destinații printr-un mic prompt în interfață și mutarea fișierului.

Copiere (Tasta C): Citirea fișierului sursă în bucăți de memorie (buffer) și scrierea lor într-un fișier nou creat la destinație.

Instrumente și funcții utilizate:

Ștergere: unlink() pentru fișiere, rmdir() / remove().

Mutare: rename().

Copiere: open(), read(), write(), close() din <fcntl.h> și <unistd.h>.

Etapa 5: Tratarea Erorilor, Validare & Portabilitate

În ce constă task-ul: Prevenirea erorilor de rulare (ex. lipsă permisiuni), gestionarea cazurilor limită și asigurarea compatibilității complete pe Linux.

Cum plănuiesc să îl realizez: Voi adăuga verificări pentru toate apelurile de sistem (ex: verificare dacă opendir() a reușit) și voi afișa un mesaj de eroare în interfață dacă o operație e eșuată. Înainte de predare, voi efectua o compilare și testare pe un mediu Linux (Ubuntu/Debian).

Instrumente și funcții utilizate:

Verificare coduri de retur și errno.

Flag-uri de compilare strictă: gcc -Wall -Wextra.
