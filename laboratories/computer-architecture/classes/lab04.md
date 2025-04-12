## Sterowanie przebiegiem programu w asemblerze ARM

### 1. Wprowadzenie

Sterowanie przebiegiem programu to mechanizm pozwalający zmienić *sekwencyjny* (ciągły) sposób wykonywania instrukcji przez procesor. W typowym programie, jeśli nie zastosujemy żadnych specjalnych instrukcji sterujących, procesor wykonuje polecenia *jedno po drugim* w kolejności rosnących adresów pamięci. Taki liniowy przepływ jest jednak niewystarczający do realizacji większości algorytmów. W praktyce potrzebujemy możliwości podejmowania decyzji (np. wykonania pewnego bloku kodu tylko przy spełnieniu określonego warunku) oraz powtarzania fragmentów kodu (pętle) czy wywoływania podprogramów. *Instrukcje sterujące przebiegiem programu* w asemblerze umożliwiają tworzenie konstrukcji takich jak instrukcje warunkowe `if/else`, pętle `while/for` czy wywołania funkcji i powroty z nich.

Na poziomie asemblera brak jest wysokopoziomowych konstrukcji sterujących znanych z języków takich jak C czy Python. Programista musi sam wykorzystać odpowiednie rozkazy procesora, aby zmienić wartość licznika rozkazów (PC, Program Counter) i skierować wykonywanie programu w inne miejsce. Do najważniejszych instrukcji tego typu należą *rozkazy skoku*(bezwzględnego i warunkowego) oraz *rozkazy wywołania podprogramu*. W architekturze ARM są to przede wszystkim instrukcje `B` (Branch), `BL` (Branch with Link), `BX` (Branch eXchange) oraz możliwość dołączenia do instrukcji kodów warunkowych (np. `BEQ`, `BNE`, `BGT` itd.). Kluczową rolę odgrywa tu również instrukcja porównania `CMP` (Compare), która ustawia *flagi warunkowe* w rejestrze statusu programu (CPSR) wykorzystywane następnie przez warunkowe skoki.

### 2. Instrukcje sterujące przebiegiem programu w ARM

#### Instrukcja B (Branch – skok bezwarunkowy)

*Instrukcja `B` (Branch)* – jest to podstawowa instrukcja skoku bezwarunkowego. Użycie B powoduje `bezwarunkową zmianę przepływu programu` poprzez załadowanie do rejestru PC nowego adresu wskazanego przez etykietę podaną jako operand. W praktyce `B label` wykonuje skok do instrukcji oznaczonej etykietą `label`. Po takim skoku procesor kontynuuje wykonywanie programu od wskazanego miejsca, zamiast kontynuować liniowo. Instrukcja `B` służy do implementacji prostego goto, do opuszczania/omijania fragmentów kodu, tworzenia pętli nieskończonych lub realizacji skoków warunkowych (gdy występuje w połączeniu z instrukcjami ustawiającymi odpowiednio PC lub flagi warunkowe).

* **Składnia**: `B{cond} label`  
* **Opis**: Bezwarunkowo zmienia przepływ programu, ustawiając rejestr PC na adres etykiety `label`.  
* **Zastosowanie**: Przekierowanie wykonywania programu (np. pominięcie fragmentu kodu).

*Przykład:*
```asm
.text
.global _start
_start:
    MOV R0, #5        @ Ustaw R0 na 5
    B skip            @ Skok bezwarunkowy do etykiety 'skip'
    MOV R0, #10       @ Ta instrukcja zostanie pominięta
skip:
    MOV R1, #1        @ Ustaw R1 na 1
    B .               @ Pętla nieskończona (zakończenie programu)
```

#### Instrukcja BL (Branch with Link – skok z linkiem)

*Instrukcja `BL` (Branch with Link)* – jest to skok bezwarunkowy z mechanizmem zapamiętania adresu powrotu. Instrukcja `BL label` powoduje skok do etykiety `label` `zapamiętując adres następnej instrukcji` w rejestrze `LR (Link Register)`. Rejestr LR (w architekturze ARM jest to R14) pełni rolę przechowania adresu powrotu z podprogramu. Dzięki temu po zakończeniu wykonywania podprogramu możemy wykonać instrukcję powrotu (np. `BX LR`), aby kontynuować od miejsca tuż za wywołaniem. `BL` służy zatem do wywoływania podprogramów (funkcji). Typowy schemat to: w kodzie głównym wywołać `BL nazwa_podprogramu`, a wewnątrz podprogramu na końcu wykonać `BX LR`, aby wrócić. Gdy nie potrzebujemy wracać (np. chcemy na stałe skoczyć w inne miejsce programu bez powrotu), wtedy używamy zwykłego `B` zamiast `BL`.

* **Składnia**: `BL{cond} label`  
* **Opis**: Wykonuje skok do etykiety `label` oraz zapisuje adres powrotu w rejestrze LR (R14). Umożliwia wywoływanie podprogramów.  
* **Zastosowanie**: Wywoływanie funkcji/podprogramów.

*Przykład:*
```asm
*Przykład:*
```asm
.text
.global _start
_start:
    MOV R0, #5        @ Ustaw R0 na 5
    B skip            @ Skok bezwarunkowy do etykiety 'skip'
    MOV R0, #10       @ Ta instrukcja zostanie pominięta
skip:
    MOV R1, #1        @ Ustaw R1 na 1
    B .               @ Pętla nieskończona (zakończenie programu)
```

#### Instrukcja BX (Branch eXchange – skok do adresu w rejestrze)

*Instrukcja `BX` (Branch eXchange)* – jest to instrukcja skoku bezwarunkowego do adresu znajdującego się w rejestrze. Najczęściej stosuje się ją do powrotu z podprogramu poprzez `BX LR`, co powoduje skok pod adres zapisany w rejestrze LR (uprzednio ustawiony przez `BL`). Instrukcja `BX Rn` może jednak wykonać skok pod dowolny adres przechowywany w rejestrze ogólnym `Rn` – umożliwia to np. realizację tablic skoków (dynamiczne wybieranie adresu docelowego z tablicy w pamięci lub na podstawie obliczeń). Nazwa "Branch eXchange" wynika stąd, że w starszych 32-bitowych procesorach ARM rozkaz ten oprócz skoku mógł zmieniać *stan pracy CPU* (ARM/Thumb) na podstawie najmłodszego bitu adresu. W kontekście naszego laboratorium można przyjąć, że `BX` po prostu wykonuje skok pod adres w rejestrze (bez dodatkowych skutków ubocznych). Typowym zastosowaniem `BX` jest właśnie powrót z funkcji (`BX LR`).

* **Składnia**: `BX Rn`  
* **Opis**: Skacze do adresu przechowywanego w rejestrze `Rn`. Najczęściej używana do powrotu z podprogramu (np. `BX LR`).  
* **Zastosowanie**: Powrót z funkcji, dynamiczne wyznaczanie adresu docelowego.

*Przykład:*
```asm
*Przykład:*
```asm
.text
.global _start
_start:
    MOV R0, #5        @ Ustaw R0 na 5
    B skip            @ Skok bezwarunkowy do etykiety 'skip'
    MOV R0, #10       @ Ta instrukcja zostanie pominięta
skip:
    MOV R1, #1        @ Ustaw R1 na 1
    B .               @ Pętla nieskończona (zakończenie programu)
```

#### Instrukcja CMP – Compare

*Instrukcja `CMP` (Compare)* – służy do porównania dwóch wartości. Składnia to `CMP <op1>, <op2>`, gdzie operandy mogą być rejestrami (lub rejestrem i wartością natychmiastową). Działanie `CMP` polega na *odjęciu* drugiego operandu od pierwszego (matematycznie: op1 – op2) i *ustawieniu flag* w rejestrze CPSR odpowiednio do wyniku tego odejmowania, *nie zapisując* jednak wyniku w żadnym rejestrze. Oznacza to, że wynik odejmowania zostaje odrzucony, a zachowane zostają jedynie bity statusu (flagi NZCV). W efekcie porównania ważne jest jedynie, jakie flagi zostały ustawione/wyzerowane. Przykładowo, jeśli porównywane wartości są równe, wynik odejmowania = 0, więc flaga *Z* (zero) zostanie ustawiona na 1. Jeśli pierwszy operand jest mniejszy od drugiego (przy interpretacji bez znaku, tzn. nastąpiła pożyczka przy odejmowaniu), flaga *C* zostanie wyzerowana. Instrukcja `CMP` sama `nie zmienia przepływu programu`, ale zwykle `poprzedza skok warunkowy`, dostarczając mu informacji do oceny warunku (poprzez ustawienie odpowiednich flag w CPSR). Warto zauważyć, że `CMP` nie modyfikuje swoich operandów – wpływa jedynie na rejestr statusu.

* **Składnia**: `CMP Rn, <operand2>`  
* **Opis**: Porównuje zawartość rejestru `Rn` z drugim operandem przez odejmowanie, ale **nie zapisuje** wyniku – jedynie aktualizuje flagi w CPSR.  
* **Zastosowanie**: Ustawienie flag warunkowych (N, Z, C, V) przed wykonaniem skoków warunkowych.

*Przykład:*
```asm
.text
.global _start
_start:
    MOV R0, #7        @ Ustawienie wartości w R0
    MOV R1, #7        @ Ustawienie wartości w R1
    CMP R0, R1        @ Porównanie R0 z R1 (ustawia flagę Z, jeśli równe)
    BEQ equal_label   @ Skok do etykiety 'equal_label', jeśli R0 == R1
    B   notequal      @ Skok, jeśli R0 != R1
equal_label:
    MOV R2, #1        @ Jeśli równe, ustaw R2 na 1
    B .               @ Pętla nieskończona (zakończenie programu)
```

#### Instrukcje skoku warunkowego (BEQ, BNE, BGT, BLT, BGE, BLE, ...)

Assembler ARM umożliwia wykonywanie skoków warunkowych w oparciu o stan flag CPSR. Przykładem są mnemotechniki takie jak `BEQ`, `BNE`, `BGT`, `BLT`, `BGE`, `BLE` i inne, które oznaczają skok (`B`) wykonywany tylko przy spełnieniu określonego warunku (podanego skrótem po `B`). Warunki te odpowiadają różnym kombinacjom flag (ustawionych np. przez wcześniejszą instrukcję `CMP`). Poniżej zestawienie podstawowych instrukcji warunkowych i ich znaczenia:

* **BEQ** Branch if Equal) – skok, gdy flaga *Z = 1* (wynik poprzedniego porównania był równy zero, czyli porównywane wartości były równe).
* **BNE** (Branch if Not Equal)  – skok, gdy flaga *Z = 0*(wartości porównane nie były równe).
* **BGE** – (Branch if Greater or Equal) – skok, gdy *N i V są równe* (co oznacza, że wynik pierwszej liczby był ≥ drugiej liczby w sensie signed, tzn. przy porównaniu ze znakiem nie wystąpiło sprzeczne przepełnienie – pierwsza wartość nie jest mniejsza od drugiej). 
* **BLT** (Branch if Less Than) – skok, gdy *N ≠ V* (pierwsza liczba < druga liczba w sensie signed, wystąpiło przepełnienie sygnalne świadczące o zmianie znaku wyniku).
* **BGT** (Branch if Greater Than) – skok, gdy *Z = 0 i N = V* (wynik porównania jest dodatni i nie wystąpiło przepełnienie).
* **BLE** (Branch if Less or Equal) – skok, gdy *Z = 1 lub N ≠ V* (pierwsza liczba ≤ druga ze znakiem; warunek "mniejsze lub równe" jest spełniony, gdy wynik porównania był zero lub nastąpiło przepełnienie sygnalne wskazujące na wynik ujemny).

Podane wyżej skoki warunkowe `Bxx` najczęściej wykonuje się zaraz po instrukcji `CMP`, aby na podstawie ustawionych flag zdecydować o dalszym przebiegu programu. Warunki większe/mniejsze tutaj odnoszą się do liczb ze znakiem (porównanie w arytmetyce *signed*). ARM udostępnia także osobne kody warunków dla porównań bez znaku: np. *BCS/BHS* (Branch if Carry Set / Branch if Higher or Same) oznacza "większe lub równe bez znaku" (flaga C = 1, interpretacja: brak pożyczki przy odejmowaniu), a *BCC/BLO* (Branch if Carry Clear / Branch if Lower) oznacza "mniejsze bez znaku" (flaga C = 0). Istnieją również warunki sprawdzające pojedyncze flagi, np. *BMI* (minus, N = 1), *BPL* (plus, N = 0), *BVS* (overflow set, V = 1), *BVC* (overflow clear, V = 0) i kilka innych. W codziennym programowaniu w asemblerze najczęściej spotyka się jednak warunki typu EQ/NE oraz te związane z wynikami porównań (GT, LT, itp.), gdyż odpowiadają one typowym konstrukcjom sterującym.

*Przykład:*
```asm
.text
.global _start
_start:
    MOV R0, #5        @ R0 = 5
    MOV R1, #10       @ R1 = 10
    CMP R0, R1        @ Porównaj R0 z R1
    BNE not_equal     @ Skok, jeśli R0 != R1 (flaga Z = 0)
    MOV R2, #1        @ Jeśli równe, ustaw R2 na 1
    B done
not_equal:
    MOV R2, #0        @ Jeśli nie równe, ustaw R2 na 0
done:
    B .               @ Pętla nieskończona (zakończenie programu)
```

### 3. Rejestr PC i flagi CPSR

*Rejestr PC (Program Counter)* – w procesorach ARM rejestr PC wskazuje na adres bieżącej lub następnej instrukcji do wykonania. Normalnie po wykonaniu każdej instrukcji PC jest automatycznie zwiększany tak, aby wskazywał kolejną instrukcję w pamięci (w trybie ARM zwiększa się o 4 bajty, czyli długość instrukcji). Instrukcje sterujące przebiegiem programu modyfikują zawartość PC, powodując skok wykonania. Na przykład `B label` wpisuje do PC adres etykiety `label` (czyli adres docelowej instrukcji), `BL label` robi to samo, ale dodatkowo zapisuje poprzednią wartość PC do LR (dla ewentualnego powrotu), zaś `BX Rm` wpisuje do PC wartość przechowywaną w rejestrze `Rm`. Tym samym instrukcje te przełamują domyślną sekwencyjność wykonywania rozkazów i zmieniają przebieg programu.

*Flagi CPSR:* W architekturze ARM bieżący stan warunków i trybu procesora przechowywany jest w rejestrze CPSR (Current Program Status Register). Nas interesują głównie cztery *flagi warunkowe*: *N* (negative), *Z* (zero), *C* (carry) i *V* (overflow), znajdujące się w najbardziej znaczących bitach CPSR. Są one ustawiane lub czyszczone przez instrukcje arytmetyczno-logiczne (w tym `CMP`) w zależności od wyniku działania:

* *N (Negative)* – ustawiana (N = 1), gdy wynik operacji jest ujemny (tzn. bit znakowy wyniku = 1). Przykładowo po `CMP R0, R1`, jeśli (R0 – R1) jest liczbą ujemną, to flaga N zostanie ustawiona.

* *Z (Zero)* – ustawiana (Z = 1), gdy wynik operacji jest równy zero. Np. gdy porównywane wartości są równe, `CMP` ustawi Z = 1.

* *C (Carry)* – flaga przeniesienia. Przy dodawaniu jest ustawiana (C = 1), gdy wynik wymaga przeniesienia poza najbardziej znaczący bit (nadmiar przy dodawaniu). Przy odejmowaniu (porównywaniu) flaga C = 1 oznacza, że nie było pożyczki – można to interpretować tak, że pierwszy operand ≥ drugi (w sensie unsigned). Jeśli `CMP R0, R1` spowoduje C = 0, oznacza to, że przy R0 – R1 nastąpiła pożyczka, czyli R0 < R1 (dla liczb dodatnich).

* *V (Overflow)* – flaga nadmiaru (przepełnienia arytmetycznego) dla operacji na liczbach ze znakiem. Ustawiana (V = 1), gdy wynik dodawania/odejmowania wykracza poza zakres reprezentacji (w kodzie U2 – uzupełnieniu do dwóch). Przykład: dodanie dwóch liczb dodatnich dało wynik ujemny lub odwrotnie, co oznacza przekroczenie zakresu reprezentacji 32-bitowej.

Instrukcje skoku *nie zmieniają* żadnych flag (tzn. `B`, `BL`, `BX` nie modyfikują CPSR). To instrukcje typu `CMP` lub inne instrukcje arytmetyczne z opcją ustawienia flag (np. `ADDS`, `SUBS`) wpływają na flagi, a skoki warunkowe jedynie odczytują stan tych flag, aby podjąć decyzję o ewentualnej zmianie PC. Dlatego typowy schemat to: *najpierw* wykonanie `CMP` (lub innej operacji ustawiającej flagi) w celu ustawienia flag na podstawie porównania, a *zaraz po nim* odpowiednia instrukcja `Bxx`, która sprawdza np. flagę Z lub kombinację N i V. Zrozumienie znaczenia flag N, Z, C, V jest kluczowe do poprawnego użycia warunkowych rozgałęzień programu – każda z tych flag informuje o określonym aspekcie wyniku ostatniej operacji i warunkowe skoki reagują właśnie na te informacje.

*Przykład:*
```asm
.text
.global _start
_start:
    MOV R0, #12       @ R0 = 12
    MOV R1, #8        @ R1 = 8
    CMP R0, R1        @ Porównanie: 12 - 8 = 4 (flaga Z = 0, N = 0)
    BEQ equal_case    @ Jeśli równe (Z = 1), skok
    B .               @ W przeciwnym razie zatrzymaj program
equal_case:
    MOV R2, #1        @ Ustaw R2 na 1, jeśli równe (teoretycznie)
    B .
```

### 4. Przykłady programów

#### Przykład 1: Skok bezwarunkowy

Program demonstruje, że instrukcja `B` powoduje pominięcie instrukcji znajdującej się między skokiem a etykietą docelową.

```asm
.text
.global _start
_start:
    MOV R0, #5        @ Ustaw R0 na 5
    B skip            @ Skok do etykiety skip
    MOV R0, #10       @ Ta instrukcja zostanie pominięta
skip:
    MOV R1, #1        @ Ustaw R1 na 1
    B .               @ Pętla nieskończona (zatrzymanie programu)
```

W powyższym kodzie po wykonaniu skoku `B skip` procesor przejdzie od razu do etykiety `skip`, pomijając instrukcję `MOV R0, #10`. Można to zaobserwować: wartość R0 pozostanie równa 5, a nie zostanie zmieniona na 10. Skok `B .` na końcu powoduje przejście do samej siebie (adres “.” oznacza bieżącą instrukcję), co tworzy pętlę zatrzymującą dalsze postępy programu (w warunkach symulatora to sposób na zakończenie wykonania).

#### Przykład 2: Instrukcja warunkowa (if/else)

Kod ilustruje prostą konstrukcję typu if/else. Do rejestrów R0 i R1 wpisane są dwie liczby. Za pomocą `CMP` porównujemy te wartości, a następnie wykorzystujemy `BLT` (branch if less than) aby rozgałęzić program w zależności od wyniku porównania:
jeśli R0 < R1, wykonywany jest blok kodu ustawiający R2 na wartość R1 (bo R1 jest większe); w przeciwnym razie (gdy R0 ≥ R1) wykonywany jest blok ustawiający R2 na R0. W ten sposób w R2 znajdzie się większa z dwóch liczb. Na końcu oba warianty zbiegną się w etykiecie `done`:

```asm
.global _start
_start:
    MOV R0, #3        @ przykładowa wartość 1: R0 = 3
    MOV R1, #7        @ przykładowa wartość 2: R1 = 7
    CMP R0, R1        @ porównaj R0 z R1 (ustawia flagi N,Z,C,V)
    BLT set_R1        @ jeśli R0 < R1, skocz do etykiety set_R1
    @ (jeśli skok nie nastąpił, to znaczy R0 >= R1)
    MOV R2, R0        @ R0 jest większe lub równe, ustaw R2 = R0
    B done            @ skok do końca, pominięcie bloku 'set_R1'
set_R1:
    MOV R2, R1        @ R0 < R1, więc ustaw R2 = R1 (większa wartość)
done:
    B .               @ koniec programu (pętla nieskończona)
```

Po uruchomieniu powyższego kodu i podstawieniu przykładowych wartości (R0 = 3, R1 = 7), rejestr R2 otrzyma wartość 7, czyli większą z dwóch. Warto prześledzić działanie: ponieważ 3 < 7, spełniony został warunek skoku `BLT`, więc nastąpiło przejście do etykiety `set_R1` i wykonanie `MOV R2, R1`. Gdyby wartości R0 i R1 były inne (np. R0 = 10, R1 = 7), skok warunkowy nie zostałby wykonany, więc program kontynuowałby liniowo i ustawił R2 = R0 (w tym przypadku 10). W obu sytuacjach etykieta `done` stanowi wspólny punkt dalszej kontynuacji programu.

#### Przykład 3: Wywołanie podprogramu

Poniższy kod zawiera podprogram (funkcję) realizujący dodawanie dwóch liczb. W rejestrach R0 i R1 program główny umieszcza argumenty (np. 10 i 4), następnie wywołuje podprogram `add_numbers` za pomocą `BL`. W podprogramie następuje obliczenie sumy i zapisanie wyniku do R0, po czym instrukcja `BX LR` powoduje powrót do programu głównego. Po powrocie w R0 znajduje się wynik dodawania (suma dwóch liczb). Program główny w tym przykładzie nie wykonuje już dalszych obliczeń – przechodzi w pętlę nieskończoną zatrzymując działanie:

```asm
.global _start
_start:
    MOV R0, #10       @ argument 1 = 10
    MOV R1, #4        @ argument 2 = 4
    BL add_numbers    @ wywołanie podprogramu dodającego liczby
    @ (po powrocie wynik sumy jest w R0)
    B .               @ koniec programu (pętla nieskończona)
add_numbers:
    ADD R0, R0, R1    @ oblicz R0 = R0 + R1 (suma dwóch argumentów)
    BX LR             @ powrót z podprogramu (skok pod adres w LR)
```

W powyższym przykładzie należy zwrócić uwagę, że `BL add_numbers` ustawia rejestr LR tak, by wskazywał na adres następnej instrukcji w programie głównym (czyli na `B .`). Podprogram `add_numbers` wykonuje działanie ADD, po czym `BX LR` powoduje skok z powrotem do głównego kodu. Po zakończeniu `add_numbers`, rejestr R0 zawiera sumę 10 + 4 = 14, co można zaobserwować w emulatorze. (Oczywiście w bardziej rozbudowanych programach po powrocie mogłyby następować kolejne operacje, tutaj dla prostoty następuje zakończenie programu poprzez pętlę nieskończoną).

#### Przykład 4

```asm
.text
.global _start
_start:
    MOV R0, #5        @ Ustaw R0 na 5
    B skip            @ Skok bezwarunkowy do etykiety 'skip'
    MOV R0, #10       @ Ta instrukcja zostanie pominięta
skip:
    MOV R1, #1        @ Ustaw R1 na 1
    B .               @ Pętla nieskończona (zakończenie programu)
```

#### Przykład 5

```asm
.text
.global _start
_start:
    MOV R0, #1        @ Ustawienie wartości 1 w R0
    B first           @ Skok do etykiety 'first'
second:
    MOV R1, #2        @ Ta instrukcja nie zostanie wykonana
    B end
first:
    MOV R0, #10       @ Zmiana wartości R0 na 10
    B second          @ Skok do etykiety 'second'
end:
    B .               @ Zakończenie programu (pętla nieskończona)
```

#### Przykład 6

```asm
.text
.global _start
_start:
    MOV R0, #10       @ Pierwszy argument = 10
    MOV R1, #4        @ Drugi argument = 4
    BL add_numbers    @ Wywołanie podprogramu 'add_numbers'
    B .               @ Zakończenie programu (pętla nieskończona)
add_numbers:
    ADD R0, R0, R1    @ R0 = R0 + R1
    BX LR             @ Powrót z podprogramu
```

#### Przykład 7

```asm
.text
.global _start
_start:
    MOV R0, #3        @ Liczba powtórzeń (mnożnik)
    MOV R1, #5        @ Mnożna = 5
    BL multiply       @ Wywołanie podprogramu 'multiply'
    B .               @ Zakończenie programu
multiply:
    ; Zakładamy, że wynik zaczyna się w R2 = 0
    MOV R2, #0
    ADD R2, R2, R1    @ Pierwsze dodanie: R2 = 5
    ADD R2, R2, R1    @ Drugie dodanie: R2 = 10
    ADD R2, R2, R1    @ Trzecie dodanie: R2 = 15
    BX LR             @ Powrót z podprogramu
```

#### Przykład 8

```asm
.text
.global _start
_start:
    MOV R0, #15       @ Ustawienie argumentu
    MOV R1, #5        @ Ustawienie drugiego argumentu
    BL subtract_numbers  @ Wywołanie podprogramu odejmującego
    B .               @ Zakończenie programu
subtract_numbers:
    SUB R0, R0, R1    @ R0 = R0 - R1
    BX LR             @ Powrót do miejsca wywołania
```

#### Przykład 9

```asm
.text
.global _start
_start:
    MOV R0, #label_target  @ Załaduj adres etykiety do R0
    BX R0                  @ Skok do adresu przechowywanego w R0
label_target:
    MOV R1, #99            @ Ustawienie R1 dla demonstracji
    B .                    @ Zakończenie programu
```

#### Przykład 10

```asm
.text
.global _start
_start:
    ; Ustawienie opcji kalkulatora w R0
    ; 1 = dodawanie, 2 = odejmowanie, 3 = mnożenie
    MOV R0, #2         @ Przykładowo: wybieramy opcję 2 (odejmowanie)

    ; Sprawdzenie opcji i wykonanie odpowiedniej operacji
    CMP R0, #1
    BEQ op_add         @ Jeśli R0 == 1, wykonaj dodawanie
    CMP R0, #2
    BEQ op_sub         @ Jeśli R0 == 2, wykonaj odejmowanie
    CMP R0, #3
    BEQ op_mul         @ Jeśli R0 == 3, wykonaj mnożenie
    B default_case     @ W przeciwnym razie przejdź do przypadku domyślnego

op_add:
    MOV R1, #10        @ Pierwszy operand dla dodawania
    MOV R2, #5         @ Drugi operand dla dodawania
    ADD R3, R1, R2     @ R3 = 10 + 5 = 15
    B exit_prog

op_sub:
    MOV R1, #10        @ Pierwszy operand dla odejmowania
    MOV R2, #5         @ Drugi operand dla odejmowania
    SUB R3, R1, R2     @ R3 = 10 - 5 = 5
    B exit_prog

op_mul:
    MOV R1, #10        @ Pierwszy operand dla mnożenia
    MOV R2, #5         @ Drugi operand dla mnożenia
    MUL R3, R1, R2     @ R3 = 10 * 5 = 50
    B exit_prog

default_case:
    MOV R3, #0         @ W przypadku nieznanej opcji, wynik = 0

exit_prog:
    ; Zakończenie programu
    MOV R7, #1         @ Kod systemowy zakończenia (exit)
    MOV R0, #0         @ Kod wyjścia 0 (sukces)
    SVC 0
```

### Listy zadań
1. [Lista nr 3](../exercises/list03.md)