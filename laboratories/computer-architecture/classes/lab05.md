## Procedury w asemblerze ARM

### 1. Wprowadzenie

Procedura (funkcja) to wyodrębniony fragment kodu realizujący określone zadanie, który można wielokrotnie wywoływać z różnych miejsc programu. Dzięki temu unika się powielania instrukcji, poprawia czytelność kodu oraz umożliwia tworzenie rozwiązań rekurencyjnych.

W asemblerze ARM procedury działają podobnie jak funkcje w językach wysokiego poziomu, lecz mechanizm wywołania i powrotu wymaga jawnej obsługi:

* **`LR`** (Link Register, R14) – przechowuje adres powrotu z procedury.

* **`BL labe`l** (Branch with Link) – wykonuje skok do etykiety `label` i zapisuje w LR adres instrukcji następującej po `BL` (adres powrotu).

* **`BX LR`** (Branch and Exchange) – powoduje skok pod adres zapisany w `LR`, co realizuje powrót z procedury.

ARM Procedure Call Standard (**AAPCS**) definiuje sposób przekazywania argumentów i zachowywania rejestrów:

* Argumenty 1–4 przekazywane są w **R0–R3** (dalsze przez stos).

* Wynik funkcji zwracany jest w **R0**.

* **Caller‑saved**: R0–R3, R12 – mogą być zmieniane przez procedurę.

* **Callee‑saved**: R4–R11 – procedura musi je zabezpieczyć i przywrócić.

### 2. Wywołanie i wykonanie procedury

#### 2.1 Przekazanie argumentów

```asm
    MOV R0, #a    @ pierwszy argument
    MOV R1, #b    @ drugi argument
```

#### 2.2 Wywołanie procedury

```asm
    BL my_function @ wywołanie procedury
```

#### 2.3 Kod procedury

* Odczyt argumentów z R0–R3

* Logika biznesowa: obliczenia, pętle, kolejne wywołania funkcji

#### 2.4 Powrót z procedury

```asm
    BX LR          @ powrót z procedury
```

#### 2.5 Kontynuacja wywołującego

Wykonanie instrukcji następującej po `BL`, odczyt wyniku z R0, kontynuacja programu.

### 3. Przykład

Poniższy kod demonstruje prostą procedurę `add_numbers`, która przyjmuje dwa argumenty w R0 i R1, sumuje je i zwraca wynik w R0. Program kończy się pętlą nieskończoną:

```asm
.text
.global _start
_start:
    MOV     R0, #10        @ Pierwszy argument = 10
    MOV     R1, #4         @ Drugi argument   = 4
    BL      add_numbers    @ Wywołanie procedury 'add_numbers'
    B       .              @ Pętla nieskończona – koniec programu

add_numbers:
    ADD     R0, R0, R1     @ R0 = R0 + R1
    BX      LR             @ Powrót do miejsca wywołania
```

### Listy zadań
1. [Lista nr 4](../exercises/list04.md)