# Tema 1 PA 2023

Made by Andrei Mărunțiș

> **Nota**: Acest readme este scris in markdown, deschide-l cu un editor specializat pentru o citire mai usoara.

## Problema 1 - Feribot

Solutia la aceasta problema este inspirata de [aici](https://www.geeksforgeeks.org/allocate-minimum-number-pages/).

Pasii rezolvarii sunt urmatorii:

- Dupa ce am citit datele, consider cazurile in care as avea costul minim, respectiv maxim:
    - costul minim il obtin daca am un numar de feriboturi egal cu numarul de masini, astfel costul va fi greutatea maxima dintre masini
    - costul maxim se obtine daca inghesui toate masinile pe un singur feribot, iar in acest caz costul va fi suma greutatilor tuturor masinilor
- raspunsul cautat (costul minim avand k feriboturi) se afla undeva intre valorile definite mai sus. Pentru a determina eficient costul corect, fac un fel de cautare binara (imi injumatatesc la fiecare pas domeniul de cautare, astfel):
    - Initial `stg=costMinim` si `dr=costMaxim`
    - Calculez mijlocul ca fiind media aritmetica `middle = (stg + dr) / 2`
    - Verific daca este posibil sa transport masinile cu un cost maxim egal cu `middle` (greutatea masinilor de pe fiecare feribot sa fie cel mult egala cu `middle`); fac asta parcurgand liniar vectorul de masini si facand sume partiale
    - Daca gasesc ca este posibil, atunci injumatatesc intervalul de cautare setand `dr = middle - 1` (caut doar solutii cu costul mai mic decat cel pe care l-am gasit deja ca fiind valid)
    - Altfel, injumatatesc intervalul de cautare setand `stg = middle + 1` (caut doar solutii cu costul mai mare decat cel pe care l-am verificat la pasul curent)
    - Continui pasii de mai sus pana cand `stg > dr`
- Ultimul raspuns valid este raspunsul corect al problemei

> Complexitatea solutiei: `O(n)`

##  Problema 2 - Nostory

### Task 1

Task-ul 1 este extrem de simplu. O abordare greedy este suficienta pentru a rezolva problema, caci fara nicio restrictie asupra numarului de mutari pot face mutarile asa incat scorul sa fie suma numerelor maxime din vector.

Pentru a face asta, sortez cei doi vectori (eu am facut asta separat), iar apoi am facut un soi de interclasare pentru a selecta numerele maxime din cei doi vectori.

> Complexitatea solutiei: `O(n * log n)`

### Task 2

La acest task se poate gasi usor o solutie, insa trebuie o solutie extrem de eficienta intrucat limita de timp este foarte mica.

Solutia pe care am implementat-o se bazeaza pe urmatoarea observatie: la fiecare pas, pentru a maximiza punctele, trebuie sa interschimb cel mai mare minim cu cel mai mic maxim (daca exista un minim mai mare decat un maxim).

Astfel, algoritmul de rezolvare este urmatorul:

- Imi construiesc doi vectori cu minimele, respectiv cu maximele din cele doua siruri
- Pentru eficienta calculez la acest pas si scorul (suma maximelor)
- Sortez cei doi vectori (eu i-am sortat pe ambii crecator)
- Avand vectorii sortati, acum am acces in timp liniar la cele doua valori (cel mai mare minim si cel mai mic maxim), ele fiind pe ultimele k pozitii din vectorul de minim, respectiv primele k pozitii din vectorul de maxim
- Parcurg primele k valori descrise mai sus, si la fiecare pas imi actualizez scorul ca si cum as fi interschimbat cel mai mare minim cu cel mai mic maxim (adun diferenta celor doua)
- Daca nu mai am minime mai mari decat maximele in vectorul meu, ma opresc

La finalul acestei parcurgeri, scorul va avea valoarea maxima.

> Complexitatea solutiei: `O(n * log n)`

## Problema 3 - Sushi

### Task 1

Problema aceasta devine exact problema rucsacului discreta, daca consider urmatoarele analogii:

- obiectele de luat = platourile, deci numarul de obiecte = nr de platouri
- greutatea maxima suportata de rucsac = bugetul total al prietenilor (`n * x`)
- valoarea obiectelor = suma notelor oferite de prieteni fiecarui platou de sushi

Avand in vedere observatiile de mai sus, aplic algoritmul prezentat in laboratorul 3 (de pe pagina de ocw).

> Complexitatea solutiei: `O(n * m * x)`

### Task 2

Acest task este foarte asemanator cu cel precedent. Unica modificare pe care o fac este ca dublez toate platourile de sushi (fiecare platou va aparea de 2 ori in vectorul de preturi, respectiv in vectorul de note).

> Complexitatea solutiei: `O(n * m * x)`

### Task 3

Task-ul acesta reprezinta problema rucsacului extinsa, solutia este inspirata de [aici](https://www.geeksforgeeks.org/extended-knapsack-problem/).

Diferentele fata de task-ul precedent sunt urmatoarele:

- la matricea mea de prgramare dinamica voi adauga o a 3a dimensiune - numarul de platouri pe care le-am comandat pentru solutia mea - caci acum trebuie sa tin cont de 3 restrictii diferite (bugetul, sa maximizez notele si limita de n platouri)
- la fiecare pas al recurentei incerc sa gasesc cate o solutie pentru 1, 2, ... n platouri cumparate
- la final, solutia problemei se va gasi in `dp[2 * m][totalMoney][n]` -> punctajul maxim pe care il pot obtine cu `n * x` bani, cumparand maxim n platouri, avand la dispozitie cele `2*m` platouri

> Complexitatea solutiei: `O(n^2 * m * x)`

## Problema 4 - Semnale

### Task 1

Pentru a rezolva problema de combinatorica folosesc urmatorul rationament:

- Am `x` biti de 0 si `y` biti de 1.
- Gandesc posibilele siruri astfel:
    - Pentru a ma asigura ca nu am doi biti de 1 alaturati, gandesc ca am sirul de `x` biti de 0 intre care pot sa asez (sau nu) cate un bit de 1
    - Spre exemplu, pentru 4 biti de 0 si 2 biti de 1, am sirul `_ 0 _ 0 _ 0 _ 0 _`, pe oricare dintre spatii poate sta sau nu un bit de 1
    - Problema se reduce la a numara in cate moduri pot aseza cei `y` biti de 1 pe cele `x + 1` pozitii libere; solutia este combinari de `x + 1` luate cate `y`
- Pentru a calcula combinarile folosesc formula cunoscuta `π(n-k+1...n)/π(1...k)` (`π(i...j)` inseamna produs de la i la j)

Dificultatea problemei consta in calculul impartirilor modulo `10^9+7`. Pentru a face asta se foloseste metoda descrisa [aici](https://www.geeksforgeeks.org/multiplicative-inverse-under-modulo-m/): 

Daca vreau sa calculez `(x / y) % mod` nu este corect sa efectuaz impartirea, trebuie sa calculez `(x * inv(y)) % mod`, `inv(y)` fiind inversul modular multiplicativ al lui `y` fata de `mod`.

Pentru a calcula acest invers se foloseste in mod normal algoritmul lui euclid extins descris [aici](https://www.geeksforgeeks.org/euclidean-algorithms-basic-and-extended/), insa exista o proprietate (prezentata [aici](https://www.geeksforgeeks.org/modular-division/)) care spune ca inversul modular multiplicativ al unui numar `y` modulo `mod` cu proprietatea ca `y` si `mod` sunt prime intre ele este egal cu `y^(mod - 2) % mod`.

Astfel, pentru a calcula inversul folosesc proprietatea de mai sus, iar pentru eficienta implementez exponentierea in timp logaritmic (cu injumatatirea exponentului la fiecare pas):

- `A^2k = (A^2)^k`
- `A^(2k + 1) = (A^2)^k * A`

> Complexitatea solutiei: `O(y)`

### Task 2

Acest task prezinta in plus fata de task-ul precedent o problema de combinatorica mai complexa, insa calculul in sine al combinarilor nu se modifica deloc.

Pentru a rezolva problema de combinatorica, folosesc urmatorul algoritm:

- Asemanator task-ului 1, consider sirul de `x` biti de 0 intre care pot sa pun 0, 1 sau 2 biti de 1
- Spre exemplu, pentru 4 biti de 0 am sirul `_ 0 _ 0 _ 0 _ 0 _`
- Initial, consider ca pun cel mult 1 bit de 1 in fiecare spatiu (am `y` grupuri de cate 1 bit de 1); solutia la aceasta problema este aceeasi cu task-ul 1
- La urmatorul pas:
    - consider ca am 1 grup de 2 biti de 1 alaturati si `y - 1` grupuri de cate 1 bit de 1, pe care incerc sa ii asez in cele `x + 1` pozitii
    - pot face acest lucru in combinari de `x + 1` luate cate `y - 1` moduri
    - de aceasta data, conteaza insa si ordinea in care asez aceste grupuri (intrucat ele nu mai sunt identice); astfel, trebuie sa inmultesc rezultatul de mai sus cu inca o valoare combinatoriala, anume combinari de `y - 1` luate cate 1
- In cazul general, considerand ca am `nr_groups` grupuri de 1 si 11 (in total) si `nr_double = y - nr_groups` grupuri de 11, efectuez urmatorul calcul:
    - pot sa aleg pozitiile pe care pun grupuri in combinari de `x + 1` luate cate `nr_groups` moduri
    - numarul de moduri in care pot alege ordinea in care pun grupurile pe cele `nr_groups` pozitii alese va fi combinari de `nr_groups` luate cate `nr_double`
    - astfel, numarul de siruri cu `nr_double` grupuri de 11 va fi produsul combinarilor de la punctele de mai sus
- Repet pasul de mai sus pentru toate valorile posibile pentru `nr_double = 0...nr_groups/2` si adun valorile obtinute astfel

> Complexitatea solutiei: `O(y^2)`

## Problema 5 - BadGPT

Se observa usor urmatoarea recurenta (absolut identica pentru u si n):

- Fie `u[i]` = numarul de siruri de u/w care se termina in u
- Fie `w[i]` = numarul de siruri de u/w care se termina in w
- Vreau sa calculez `u[i + 1]`, `w[i + 1]`:
    - Pot sa obtin un sir care se termina in u adaugand u la un sir care se termina in u sau w, asadar `u[i + 1] = u[i] + w[i]`
    - Pot sa obtin un sir care se termina in w doar adaugand u la un sir care se termina deja in u si transformand cei doi de u de la final in w, asadar: `w[i + 1] = u[i]`
- Observ astfel ca de fapt nu am nevoie de 2 vectori separati, intrucat `w[i] = u[i - 1]`, oricare ar fi i. Astfel, obtin noua recurenta:
    - `u[i + 1] = u[i] + u[i - 1]`
- Recurenta de mai sus este definitia sirului lui Fibonacci cu primele elemente `u[0] = 0`, `u[1] = 1`. (Sirul lui Fibonacci clasic)
- Numarul total de posibilitati in care un sir de lungime i poate fi intepretat va fi `total[i] = u[i] + w[i] = u[i] + u[i - 1]`.

Astfel, cerinta problemei necesita calculul sirului lui Fibonacci. Trebuie implementata a solutie eficienta atat din punct de vedere spatial, cat si temporal, pentru a respecta restrictia de timp (3s), respectiv limita de memorie a masinii.

Optimizarile pe care le-am facut sunt urmatoarele:

- Calculez elementele sirului lui Fibonacci folosind exponentierea matricilor in timp logaritmic (vezi explicatie la acest [link](https://www.nayuki.io/page/fast-fibonacci-algorithms), sectiunea **Matrix exponentiation**)
- La metoda de mai sus adaug calculul exponentierii in timp logaritmic folosind urmatoarele formule simple:
    - `A^2k = (A^2)^k`
    - `A^(2k + 1) = (A^2)^k * A`
- Sirul lui Fibonacci are proprietatea ca resturile sale modulo orice numar sunt periodice. Lungimile perioadelor modulo 1, 2, 3 etc. formeaza un sir numit sirul lui Pisano. Astfel, folosind codul scris in fisierul `Pisano.java` ([sursa](https://www.geeksforgeeks.org/fibonacci-number-modulo-m-and-pisano-period/)) am determinat (printr-ul calcul separat, "hardcodat") perioada Pisano a sirului lui Fibonacci modulo `10^9+7` si am obtinut perioada `2*10^9+16`. Astfel, a trebuit sa calculez valorile sirului lui Fibonacci pentru al maxim `2*10^9+16`-lea element.
- Am implementat si un mecanism de memoizare in solutia mea; pentru a calcula valorile lui `Fibo[i]` (al i-lea element din sirul lui Fibonacci) prin metoda exponentierei de matrici primesc si valorile `Fibo[i - 1]` si `Fibo[i + 1]` de care e posibil sa am nevoie mai tarziu. Retin aceste valori intr-un HashMap.

> Complexitatea solutiei: `O(G * log n)`
