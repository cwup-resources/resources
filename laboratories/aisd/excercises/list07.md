## Zadania laboratoryjne - lista nr 7

### **Zadanie 1: Współliniowość trzech punktów**

Napisz program, który sprawdza, czy trzy podane punkty $A, B, C$ w przestrzeni 2D leżą na jednej prostej.

#### Wejście:

* Trzy punkty w formacie: $A(x_1, y_1), B(x_2, y_2), C(x_3, y_3)$

#### Wyjście:

* `Tak` – jeśli są współliniowe
* `Nie` – w przeciwnym wypadku

#### Wskazówka:

Użyj wzoru na pole trójkąta lub iloczynu wektorowego.

---

### **Zadanie 2: Otoczka wypukła (Convex Hull)**
Dla danego zbioru punktów w przestrzeni 2D, napisz program wyznaczający otoczkę wypukłą i wypisujący punkty w porządku przeciwnym do ruchu wskazówek zegara.

#### Wejście:

* Lista punktów w postaci: `[(x1, y1), (x2, y2), ..., (xn, yn)]`

#### Wyjście:

* Lista punktów tworzących otoczkę wypukłą

#### Wskazówka:

* Zaimplementuj algorytm Andrew's Monotone Chain lub Graham Scan.
* https://www.geeksforgeeks.org/convex-hull-in-python/

---

### **Zadanie 3: BFS w grafie**
Zaimplementuj algorytm przeszukiwania wszerz (BFS) w nieskierowanym grafie reprezentowanym jako lista sąsiedztwa.

#### Wejście:

* Lista sąsiedztwa (słownik Python)
* Wierzchołek początkowy

#### Wyjście:

* Kolejność odwiedzania wierzchołków

#### Przykład grafu:

```python
graph = {
    'A': ['B', 'C'],
    'B': ['D'],
    'C': ['E'],
    'D': [],
    'E': ['F'],
    'F': []
}
```
#### Wskazówka:
* https://www.geeksforgeeks.org/breadth-first-search-or-bfs-for-a-graph/
---

### **\*Zadanie 4: Prosty algorytm genetyczny (maksymalizacja funkcji)**

**Temat:** Algorytm genetyczny
**Poziom:** Średni

#### Treść:

Napisz prosty algorytm genetyczny, który maksymalizuje funkcję $f(x) = x^2$, gdzie $x \in [-10, 10]$ i jest liczbą całkowitą.

#### Wymagania:

* Populacja: np. 10 osobników
* Selekcja: wybierz najlepszych 2
* Krzyżowanie: średnia arytmetyczna rodziców
* Mutacja: dodaj lub odejmij 1 z pewnym prawdopodobieństwem
* Iteracje: np. 20 pokoleń

#### Wyjście:

* Najlepsze rozwiązanie po zakończeniu procesu
