## Budowa algorytmów oraz programów rekurencyjnych

### Wprowadzenie do rekurencji

#### Definicja 

Rekurencja to technika programowania, w której funkcja wywołuje samą siebie jako część swojego wykonania. Każde wywołanie funkcji ma swoje własne zmienne i przestrzeń roboczą, co pozwala na niezależne wykonanie każdego wywołania.

#### Porównanie rekurencji i iteracji

Rekurencja i iteracja to dwie różne metody osiągania powtarzalności w programowaniu. Iteracja polega na użyciu struktur sterujących, takich jak pętle, do wielokrotnego wykonania bloku kodu. Rekurencja natomiast polega na wywoływaniu funkcji przez samą siebie.

Chociaż obie metody mogą być używane do rozwiązania tych samych problemów, różnią się pod względem efektywności i zrozumiałości. Rekurencja może być bardziej elegancka i zrozumiała dla niektórych problemów, ale może również prowadzić do większego zużycia pamięci i zasobów procesora.

#### Przykłady funkcji rekurencyjnych: silnia, ciąg Fibonacciego

###### Silnia

Silnia liczby n (oznaczana jako n!) to iloczyn wszystkich liczb całkowitych od 1 do n. Funkcja rekurencyjna do obliczania silni wyglądałaby tak:

```python
def silnia(n):
    if n == 0:
        return 1
    else:
        return n * silnia(n-1)
```

###### Ciąg Fibonacciego

Ciąg Fibonacciego to ciąg liczb, w którym każda liczba jest sumą dwóch poprzednich. Funkcja rekurencyjna do generowania ciągu Fibonacciego wyglądałaby tak:

```python
def fibonacci(n):
    if n <= 1:
       return n
    else:
       return (fibonacci(n-1) + fibonacci(n-2))
```

### Budowa Programów Rekurencyjnych

#### Warunek zakończenia rekurencji

Każda funkcja rekurencyjna musi mieć warunek zakończenia, który zapobiega nieskończonemu wywoływaniu samej siebie. Bez warunku zakończenia, funkcja rekurencyjna kontynuowałaby wywoływanie samej siebie nieskończenie, co prowadziłoby do przekroczenia limitu stosu.

#### Zagrożenia związane z rekurencją: przekroczenie limitu stosu, złożoność obliczeniowa

Rekurencja, chociaż potężna, niesie ze sobą pewne ryzyko. Główne zagrożenia związane z rekurencją to:

- *Przekroczenie limitu stosu:* Każde wywołanie funkcji, w tym wywołania rekurencyjne, zużywa pewną ilość pamięci stosu. Jeśli funkcja rekurencyjna wywołuje samą siebie zbyt wiele razy bez osiągnięcia warunku zakończenia, może dojść do przekroczenia limitu stosu, co prowadzi do błędu.
- *Złożoność obliczeniowa:* Niektóre funkcje rekurencyjne mogą mieć wysoką złożoność obliczeniową, co oznacza, że mogą być wolne lub nieefektywne przy dużych danych wejściowych.

#### Techniki optymalizacji rekurencji: memoizacja, rekurencja ogonowa

Istnieją techniki, które mogą pomóc zminimalizować niektóre z zagrożeń związanych z rekurencją:

- *Memoizacja:* Jest to technika, która polega na zapisywaniu wyników funkcji w pamięci, aby uniknąć niepotrzebnego powtarzania tych samych obliczeń. Jest szczególnie przydatna w funkcjach rekurencyjnych, które wielokrotnie wywołują samą siebie z tymi samymi argumentami.
- *Rekurencja ogonowa:* Jest to forma rekurencji, w której wywołanie rekurencyjne jest ostatnią operacją w funkcji. Niektóre języki programowania (takie jak Scheme i Scala) mogą optymalizować rekurencję ogonową, przekształcając ją w iterację i eliminując ryzyko przekroczenia limitu stosu. W Pythonie jednak rekurencja ogonowa nie jest optymalizowana i może prowadzić do przekroczenia limitu stosu.

### Zadania do wykonania 

#### Zadanie nr.1 

Napisz rekurencyjną wersję algorytmu binSearch, dokonaj analizy złożoności czasowej i pamięciowej i porównaj z wersją nierekurencyjną.

```python 
def binSearch(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid  # target found
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1  # target is missing


arr = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21]
target = 15

index = binSearch(arr, target)
if index != -1:
    print(f"Element {target} znaleziony na pozycji {index}")
else:
    print(f"Element {target} nie znaleziony w liście")
```

W tym kodzie, arr to posortowana lista, a target to element, którego szukamy. Funkcja zwraca indeks elementu target w liście arr jeśli jest obecny, w przeciwnym razie zwraca -1. Algorytm działa poprzez ciągłe dzielenie listy na dwie połówki, aż znajdzie szukany element lub do momentu, gdy lista zostanie całkowicie przeszukana.
