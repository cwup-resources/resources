# Zadania laboratoryjne - lista nr 4

### Zadanie 1 – Proste wywołanie procedury

Cel: Poznanie mechanizmu wywołania i powrotu z procedury.Polecenie: Napisz i przetestuj procedurę `add_numbers`, która:

* przyjmuje argumenty w R0, R1,

* oblicza ich sumę i zapisuje w R0,

* powraca do kodu głównego. Program główny powinien ustawić R0 i R1 na dowolne wartości, wywołać `add_numbers`, a następnie zatrzymać się w pętli nieskończonej.

### Zadanie 2 – Wartość bezwzględna

Cel: Ćwiczenie warunków i instrukcji `RSB`.
Polecenie: Zaimplementuj procedurę `absolute`, która:

* w R0 otrzymuje liczbę całkowitą,

* zwraca jej wartość bezwzględną w R0,

* dla liczb ujemnych wykorzystuje `RSB` (Reverse Subtract).

### Zadanie 3 – Proste porównanie i gałęzie

Cel: Ćwiczenie instrukcji `CMP` i skoków warunkowych.
Polecenie: Napisz procedurę `cmp_test`, która:

* w R0 i R1 otrzymuje dwie liczby,

* porównuje je za pomocą `CMP R0, R1`,

* jeśli R0 == R1 ustawia R2 = 1, w przeciwnym razie R2 = 0,

* zwraca sterowanie do `_start` i zatrzymuje program.

### Zadanie 4 – Maksimum dwóch liczb

Cel: Wykorzystanie instrukcji warunkowych w procedurze.
Polecenie: Zaimplementuj `max2`, która:

* w R0 i R1 otrzymuje dwie liczby,

* porównuje je, ustawiając w R0 większą z nich,

* zwraca sterowanie do `_start`.