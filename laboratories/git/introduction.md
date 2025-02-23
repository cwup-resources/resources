# System kontroli wersji Git

## Wprowadzenie

Aby rozpocząć pracę z Gitem, utwórz konto na platformie [GitHub](http://github.com/) używając adresu studenckiego. Po rejestracji zweryfikuj adres e-mail w ustawieniach (*Settings > Emails*). Jeśli posiadasz już konto, dodaj adres studencki jako dodatkowy i również go zweryfikuj. Prowadzący doda Cię do organizacji uczelnianej ([collegiumwitelona](https://github.com/collegiumwitelona)), co umożliwi realizację zajęć i oddawanie zadań. Sprawdź pocztę (w tym folder spam) i zaakceptuj zaproszenie do organizacji.

## Co to jest Git?

Git to rozproszony system kontroli wersji (DVCS), który umożliwia zarządzanie zmianami w plikach, głównie w kodzie źródłowym, ale także w dowolnych plikach tekstowych (np. dokumentach Markdown). Dzięki Gitowi możesz śledzić historię zmian, wracać do wcześniejszych wersji i współpracować z innymi osobami. W odróżnieniu od scentralizowanych systemów (np. SVN), w Git każdy użytkownik ma pełną kopię repozytorium na swoim komputerze. To zwiększa bezpieczeństwo (brak zależności od serwera) i umożliwia pracę offline. Git jest szczególnie popularny w projektach programistycznych dzięki swojej elastyczności i wydajności.

## Dlaczego warto używać Gita?

Git oferuje wiele zalet, które czynią go kluczowym narzędziem w pracy programistów:

- **Kontrola wersji**: Pozwala śledzić każdą zmianę w projekcie, co ułatwia znajdowanie błędów i powrót do działającej wersji.
- **Współpraca**: Umożliwia zespołową pracę nad kodem, np. dzięki *pull requestom* na GitHubie, które wspierają recenzję zmian.
- **System rozproszony**: Pełna kopia repozytorium na każdym komputerze zwiększa bezpieczeństwo i umożliwia pracę bez internetu.
- **Efektywność**: Git działa szybko, zwłaszcza przy operacjach lokalnych, takich jak commitowanie czy przeglądanie historii.
- **Branching i merging**: Możesz tworzyć gałęzie (*branches*), aby pracować nad różnymi funkcjami równolegle, a następnie łączyć je (*merge*) w całość.
- **Łatwość w obsłudze**: Choć nauka Gita wymaga czasu, jego interfejs wiersza poleceń jest potężny i elastyczny.

## Instalacja Gita

Aby używać Gita, zainstaluj go na swoim komputerze. Szczegółowe instrukcje znajdziesz w [oficjalnej dokumentacji](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git). Przykładowe polecenia:

- **Linux (Debian/Ubuntu)**:
```bash
  sudo apt install git
```
- **Linux (Fedora)**:
```bash
  sudo dnf install git
```
- **macOS**:
```bash
  brew install git
```

Po instalacji sprawdź wersję Gita w konsoli:
```bash
git --version
```

Jeśli Git jest zainstalowany, zobaczysz numer wersji (np. `git version 2.34.1`). Jeśli pojawi się błąd, oznacza to, że instalacja nie powiodła się – wróć do instrukcji.

## Konfiguracja Gita

Po instalacji skonfiguruj swoje dane, aby Git wiedział, kto wprowadza zmiany. Użyj poniższych poleceń:

```bash
git config --global user.name "Imię Nazwisko"
git config --global user.email "example@gmail.com"
```

Opcja `--global` ustawia konfigurację dla wszystkich repozytoriów na Twoim komputerze. Podany adres e-mail i nazwa pojawią się w historii commitów, co jest ważne przy współpracy i weryfikacji autorstwa.

## Inicjalizacja katalogu repozytorium

Utwórz katalog dla swojego projektu:

```bash
mkdir aisd-2025
cd aisd-2025
```

- `mkdir aisd-2025`: Tworzy katalog o nazwie np. `aisd-2025` (sugerowana nazwa związana z przedmiotem).
- `cd aisd-2025`: Przechodzi do nowo utworzonego katalogu. Możesz utworzyć katalog w dowolnej lokalizacji, np. w folderze domowym (`~/aisd-2025`).

## Inicjalizacja repozytorium

Będąc w katalogu projektu, zainicjuj repozytorium i połącz je ze zdalnym serwerem:

```bash
git init
git remote add origin https://github.com/collegiumwitelona/2025-aisd-twojanazwauzytkownika
echo "# nr indeksu / imię nazwisko / email" >> README.md
git add .
git commit -m "first commit"
git push -u origin main
```
Kolejne kroki oznaczają:

- `git init`: Inicjalizuje lokalne repozytorium Git.
- `git remote add origin <URL>`: Łączy lokalne repozytorium ze zdalnym na GitHubie.
- `echo "# ..." >> README.md`: Tworzy plik `README.md` z nagłówkiem (`#` w Markdownie) i Twoimi danymi.
- `git add .`: Dodaje wszystkie pliki do tzw. *staging area*.
- `git commit -m "first commit"`: Zapisuje zmiany z opisem.
- `git push -u origin main`: Wysyła zmiany do zdalnego repozytorium (gałąź `main`) i ustawia ją jako domyślną.

Uwagi:

- Jeśli zdalne repozytorium nie jest puste, wykonaj najpierw `git pull origin main`, aby pobrać istniejące zmiany.
- Domyślna gałąź na GitHubie to `main` (nie `master`).

## Wprowadzanie zmian do repozytorium

Aby wprowadzić zmiany do repozytorium:

```bash
git add .
git status
git commit -m "zmiany"
git push origin main
```

Kolejne kroki oznaczają:

- `git add .`: Dodaje wszystkie zmienione i nowe pliki do staging area.
- `git status`: Pokazuje, co zostanie zatwierdzone.
- `git commit -m "zmiany"`: Tworzy commit z opisem zmian.
- `git push origin main`: Wysyła zmiany na serwer.

## Tokeny dostępu Github

Przy `git push` GitHub poprosi o login i hasło. Zamiast hasła użyj tokena osobistego dla większego bezpieczeństwa. Aby go wygenerować:

1. Zaloguj się na GitHubie.
2. Przejdź do https://github.com/settings/tokens.
3. Wybierz Generate new token (classic), nadaj nazwę (np. "Git Access"), ustaw uprawnienia (minimum `repo`) i zapisz token w bezpiecznym miejscu.
4. Użyj tokena jako hasła podczas autoryzacji w Git.

## Zadanie do wykonania
W repozytorium proszę dodać:
* plik `readme.md` z podanym numerem indeksu, adresem mejlowym, oraz imieniem i nazwiskiem studenta;
* katalog o nazwie `lab01` z plikiem `lab01.md`; plik powinien zawierać listę komend potrzebnych do zainicjowania repozytorium, pobrania zmian z serwera oraz wgrania nowych zmian.