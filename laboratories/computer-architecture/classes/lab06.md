## Łączenie programu w języku wysokopoziomowym z procedurami w asemblerze ARM

### 1. Wprowadzenie

W programowaniu systemów wbudowanych często zachodzi potrzeba połączenia kodu pisanego w języku wysokiego poziomu (np. C) z procedurami w asemblerze. Taki podział może być motywowany:

* **wydajnością** – fragmenty asemblera realizują krytyczne czasowo operacje (np. obsługa przerwań, pętle DSP),

* **dostępem do niskopoziomowego sprzętu** – asembler pozwala precyzyjnie kontrolować rejestry i instrukcje,

* **współdzieleniem kodu** – np. wykorzystanie bibliotek asemblerowych w różnych projektach.

W ARM takie połączenie jest naturalnie wspierane. Kompilator języka C (np. `gcc-arm`) pozwala:

* wywoływać funkcje asemblerowe z kodu C,

* wywoływać funkcje C z kodu asemblerowego,

* wstawiać kod asemblerowy bezpośrednio w funkcji C (tzw. inline assembly),

* kompilować osobno pliki `.s/.S` (asembler) i `.c` (C), a następnie łączyć je razem (`linking`).

### 2. Konwencja wywołań funkcji w ARM (AAPCS)

Aby poprawnie przekazywać dane między C a asemblerem, należy znać **konwencję wywoływania funkcji (ABI)**. Dla ARM (ARM Architecture Procedure Call Standard – AAPCS) obowiązują następujące zasady:

* **Argumenty** przekazywane są w rejestrach `r0` do `r3` (jeśli jest ich więcej, pozostałe przekazywane są na stosie),

* **Wartość zwracana** (return) trafia do `r0`,

* **Rejestry** `r0–r3`, `r12`, `lr` są **rejestrami tymczasowymi** – mogą zostać nadpisane,

* **Rejestry** `r4–r11` **muszą być zachowane** (callee-saved) – jeśli funkcja ich używa, musi zapisać/odtworzyć ich wartość,

* Stos rośnie w **kierunku malejących adresów**,

* Przed wywołaniem funkcji używa się instrukcji `BL` (Branch with Link), która zapisuje adres powrotu do rejestru `LR`.

### 3. Wywoływanie procedury asemblerowej z języka C

**a) Kod C:**

```c
extern int asm_add(int a, int b);  // Deklaracja funkcji napisanej w asemblerze

int main() {
    int wynik = asm_add(3, 4);     // Wywołanie procedury asemblerowej
    while(1);                      // zatrzymaj program
}
```

**b) Kod asemblerowy:**

```asm
    .global asm_add
asm_add:
    ADD r0, r0, r1     @ r0 = r0 + r1 (argumenty w r0 i r1)
    BX  lr             @ powrót z funkcji (adres w LR)
```

* Funkcja `asm_add` przyjmuje dwa argumenty (a, b) w rejestrach `r0`, `r1`,

* Zwraca wynik w `r0` – konwencja ARM ABI,

* Nie używa innych rejestrów – nie trzeba nic zapisywać na stos.

### 4. Wywoływanie funkcji w języku C z kodu asemblerowego

Jeśli chcemy z poziomu kodu asemblerowego wywołać funkcję C, wystarczy:

* zadbać o poprawne przekazanie argumentów w r0–r3,

* wywołać funkcję BL nazwafunkcji,

* odebrać wynik z r0.

**a) Kod C:**

```c
int c_multiply(int a, int b) {
    return a * b;
}
```

**b) Kod asemblerowy:**

```asm
    .global _start
    .extern c_multiply

_start:
    MOV r0, #6         @ pierwszy argument: 6
    MOV r1, #7         @ drugi argument: 7
    BL  c_multiply     @ wywołanie funkcji C
    MOV r2, r0         @ wynik znajduje się w r0 – przepisz do r2
    B  .               @ pętla kończąca
```

### 5. Wstawki asemblerowe w języku C (inline asm)

Można też umieszczać fragmenty kodu asemblerowego bezpośrednio w kodzie C:

```c
int x = 5, y = 3, z;
__asm__(
    "ADD %[result], %[val1], %[val2]"
    : [result] "=r" (z)
    : [val1] "r" (x), [val2] "r" (y)
);
```

* Wstawka `__asm__` (lub `asm`) pozwala napisać kod asemblerowy bezpośrednio w funkcji,

* Można korzystać z wartości C jako operandów (`x`, `y`, `z`),

* Kompilator sam zadba o użycie odpowiednich rejestrów.

### 6. Kompilacja i łączenie

W typowym systemie (np. z użyciem `arm-none-eabi-gcc`):

```bash
arm-none-eabi-as asm_add.s -o asm_add.o       # asembler
arm-none-eabi-gcc main.c asm_add.o -o prog.elf  # kompilacja i linkowanie razem
```

W CPulatorze można wrzucać kod asemblera i C w osobnych zakładkach – **interfejs emulatora automatycznie łączy te pliki.**

### 7. Zastosowania w systemach wbudowanych

* C odpowiada za logikę wysokiego poziomu (np. obsługa protokołów, logika programu),

* Asembler przejmuje kontrolę nad:

    * obsługą przerwań (ISR),

    * szybkimi operacjami bitowymi,

    * optymalizacją pętli (np. w DSP, obsłudze sterowników),

    * bezpośrednim dostępem do portów GPIO.