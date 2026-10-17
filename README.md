# RomejroTV — przenosiny 5v.pl -> GitHub

Pakiet zawiera statyczną wersję strony pobierania ze starego `index.php`, przygotowaną dla GitHub Pages. Zawartości oryginalnych JSON-ów nie było w przesłanych plikach, dlatego **nie zostały wymyślone**. Dodaj własne pliki JSON w miejscach oznaczonych instrukcjami.

## 1. Utwórz repozytorium

Na https://github.com/new utwórz publiczne repozytorium `romejrotv-updates`. Publiczne jest potrzebne, gdy używasz darmowego konta GitHub Pages. Umieść pliki tego pakietu w katalogu głównym repozytorium, a do `assets` dołącz swoje `logo.png`.

## 2. Umieść APK w Releases, nie w repozytorium

Otwórz repo -> Releases -> Draft a new release.

Możesz utworzyć **osobne wydania dla TV i Phone**, np. `tv-v1.2.3` i `phone-v1.2.3` (przykładowe tagi; dostosuj do swoich wersji) i do każdego wgrać odpowiedni APK:

- `app-tv-release.apk`
- `app-phone-release.apk`

Dokładny link do pliku będzie wyglądał tak:

- `https://github.com/TWOJ_LOGIN/romejrotv-updates/releases/download/TAG_TV/app-tv-release.apk`
- `https://github.com/TWOJ_LOGIN/romejrotv-updates/releases/download/TAG_PHONE/app-phone-release.apk`

Nie używaj `/releases/latest/download/` do niezależnych aktualizacji TV/Phone w jednym repozytorium: `latest` oznacza jedno wydanie i może nie zawierać APK obu platform. Używaj tagów konkretnych wydań w JSON.

## 3. Zmień linki w ORYGINALNYCH JSON-ach

Skopiuj istniejące `update.json` (TV i Phone) z poprzedniego serwera do folderów `tv/` i `phone/`. Zachowaj wszystkie klucze, wersje i format danych, którego oczekuje aplikacja Android. Zmień tylko wartość URL pobierania APK na link z GitHub Releases, oraz ewentualne URL-e metadanych, jeżeli są faktycznie używane.

**Uwaga:** jeśli pliki aktualizacji u Ciebie nazywają się `updates.json` (liczba mnoga), zachowaj tę nazwę i dostosuj odwołania w `index.html` (jest tam `update.json`) oraz w aplikacji do faktycznego nazewnictwa.

## 4. Włącz GitHub Pages

Repo -> Settings -> Pages -> Build and deployment: `Deploy from a branch`, Branch `main`, Folder `/(root)`, Save.

Powstałe adresy (wstaw swój rzeczywisty login) to:

- Strona: `https://TWOJ_LOGIN.github.io/romejrotv-updates/`
- TV JSON: `https://TWOJ_LOGIN.github.io/romejrotv-updates/tv/update.json`
- Phone JSON: `https://TWOJ_LOGIN.github.io/romejrotv-updates/phone/update.json`

GitHub Pages nie wykonuje PHP, dlatego przygotowana strona jest statyczna i pobiera linki do APK z plików JSON. Mechanizm odczytu strony rozpoznaje najczęstsze nazwy pól URL i wymaga adresu HTTPS zakończonego na `.apk` (może też zawierać query string).

## 5. Zmień endpoint aktualizacji w Android TV i Phone

Jeżeli APK mają w kodzie stary URL, zmień adres zapytania OTA z:

`https://romejrotv.5v.pl/updates/tv/update.json`

na:

`https://TWOJ_LOGIN.github.io/romejrotv-updates/tv/update.json`

Analogicznie dla Phone. Jeśli aplikacja używa nazwy `updates.json`, zachowaj ją. Nie zmieniaj mechanizmu porównywania `versionCode` ani schematu JSON bez sprawdzenia kodu Android.

## 6. Nie odcinaj aktualnych instalacji aplikacji

Stare, już zainstalowane wersje będą nadal sprawdzać URL na 5v.pl, jeśli został zakodowany w APK. Do czasu przejścia użytkowników na nową wersję utrzymaj na 5v.pl małe `update.json` z nowymi linkami do APK na GitHub (lub przekierowanie do nowego JSON, jeśli klient Android obsługuje przekierowania). Nie musisz tam już trzymać dużych APK. Dopiero nowa wersja aplikacji powinna odpytywać GitHub Pages.

## 7. Sprawdź przed wyłączeniem starego hostingu

Sprawdź osobno pliki JSON, całkowity rozmiar obu APK, instalację apk i ich podpis oraz aktualizację na TV i Phone. Upewnij się, że pobrane APK mają te same pakiety/podpisy co poprzednie wersje, a `versionCode` wzrósł, gdy rzeczywiście publikujesz nowszą wersję.

Repo publiczne ujawnia wszystkie dane znajdujące się w repozytorium: nie dodawaj prywatnych kluczy, tokenów, plików `.keystore` ani poufnych ustawień.
