# WYTYCZNE PROJEKTU

### Wymgania wstępne
Projekt będzie realizowany w grupach 2-3 osobowych. Projekt może poruszać tą samą tematykę co projekt z przedmiotu `Projektowanie i programowanie systemów internetowych I` u Pana **Rewaka**.

----


## **1. Cel projektu**
Celem projektu jest **zaprojektowanie, implementacja i prezentacja systemu bazodanowego** zgodnie z nowoczesnymi praktykami inżynierii oprogramowania. System powinien być **modularny, łatwy w utrzymaniu i uruchamialny u każdego**.

---

## **2. Zakres projektu**
Projekt obejmuje **wszystkie etapy cyklu życia systemu bazodanowego**, z naciskiem na iteracyjny rozwój modelu danych:

1. **Wybór środowiska SZBD oraz konfiguracja środowiska deweloperskiego**
    - podjęcie decyzji o wyborze PostgreSQL / MySQL,
    - przygotowanie infrastruktury z wykorzystaniem Dockera,
    - weryfikacja poprawności działania baz danych i połączenia z narzędziami deweloperskimi.

2. **Analiza wymagań i wstępny model danych**
    - określenie podstawowej funkcjonalności systemu,
    - zdefiniowanie kluczowych encji i ich relacji w kontekście wymagań biznesowych,
    - wstępna analiza potencjalnych operacji na bazie (przepływ danych, typy relacji),
    - przygotowanie schematu ERD, **który będzie ewoluował w trakcie projektu**.

3. **Iteracyjne projektowanie i modyfikacja bazy danych**
    - **migracje jako mechanizm zmian** – każda iteracja przynosi nowe zmiany w strukturze bazy,
    - wprowadzanie kolejnych encji i relacji w miarę potrzeb rozwoju aplikacji,
    - normalizacja danych,
    - optymalizacja schematu w kontekście wydajności zapytań.

4. **Implementacja migracji i seedów**
    - tworzenie i modyfikacja migracji w systemie (Laravel, Doctrine lub inne),
    - generowanie **danych testowych za pomocą seederów**,
    - wersjonowanie zmian w bazie i możliwość rollbacku (migracje w górę i w dół).

5. **Implementacja interfejsu użytkownika**
    - GUI umożliwiające interakcję z danymi,
    - tworzenie operacji CRUD dla głównych encji.

6. **Implementacja więzów integralności**
    - definiowanie kluczy obcych i **mechanizmów usuwania kaskadowego**,
    - tworzenie indeksów i ograniczeń unikalności.

7. **Testowanie i optymalizacja**
    - testowanie poprawności operacji CRUD.
    - analiza wydajności zapytań i dodawanie indeksów tam, gdzie to konieczne,

8. **Sporządzenie dokumentacji technicznej**
    - opis struktury bazy, głównych funkcji systemu i sposobu działania Dockera,
    - instrukcja uruchomienia projektu i jego wersjonowania.

---

## **3. Wymagania techniczne**
### **3.1 Środowisko uruchomieniowe**
- **Baza danych:** PostgreSQL lub MySQL (do wyboru),
- **Framework/Język:** kwestia preferencji (Laravel, Symfony, Django, inne),
- **Docker:** Projekt powinien być **uruchamialny w kontenerze**.

### **3.2 Struktura projektu**
- **Kod źródłowy** powinien być wersjonowany w Git.
- **Pliki migracji** powinny definiować wszystkie zmiany w bazie danych.
- **Seedery** powinny umożliwiać szybkie wypełnienie bazy testowymi danymi.
- **Instrukcja uruchomieniowa** powinna być dołączona w pliku `README.md`.

---

## **4. Kryteria oceny**
| **Kryterium**                                                    |
|------------------------------------------------------------------|
| Model ERD                                                        |
| Implementacja bazy danych (migracje)                             |
| Implementacja więzów integralności (klucze obce, cascade delete) |
| Poprawność działania interfejsu graficznego                      | 
| Użycie Dockera (powtarzalne uruchomienie)                        |
| Seedery i migracje poprawnie działające                          | 
| Testowanie i optymalizacja                                       |
| Dokumentacja techniczna                                          | 

---

## **5. Terminy**
1. **Zgłoszenie tematu projektu:** do 4 zajęć w semestrze (08.03.2025)
2. **Konsultacje z prowadzącym:** między 5 a 14 zajęciami w semestrze
3. **Oddanie projektu:** maksymalnie na ostatnich zajęciach w semestrze (07.06.2025)

---

## **6. Przykładowe tematy projektów**
1. **System zarządzania biblioteką**
    - użytkownicy, książki, wypożyczenia, rezerwacje, oceny,
    - możliwość dodawania nowych książek, wypożyczeń, rezerwacji,
    - wyszukiwanie książek, użytkowników, statystyki wypożyczeń.

2. **System zarządzania zadaniami**
    - użytkownicy, zadania, projekty, komentarze, terminy,
    - możliwość dodawania nowych zadań, projektów, komentarzy,
    - przypisywanie zadań do użytkowników, terminy wykonania.

3. **Forum internetowe**
    - użytkownicy, posty, komentarze, oceny, tagi,
    - możliwość dodawania nowych postów, komentarzy, tagów,
    - wyszukiwanie postów, użytkowników, statystyki ocen.
