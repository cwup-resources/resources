# Zadania laboratoryjne - lista nr 3

## Zadanie 1 – Skok bezwarunkowy

**Cel:** Zapoznać się z działaniem instrukcji `B` do pomijania wybranej instrukcji.

**Polecenie:**
- Ustaw w rejestrze R0 początkową wartość (np. 5).
- Następnie użyj instrukcji `B` do pominięcia instrukcji, która zmienia wartość R0 (np. `MOV R0, #10`).
- Po etykiecie docelowej skoku wykonaj inną operację (np. ustawienie R1 na 1) w celu potwierdzenia, że kod pominięty przez `B` nie został wykonany.
- Uruchom program krokowo i zweryfikuj, że wartość w R0 pozostaje bez zmian.

---

## Zadanie 2 – Porównanie i sygnalizacja równości

**Cel:** Przećwiczyć porównanie dwóch liczb oraz zastosowanie skoków warunkowych.

**Polecenie:**
- Załaduj dwie liczby do rejestrów R0 i R1 (np. R0 = 5, R1 = 5 lub R0 = 3, R1 = 7).
- Wykonaj porównanie tych liczb za pomocą instrukcji `CMP`.
- Użyj instrukcji warunkowej (np. `BEQ` lub `BNE`), aby ustawić wartość 1 w R2, jeśli liczby są równe, lub 0, jeśli są różne.
- Zakończ program.

---

## Zadanie 3 – Wartość bezwzględna liczby

**Cel:** Obliczyć wartość bezwzględną liczby znajdującej się w rejestrze.

**Polecenie:**
- Załaduj liczbę całkowitą do rejestru R0 (np. R0 = 10, R0 = -5, R0 = 0).
- Użyj instrukcji `CMP` do porównania R0 z zerem.
- Wykorzystaj skok warunkowy (np. `BGE` lub `BLT`) do sprawdzenia, czy liczba jest ujemna.
  - Jeśli R0 jest ujemne, wykonaj operację: R0 := 0 - R0 (uzyskasz jej przeciwnik, czyli wartość bezwzględną).
  - Jeśli R0 nie jest ujemne, pozostaw wartość bez zmian.
- Zakończ program.

---

## Zadanie 4 – Maksimum z trzech liczb

**Cel:** Wyznaczyć największą z trzech liczb zapisanych w rejestrach.

**Polecenie:**
- Załaduj trzy liczby do rejestrów R0, R1 i R2.
- Porównaj R0 i R1 używając `CMP` i odpowiednich skoków warunkowych, aby ustalić większą wartość.
- Następnie porównaj uzyskany wynik z wartością w R2, aby wyznaczyć największą liczbę.
- Umieść wynik (największą liczbę) w rejestrze R3.
- Zakończ program.

---

## Zadanie 5 – Podprogram max2

**Cel:** Zastosować wywołanie podprogramu do porównania dwóch liczb.

**Polecenie:**
- Napisz podprogram `max2`, który przyjmuje dwie liczby w rejestrach R0 i R1 i zwraca w R0 większą z nich.
  - Wewnątrz `max2` użyj instrukcji `CMP` oraz odpowiednich skoków warunkowych, aby porównać wartości.
  - Do powrotu z podprogramu użyj instrukcji `BX LR`.
- W programie głównym ustaw przykładowe wartości, np. R0 = 8 i R1 = 12.
- Wywołaj podprogram `max2` za pomocą `BL max2`.
- Po powrocie sprawdź, czy w R0 znajduje się większa liczba.
- Testuj działanie programu dla różnych kombinacji wartości (np. przypadek, gdy pierwsza liczba jest większa, oraz gdy obie są równe).
- Zakończ program.

---

# Pytania sprawdzające zrozumienie materiału

1. **Dlaczego w asemblerze konieczne jest stosowanie instrukcji sterujących przebiegiem programu?**  
   Co by się stało, gdyby program nie miał żadnych skoków ani rozgałęzień – jakie ograniczenia by to powodowało? Podaj przykłady sytuacji, w których musimy zmienić domyślny sekwencyjny przepływ wykonania instrukcji.

2. **W jaki sposób w asemblerze ARM realizuje się konstrukcje warunkowe typu if/else?**  
   Wymień, jakie instrukcje są do tego potrzebne i krótko opisz, jak za ich pomocą można zrealizować warunek: "jeżeli warunek to wykonaj X, w przeciwnym razie wykonaj Y". (W odpowiedzi wspomnij o roli instrukcji CMP oraz skoków warunkowych).

3. **Czym różni się instrukcja B od BL?**  
   Jakie dodatkowe działanie wykonuje BL w porównaniu do B i do czego to służy? Podaj typowe zastosowanie każdej z tych instrukcji w programie.

4. **W jakim rejestrze procesor ARM przechowuje adres powrotu po wywołaniu podprogramu instrukcją BL?**  
   Jak program może wykorzystać ten rejestr, aby wrócić do miejsca wywołania procedury? (Opisz mechanizm powrotu z użyciem instrukcji BX).

5. **Do czego służy instrukcja BX?**  
   Wyjaśnij jej typowe zastosowanie w programie oraz pochodzenie nazwy "Branch eXchange". Dlaczego do powrotu z funkcji stosujemy BX LR zamiast zwykłego B?

6. **Co robi instrukcja CMP R1, R2?**  
   Czy wpływa ona na zawartość samych rejestrów R1 i R2? Jaki będzie stan flag (N, Z, C, V) po jej wykonaniu dla różnych relacji między wartościami w R1 i R2 (np. gdy R1 == R2, R1 < R2, R1 > R2)?

7. **Kiedy zostanie wykonana instrukcja BEQ, a kiedy BNE?**  
   Jakie warunki (stany flag) muszą być spełnione, aby skok doszedł do skutku? Co musi wystąpić wcześniej w programie, żeby te flagi zostały ustawione w odpowiedni sposób?

8. **Wyjaśnij znaczenie poszczególnych flag NZCV w rejestrze CPSR.**  
   Za co odpowiada flaga N? Co sygnalizuje flaga Z? Kiedy ustawiana jest flaga C, a kiedy flaga V? Która instrukcja omawiana na tych zajęciach służy do ustawienia tych flag przed wykonaniem skoku warunkowego i w jaki sposób skoki warunkowe korzystają z tych flag przy podejmowaniu decyzji o zmianie przebiegu programu?
