## Zadania laboratoryjne - lista nr 5
### Zadanie nr. 1
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

### Zadanie nr. 2
Napisz rekurencyjną wersję algorytmu który oblicza n-ty wyraz ciągu Fibonacciego, dokonaj analizy złożoności czasowej i pamięciowej i porównaj z wersją nierekurencyjną.

### Zadanie nr. 3
Napisz rekurencyjną wersję algorytmu sortowania bąbelkowego, dokonaj analizy złożoności czasowej i pamięciowej i porównaj z wersją nierekurencyjną.

### Zadanie nr. 4
Napisz rekurencyjną wersję algorytmu sortowania przez scalanie, dokonaj analizy złożoności czasowej i pamięciowej i porównaj z wersją nierekurencyjną.
