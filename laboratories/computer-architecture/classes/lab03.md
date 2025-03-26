## Instrukcje przesyłania danych, instrukcje arytmetyczne, rejestr znaczników

### Wprowadzenie
W asemblerze ARM operacje na danych wykonuje się przy użyciu dedykowanych instrukcji. Wśród nich wyróżniamy:

* **Instrukcje przesyłania danych** – umożliwiają przenoszenie wartości pomiędzy rejestrami oraz między rejestrami a pamięcią.

* **Instrukcje arytmetyczne** – wykonują operacje matematyczne (dodawanie, odejmowanie, mnożenie itp.) oraz w wersjach z sufiksem „S” aktualizują rejestr znaczników.

* **Rejestr znaczników (CPSR)** – specjalny rejestr, który przechowuje flagi (takie jak N, Z, C, V) informujące o wyniku operacji arytmetycznych oraz warunkach wykonania, co umożliwia podejmowanie decyzji o warunkowych skokach.

### Instrukcje przesyłania danych

Instrukcje te są podstawą komunikacji między rejestrami a pamięcią.

#### MOV - Przenoszenie danych

* **Składnia**: `MOV{S}{cond} Rd, <operand2>`
* **Opis**: Przenosi wartość z `<operand2>` do rejestru `Rd`.
* **Przykład**: `MOV R0, #10` – przenosi wartość 10 do rejestru `R0`.

```assembly
MOV R0, #25      @ Załadowanie liczby 25 do rejestru R0
MOV R1, R0       @ Kopiowanie zawartości R0 do R1
```

Komentarz: MOV nie modyfikuje flag w CPSR.

#### LDR - Ładowanie danych z pamięci

* **Składnia**: `LDR{cond}{B}{T} Rd, <adres>`
* **Opis**: Ładuje wartość spod adresu `<adres>` do rejestru `Rd`.
* **Przykład**: `LDR R0, [R1]` – ładuje wartość spod adresu w rejestrze `R1` do rejestru `R0`.

```assembly
LDR R2, [R3]     @ Ładuje wartość z pamięci, której adres znajduje się w R3, do R2
```

Komentarz: Rejestr R3 traktowany jest jako wskaźnik do adresu w pamięci.

#### STR – Zapis danych do pamięci

* **Składnia**: `STR{cond}{B}{T} Rd, <adres>`
* **Opis**: Zapisuje wartość z rejestru `Rd` pod adres `<adres>`.
* **Przykład**: `STR R0, [R1]` – zapisuje wartość z rejestru `R0` pod adres w rejestrze `R1`.

```assembly
STR R4, [R5]     @ Zapisuje zawartość R4 do pamięci pod adresem wskazywanym przez R5
```

Komentarz: Instrukcja ta umożliwia przesyłanie danych z rejestru do wybranej lokalizacji pamięci.

#### Instrukcje przesyłania danych w bloku

* **LDM (Load Multiple)**: Ładuje wiele rejestrów jednocześnie z pamięci.

* **STM (Store Multiple)**: Zapisuje zawartość wielu rejestrów do pamięci.

* **Przykład**:

```assembly
LDM R0!, {R1-R3}   @ Ładuje zawartość pamięci do rejestrów R1, R2 i R3, przy czym R0 jest automatycznie inkrementowany
STM R4!, {R5, R6}  @ Zapisuje zawartość rejestrów R5 i R6 do pamięci, aktualizując R4
```

Komentarz: Operator „!” wskazuje na automatyczną modyfikację adresu bazowego po wykonaniu operacji.

### Instrukcje arytmetyczne

Instrukcje te służą do wykonywania operacji matematycznych na danych przechowywanych w rejestrach.

#### Dodawanie - ADD i ADDS

* **ADD**: Sumuje dwie wartości bez zmiany flag.

* **ADDS**: Wersja instrukcji ADD, która oprócz dodawania aktualizuje rejestr znaczników (CPSR).

* **Przykład**: 

```assembly
ADD R1, R2, R3   @ R1 = R2 + R3, flagi w CPSR nie są modyfikowane
ADDS R1, R2, R3  @ R1 = R2 + R3, a CPSR (flagi N, Z, C, V) zostaje zaktualizowany
```

#### Odejmowanie - SUB i SUBS

* **SUB**: Odejmuje jedną wartość od drugiej.

* **SUBS**: Wersja odejmowania z aktualizacją flag w CPSR.

* **Przykład**:

```assembly
SUB R4, R5, R6   @ R4 = R5 - R6, flagi nie są zmieniane
SUBS R4, R5, R6  @ R4 = R5 - R6, a CPSR jest aktualizowany (np. ustawienie flagi Z, jeśli wynik wynosi 0)
```

#### Inne operacje arytmetyczne

* **MUL**: Mnożenie:

```assembly
MUL R0, R1, R2   @ R0 = R1 * R2
```

* **ADC**: Dodawanie z przeniesieniem:

```assembly
ADC R3, R4, R5   @ R3 = R4 + R5 + C, gdzie C to flaga przeniesienia z poprzedniej operacji
```

* **SBC**: Odejmowanie z przeniesieniem:

```assembly
SBC R3, R4, R5   @ R3 = R4 - R5 - (1 - C)
```

Komentarz: ADC i SBC wykorzystują flagę przeniesienia (C) do realizacji operacji na liczbach większych niż pojedynczy rozmiar rejestru.

### Rejestr znaczników (CPSR)

CPSR (Current Program Status Register) to rejestr systemowy, który zawiera informacje o stanie wykonania operacji arytmetycznych i logicznych. Jego główne flagi to:

* **N (Negative)**: Ustawiana, gdy wynik operacji jest ujemny.

* **Z (Zero)**: Ustawiana, gdy wynik operacji wynosi 0.

* **C (Carry)**: Informuje o przeniesieniu lub pożyczce przy operacjach arytmetycznych.

* **V (Overflow)**: Informuje o wystąpieniu przepełnienia (overflow) podczas operacji.

Flagi w CPSR są wykorzystywane do sterowania przepływem programu za pomocą instrukcji warunkowych. Przykłady takich instrukcji to:

* **BEQ (Branch if Equal)**: Skok, jeśli flaga Z jest ustawiona (wynik równy zero).

* **BNE (Branch if Not Equal)**: Skok, jeśli flaga Z nie jest ustawiona.

* **BPL (Branch if Plus)**: Skok, jeśli wynik jest dodatni (flaga N nie ustawiona).

Przykład użycia instrukcji CMP do ustawienia flag:

```assembly
CMP R1, R2      @ Porównuje zawartość R1 z R2 (de facto wykonuje odejmowanie bez zapisywania wyniku) i ustawia flagi
BEQ equal_label @ Skok do etykiety, jeśli R1 = R2 (flaga Z ustawiona)
BNE notequal   @ Skok, jeśli R1 != R2
```
Komentarz: Instrukcja CMP modyfikuje CPSR, dzięki czemu możliwe jest podejmowanie decyzji na podstawie wyniku porównania.

### Przykłady programów

#### Przykład 1: Operacje przesyłania danych

Program demonstruje wykorzystanie instrukcji MOV, LDR oraz STR do przesyłania danych między rejestrami a pamięcią:

```assembly
.text
.global _start
_start:
    MOV R0, #100      @ Ustawiamy wartość 100 w R0
    MOV R1, #200      @ Ustawiamy wartość 200 w R1
    STR R0, [R2]      @ Zapisujemy zawartość R0 do pamięci pod adresem wskazywanym przez R2
    LDR R3, [R2]      @ Ładujemy wartość z pamięci (adres w R2) do R3
    MOV R7, #1        @ Przygotowanie wywołania systemowego exit
    MOV R0, #0        @ Kod zakończenia (sukces)
    SVC 0             @ Wywołanie przerwania systemowego (zakończenie programu)
```

Komentarz: W przykładzie należy upewnić się, że rejestr R2 zawiera prawidłowy adres w pamięci – w przeciwnym razie operacje LDR/STR mogą wygenerować błąd.

#### Operacje arytmetyczne z aktualizacją CPSR

Program wykonuje operację dodawania z wykorzystaniem ADDS oraz porównanie wyniku z oczekiwaną wartością:

```assembly
.text
.global _start
_start:
    MOV R0, #15       @ Ustawiamy R0 = 15
    MOV R1, #15       @ Ustawiamy R1 = 15
    ADDS R2, R0, R1   @ R2 = R0 + R1 (15 + 15 = 30), CPSR aktualizowany
    CMP R2, #30       @ Porównanie R2 z wartością 30; aktualizacja flag w CPSR
    BEQ success       @ Skok do etykiety success, jeśli R2 wynosi 30 (flaga Z ustawiona)
    B   error        @ Skok do etykiety error w przeciwnym wypadku

success:
    MOV R7, #1
    MOV R0, #0       @ Kod zakończenia 0 (sukces)
    SVC 0

error:
    MOV R7, #1
    MOV R0, #1       @ Kod błędu
    SVC 0
```

Komentarz: Użycie ADDS umożliwia aktualizację CPSR, co pozwala instrukcji CMP określić, czy wynik operacji spełnia założone kryteria.

#### Przykład 3: Dodawanie z przeniesieniem (ADC) i odejmowanie z pożyczką (SBC)

Program ilustruje operacje arytmetyczne wykorzystujące flagę przeniesienia:

```assembly
.text
.global _start
_start:
    MOV R0, #0xFF      @ R0 = 255 (0xFF)
    MOV R1, #1         @ R1 = 1
    ADDS R2, R0, R1    @ R2 = 255 + 1; operacja ustawia flagę C (przeniesienie) oraz inne flagi CPSR
    ADC R3, R0, R1     @ R3 = 255 + 1 + flaga C (z poprzedniej operacji)
    MOV R7, #1
    MOV R0, #0
    SVC 0
```

Komentarz: ADDS oblicza sumę i ustawia flagi, które są następnie używane przez ADC do uwzględnienia przeniesienia, co jest szczególnie przydatne przy operacjach wielobitowych.

#### Przykład 4: Zaawansowane przesyłanie danych z użyciem LDM/STM

W tym przykładzie wykorzystamy instrukcje LDM i STM do przesłania grupy rejestrów. Pokażemy, jak załadować wartości do rejestrów z pamięci oraz jak zapisać zmodyfikowane dane z powrotem do pamięci.

```assembly
.text
.global _start
_start:
    @ Inicjalizacja wskaźnika pamięci (przyjmijmy, że R0 wskazuje na początek danych)
    MOV R0, #0x1000     @ Załóżmy, że dane znajdują się pod adresem 0x1000

    @ Ładujemy wartości z pamięci do rejestrów R1, R2, R3
    LDM R0!, {R1, R2, R3}  @ Automatycznie inkrementuje R0

    @ Wykonujemy operację arytmetyczną: dodajemy R1 i R2, wynik zapisujemy w R4
    ADD R4, R1, R2

    @ Modyfikujemy wartość – odejmujemy R3 od wyniku, zapisujemy w R4
    SUB R4, R4, R3

    @ Zapisujemy wynik operacji z powrotem do pamięci (np. do lokalizacji wskazywanej przez R0)
    STM R0!, {R4}      

    @ Zakończenie programu
    MOV R7, #1
    MOV R0, #0
    SVC 0
```

Komentarz: Instrukcje LDM i STM umożliwiają przesyłanie danych z/do wielu rejestrów jednocześnie, co jest przydatne w przypadku operacji na blokach danych.

#### Przykład 5: Operacje arytmetyczne i warunkowe skoki z aktualizacją CPSR

Przykład pokazuje użycie instrukcji ADDS, SUBS oraz CMP do wykonania operacji arytmetycznych i podejmowania decyzji na podstawie flag w CPSR.

```assembly
.text
.global _start
_start:
    MOV R0, #20        @ Ustawiamy R0 = 20
    MOV R1, #5         @ Ustawiamy R1 = 5

    @ Dodajemy liczby i aktualizujemy CPSR
    ADDS R2, R0, R1    @ R2 = 20 + 5 = 25

    @ Odejmujemy i aktualizujemy flagi
    SUBS R3, R2, R1    @ R3 = 25 - 5 = 20

    @ Porównujemy wynik z oczekiwaną wartością
    CMP R3, #20       @ Ustawia flagę Z, jeśli R3 == 20
    BEQ wynik_ok      @ Skok jeśli wynik poprawny

    B   blad          @ Skok do etykiety błędu, gdy wynik jest niepoprawny

wynik_ok:
    MOV R7, #1
    MOV R0, #0        @ Program zakończony sukcesem
    SVC 0

blad:
    MOV R7, #1
    MOV R0, #1        @ Kod błędu
    SVC 0
```

Komentarz: Dzięki użyciu ADDS i SUBS, CPSR zostaje zaktualizowany, co umożliwia sprawdzenie warunków (np. czy wynik operacji jest równy oczekiwanemu) przy pomocy instrukcji CMP i warunkowych skoków.

#### Przykład 6: Wykorzystanie flag CPSR w pętli

W tym przykładzie realizujemy prostą pętlę, która dekrementuje wartość w rejestrze, aż osiągnie zero. Instrukcja SUBS aktualizuje CPSR, a warunkowy skok sprawdza flagę Z (zero).

```assembly
.text
.global _start
_start:
    MOV R0, #10       @ Ustawiamy licznik pętli na 10

petla:
    SUBS R0, R0, #1   @ R0 = R0 - 1, aktualizacja flag CPSR
    CMP R0, #0        @ Porównujemy R0 z 0
    BNE petla         @ Jeśli R0 nie jest zerem (flaga Z nie ustawiona), powtarzamy pętlę

    @ Po zakończeniu pętli R0 wynosi 0
    MOV R7, #1
    MOV R0, #0        @ Zakończenie programu
    SVC 0
```

Komentarz: Pętla ta demonstruje, jak można wykorzystać flagi w CPSR do sterowania przepływem programu – szczególnie przydatne w implementacjach pętli oraz warunkowych struktur kontrolnych.

### Podsumowanie 

* **Instrukcje przesyłania danych:**

    * **MOV:** Kopiowanie danych między rejestrami lub ładowanie wartości natychmiastowych.

    * **LDR/STR:** Ładowanie danych z pamięci i zapisywanie ich do pamięci.

    * **LDM/STM:** Grupowe przesyłanie danych z/do wielu rejestrów.

* **Instrukcje arytmetyczne:**

    * **ADD/ADDS:** Dodawanie liczb z możliwością aktualizacji CPSR.

    * **SUB/SUBS:** Odejmowanie liczb, również z aktualizacją flag.

    * **MUL, ADC, SBC:** Rozszerzone operacje arytmetyczne przy użyciu flag przeniesienia.

* **Rejestr znaczników (CPSR):**

    * **Przechowuje flagi:** N (wynik ujemny), Z (wynik równy 0), C (przeniesienie/pożyczka) oraz V (przepełnienie).

    * **Flagi** te są wykorzystywane przez instrukcje porównujące (CMP) oraz warunkowe skoki (BEQ, BNE, BPL itd.), umożliwiając dynamiczną kontrolę przepływu programu.

Wskazówka: Eksperymentuj z podanymi przykładami w emulatorze (np. CPUlator) – modyfikuj wartości, kolejność operacji oraz obserwuj zmiany w rejestrach i CPSR. Praktyczne testy i analiza działania programu pomogą w lepszym zrozumieniu działania instrukcji na niskim poziomie.

### Listy zadań
1. [Lista nr 2 (oceniana)](../exercises/list02.md)