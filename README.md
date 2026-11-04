📂 Filesystem Visualizer

Manager de fișiere TUI (Text User Interface) în C pentru sisteme de operare Unix/Linux.


📌 Descriere Proiect

Aplicația oferă o interfață interactivă în consolă (inspirată de utilitarul Midnight Commander, dar cu un singur panou de vizualizare).

Permite explorarea rapidă a sistemului de fișiere direct din terminal, fără introducerea manuală a comenzilor de shell, oferind operațiuni de bază:

📁 Navigare prin directoare: navigare cu butoanele sageti, preluare comenzi de la tastatura, selectarea directorului cu tasta enter

📋 Copiere de fișiere

🚚 Mutare / Redenumire de fișiere: cu rename()

🗑️ Ștergere de fișiere: cu unlink() pt fisiere si rmdir() pt directoare

🗺️ Rezumatul Etapelor de Implementare

 [Etapa 1] Setup & ncurses
 [Etapa 2] POSIX Engine
 [Etapa 3] UI & Navigare
 [Etapa 4] Operații
 [Etapa 5] Validare & Testing


Configurarea mediului și inițializarea interfeței ncurses

Scanarea și citirea structurii sistemului de fișiere (POSIX Engine)

Randarea componentei vizuale și gestionarea navigării (UI & Events)

Implementarea operațiunilor de bază pe fișiere (Copiere, Mutare, Ștergere)

Tratarea erorilor, curățarea resurselor și testarea pe Linux

🛠️ Detalierea Etapelor de Implementare

🔹 Etapa 1: Setup Proiect & Inițializare Interfață Terminal

În ce constă task-ul:

Pregătirea mediului de dezvoltare, structurarea proiectului și configurarea stării inițiale a terminalului pentru modul grafic în consolă.

Cum plănuiesc să îl realizez:

Voi crea o aplicație C de bază care preia controlul ecranului de terminal, dezactivează afișarea automată a tastelor apăsate (echo) și activează citirea tastelor speciale (săgețile direcționale). La finalul execuției, terminalul va fi restaurat la starea inițială.

Instrumente și funcții utilizate:

Compilator: gcc / clang

Biblioteca ncurses: initscr(), noecho(), cbreak(), keypad(), endwin()



🔹 Etapa 2: Citirea și Preluarea Metadatelor Fișierelor

În ce constă task-ul:

Inspectarea directorului curent din sistemul de fișiere, parcurgerea elementelor și memorarea informațiilor despre fișiere și directoare.


O structură C (FileItem) pentru a stoca numele, dimensiunea și tipul fiecărui element (fișier vs. director). Voi parcurge conținutul directorului curent și voi popula un tablou cu aceste structuri.

Instrumente și funcții utilizate:

Inspecție directoare: opendir(), readdir(), closedir()

Metadate fișiere : stat() sau lstat(), macro-ul S_ISDIR()



🔹 Etapa 3: Interfața Grafică și Navigarea Interactivă

Desenarea listei pe ecran, adăugarea unui cursor vizual și gestionarea navigării din tastatură.

O funcție de randare care curăță ecranul, afișează calea directorului curent sus și listează fișierele linie cu linie. Elementul selectat va fi evidențiat prin culori inversate (A_REVERSE). Într-o buclă de evenimente, voi asculta tastele apăsate: Săgeți pentru navigare și Enter pentru intrarea într-un subdirector.

Instrumente și funcții utilizate:

Afișare ncurses: printw(), mvprintw(), attron(A_REVERSE), attroff(), getch(), clear(), refresh()

Schimbare director POSIX: chdir()



🔹 Etapa 4: Implementarea Operațiilor pe Fișiere

Implementarea funcționalităților de manipulare a fișierului selectat (ștergere, mutare, copiere) apelabile prin comenzi rapide de la tastatură.


Ștergere (Tasta D): Eliminarea fișierului de pe disc și reîmprospătarea listei.

Mutare / Redenumire (Tasta M): Citirea noii destinații și mutarea fișierului.

Copiere (Tasta C): Citirea fișierului sursă în bucăți de memorie (buffer de 4KB) și scrierea lor în fișierul de destinație.

Instrumente și funcții utilizate:
Ștergere: unlink(), rmdir() / remove()
Mutare: rename()
Copiere: open(), read(), write(), close()



🔹 Etapa 5: Tratarea Erorilor, Validare & Portabilitate

Prevenirea erorilor de rulare (ex. lipsă permisiuni), gestionarea cazurilor limită și asigurarea compatibilității pe Linux.

Adaugare de verificări pentru toate apelurile de sistem și afisare de mesaje de eroare în interfață dacă o operație eșuează efectuarea compilarii și testarea pe un mediu Linux.

Instrumente și funcții utilizate:
Verificare coduri de retur și errno
Flag-uri de compilare strictă: gcc -Wall -Wextra
