# Zadania laboratoryjne - lista nr 2

## Zadanie 1 – Proste przesyłanie danych (MOV)

**Cel:** Poznać działanie instrukcji MOV.

**Polecenie:**
- Załaduj wartość natychmiastową (np. 42) do rejestru R0.
- Skopiuj tę wartość z R0 do R1.
- Zakończ program wywołując systemowe zakończenie (sys_exit).

---

## Zadanie 2 – Praca z pamięcią (LDR/STR)

**Cel:** Ćwiczenie operacji ładowania i zapisu do pamięci.

**Polecenie:**
- Załaduj wartość natychmiastową (np. 100) do rejestru R0.
- Przenieś wartość z R0 do pamięci (adres określony przez R2 – ustaw R2 na przykładowy adres, np. 0x1000).
- Następnie, przy użyciu instrukcji LDR, odczytaj tę wartość do rejestru R1.
- Zakończ program.

---

## Zadanie 3 – Operacje arytmetyczne z aktualizacją CPSR

**Cel:** Wykorzystanie instrukcji ADDS, SUBS oraz porównania (CMP) bez pętli.

**Polecenie:**
- Załaduj dwie liczby (np. 30 i 10) do rejestrów R0 i R1.
- Wykonaj operację dodawania z aktualizacją flag (ADDS) i zapisz wynik w R2.
- Następnie odejmij drugą liczbę od wyniku przy użyciu SUBS, zapisując wynik w R3.
- Porównaj wartość w R3 z oczekiwaną wartością (np. 30) używając CMP.
- W zależności od wyniku porównania, wykonaj warunkowy skok do etykiety sukces lub błędu, a następnie zakończ program.

---

## Zadanie 4 – Obliczenie wyrażenia matematycznego

**Cel:** Przećwiczenie sekwencyjnego wykorzystania instrukcji arytmetycznych i przesyłania danych.

**Polecenie:**
- Załaduj następujące wartości:
  - A = 15 (do R0)
  - B = 25 (do R1)
  - C = 10 (do R2)
  - D = 5 (do R3)
- Oblicz wyrażenie: **(A + B) - (C - D)**
  - Najpierw dodaj A i B, zapisując wynik w R4.
  - Następnie odejmij D od C, zapisując wynik w R5.
  - Na końcu odejmij wartość z R5 od R4, zapisując wynik w R6.
- Porównaj wynik w R6 z wartością oczekiwaną (np. 35) używając CMP i wykonaj warunkowy skok do etykiety sukces lub błędu.
- Zakończ program.

---

## Zadanie 5 – Złożona operacja arytmetyczna – symulacja mnożenia przez ręczne dodawanie

**Cel:** Zastosowanie instrukcji przesyłania danych i arytmetycznych w sekwencyjnych krokach bez pętli.

**Polecenie:**
- Ustal dwie liczby, które chcesz „pomnożyć” – mnożna (np. 7) oraz mnożnik (np. 3).
- Wykonaj ręcznie trzy kolejne dodania mnożnej do akumulatora, aby uzyskać wynik mnożenia (czyli: wynik = 7 + 7 + 7):
  - Załaduj wartość mnożnej do rejestru R0.
  - Skopiuj wartość mnożnej do rejestru R1 jako początkowy wynik.
  - Dodaj wartość mnożnej do R1 dwukrotnie, każdorazowo używając ADDS, aby zaktualizować CPSR.
- Porównaj wynik w R1 z wartością oczekiwaną (np. 21) przy pomocy CMP.
- W zależności od wyniku, wykonaj warunkowy skok do sekcji sukcesu lub błędu.
- Zakończ program.

---