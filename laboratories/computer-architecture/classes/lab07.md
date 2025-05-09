## Współpraca z urządzeniami zewnętrznymi w asemblerze ARM

### 1. Wprowadzenie – urządzenia peryferyjne i komunikacja

Urządzenia zewnętrzne (peryferyjne) to komponenty podłączone do procesora, które umożliwiają interakcję z otoczeniem – np. diody LED jako sygnalizatory, przyciski i przełączniki jako wejścia od użytkownika, wyświetlacze do prezentacji informacji czy interfejsy komunikacyjne (UART, SPI, I2C) do wymiany danych z innymi układami. Procesor komunikuje się z nimi głównie poprzez **rejestry wejścia/wyjścia (I/O)**, które odzwierciedlają stan urządzeń lub pozwalają sterować ich działaniem. W asemblerze (tak jak w innych językach) możemy odczytywać i zapisywać te rejestry, aby np. sprawdzić czy przycisk jest wciśnięty lub zapalić diodę LED. Dzięki temu nawet prosty program asemblerowy może reagować na sygnały z zewnątrz i wpływać na świat zewnętrzny.

W praktyce wyróżniamy dwa główne sposoby adresowania urządzeń peryferyjnych przez procesor: **wejścia/wyjścia izolowane (port-mapped I/O)** z osobną przestrzenią adresową (stosowane np. w architekturze x86) oraz **wejścia/wyjścia pamięciowo mapowane (memory-mapped I/O)**. W przypadku ARM i większości mikrokontrolerów stosuje się podejście memory-mapped I/O – oznacza to, że rejestry urządzeń peryferyjnych mają własne adresy w tej samej przestrzeni, co zwykła pamięć RAM. Procesor nie rozróżnia specjalnymi instrukcjami dostępu do urządzeń, tylko używa tych samych instrukcji co do pamięci (LDR/STR w asemblerze ARM). W ten sposób **obsługa I/O jest uproszczona** – wystarczy znać adres danego rejestru i odczytać/zapisać go odpowiednio, a sprzęt zajmie się resztą.

### 2. Zasada memory-mapped I/O – peryferia w przestrzeni adresowej

**Memory-mapped I/O** polega na przypisaniu każdemu zarejestrowanemu urządzeniu peryferyjnemu unikalnego zakresu adresów w przestrzeni adresowej procesora. Gdy procesor wykonuje zapis pod taki adres, sygnały na magistrali nie trafiają do pamięci RAM, lecz do kontrolera danego urządzenia, co skutkuje np. zmianą stanu wyjścia (zaświeceniem LED, wysłaniem bajtu przez UART, itp.). Analogicznie, odczyt spod adresu urządzenia zwraca bieżący stan jego rejestru (np. słowo z ustawieniami pinów lub bieżącymi odczytami).

W emulatorze **CPUlator** dla ARM mamy do czynienia właśnie z memory-mapped I/O. Urządzenia zewnętrzne (diody, przełączniki, klawiatura matrycowa, wyświetlacze 7-seg) są odwzorowane na konkretne adresy. Przykładowo, w systemie DE1-SoC emulowanym w CPUlatorze, **adres** `0xFF200000` odpowiada 10-bitowemu rejestrowi diod LED (LED0-LED9). Oznacza to, że zapisanie wartości binarnej pod ten adres zapala/zgasza poszczególne diody zgodnie z bitami tej wartości, a odczyt pozwala poznać ich aktualny stan (co zwykle odpowiada temu, co wcześniej zapisaliśmy, bo to wyjścia).

Podobnie **przełączniki suwakowe** (SW0–SW9) są podłączone do 10-bitowego rejestru pod adresem `0xFF200040`, a przyciski (KEY0–KEY3) do 4-bitowego rejestru pod adresem `0xFF200050`. Te rejestry są tylko do odczytu (read-only) – zapis nie ma wpływu na stan fizycznych przycisków/przełączników, za to odczyt zwraca 0 lub 1 na poszczególnych bitach w zależności od tego, czy dany przycisk/przełącznik jest wciśnięty/przesunięty (1 oznacza stan aktywny, np. wciśnięty przycisk, a 0 – stan nieaktywny). Z kolei **wyświetlacze siedmiosegmentowe** mają swoje rejestry pamięci: adres `0xFF200020` steruje czterema niższymi wyświetlaczami HEX0–HEX3, a `0xFF200030` dwoma wyższymi HEX4–HEX5. Każdy z tych rejestrów ma 32 bity, z których wykorzystane są grupy po 7 bitów na każdy wyświetlacz (odpowiadają segmentom a–g, a ósmy bit może odpowiadać kropce dziesiętnej lub pozostać niewykorzystany).

W ten sposób, znając **mapę adresową** peryferiów, możemy w asemblerze bezpośrednio odwoływać się do urządzeń. Np. zapis pod adres przełączników nic nie zrobi, ale odczyt z `0xFF200040` zwróci nam stan wszystkich 10 suwaków; zapis pod `0xFF200000` ustawi diody; odczyt z `0xFF200050` da stan 4 przycisków.

**Uwaga:** W architekturze ARM operacje na tych adresach odbywają się na słowach 32-bitowych. Każdy adres rejestru peryferyjnego jest wyrównany do słowa. Jeśli urządzenie ma mniej bitów (np. 10-bitowy port diod), to i tak zajmuje ono 32 bity – pozostałe bity są nieużywane (ignorowane przy zapisie, czytane zwykle jako 0 lub nieistotne) . Dlatego stosując instrukcje asemblerowe używamy np. `LDR/STR` (które domyślnie działają na 32-bitowych słowach). CPUlator upraszcza nam sprawę, bo domyślnie urządzenia są już zmapowane i aktywne – nie musimy nic inicjalizować, wystarczy odpowiedni adres.

### 3. Przykład – zapalanie diod LED

W tym przykładzie zapalimy diody LED na płytce DE1-SoC, korzystając z CPUlatora. Wykorzystamy do tego rejestr diod LED pod adresem `0xFF200000`. Wartość zapisana pod tym adresem odpowiada stanowi diod – 1 oznacza włączenie, 0 – wyłączenie.

**a) Kod asemblerowy:**
```asm
    .section .text
    .global main
main:
    LDR r0, =0xFF200000  @ Załaduj adres rejestru LED do r0
    MOV r1, #0xFF        @ Ustaw r1 na 0xFF (wszystkie diody ON)
    STR r1, [r0]        @ Zapisz wartość do rejestru LED
    B main              @ Powtarzaj w nieskończoność
```

**b) Opis kodu:**
* `LDR r0, =0xFF200000` – załaduj adres rejestru LED do rejestru `r0`,
* `MOV r1, #0xFF` – ustaw rejestr `r1` na wartość `0xFF` (wszystkie diody ON),
* `STR r1, [r0]` – zapisz wartość z `r1` do rejestru LED (adres w `r0`),
* `B main` – powtarzaj w nieskończoność (nieskończona pętla, aby program nie zakończył się).
* Wartość `0xFF` w rejestrze LED zapala wszystkie diody (LED0–LED7). Możemy zmieniać tę wartość, aby zapalać różne kombinacje diod.
* W CPUlatorze możemy obserwować stan diod LED w zakładce "I/O" – zmieniając wartość w rejestrze LED, zobaczymy, które diody są zapalone.
* Możemy również zmieniać wartość w rejestrze LED w czasie rzeczywistym, aby zobaczyć, jak zmienia się stan diod.
* Wartości w rejestrze LED są 32-bitowe, ale tylko 10 bitów jest używanych (LED0–LED9). Pozostałe bity są ignorowane.
* Możemy również użyć innych wartości, aby zapalać różne kombinacje diod. Na przykład `0x0F` zapali diody LED0–LED3, a `0xF0` zapali LED4–LED7.

### 4. Przykład – odczyt stanu przełączników

W tym przykładzie odczytamy stan przełączników suwakowych (SW0–SW9) i wyświetlimy go na diodach LED. Użyjemy rejestru przełączników pod adresem `0xFF200040`.
**a) Kod asemblerowy:**
```asm
    .section .text
    .global main
main:
    LDR r0, =0xFF200040  @ Załaduj adres rejestru SW do r0
    LDR r1, =0xFF200000  @ Załaduj adres rejestru LED do r1
loop:
    LDR r2, [r0]        @ Odczytaj stan przełączników do r2
    STR r2, [r1]        @ Zapisz stan przełączników do rejestru LED
    B loop              @ Powtarzaj w nieskończoność
```
**b) Opis kodu:**
* `LDR r0, =0xFF200040` – załaduj adres rejestru przełączników do rejestru `r0`,
* `LDR r1, =0xFF200000` – załaduj adres rejestru LED do rejestru `r1`,
* `LDR r2, [r0]` – odczytaj stan przełączników do rejestru `r2`,
* `STR r2, [r1]` – zapisz stan przełączników do rejestru LED (adres w `r1`),
* `B loop` – powtarzaj w nieskończoność (nieskończona pętla, aby program nie zakończył się).


### 5. Ogólny schemat współpracy z urządzeniem

```asm
    LDR r0, =ADRES_URZĄDZENIA  @ Załaduj adres rejestru urządzenia do r0
    MOV r1, #WARTOŚĆ          @ Ustaw wartość do zapisania w r1
    STR r1, [r0]              @ Zapisz wartość do rejestru urządzenia
    LDR r2, [r0]              @ Odczytaj stan rejestru urządzenia do r2
```