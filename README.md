# FIELD SCANNER — strona na GitHub Pages

Gotowa aplikacja po polsku, bez instalowania npm ani kompilowania. Hasło interfejsu: **2883**.

## Uruchomienie na GitHub

1. Rozpakuj ZIP na komputerze.
2. Zaloguj się na https://github.com i wybierz **New repository**.
3. Nazwa: `FIELD-SCANNER`. Wybierz **Public**, zaznacz utworzenie README i kliknij **Create repository**.
4. W repozytorium wybierz **Add file → Upload files**.
5. Wgraj **zawartość** rozpakowanego folderu FIELD-SCANNER: index.html, style.css, folder js, folder examples, README.md i .nojekyll. Nie wgrywaj samego ZIP-a ani nadrzędnego folderu. index.html musi być widoczny bezpośrednio w głównym katalogu repozytorium. Ukryty .nojekyll nie jest niezbędny do działania tej prostej strony.
6. Kliknij **Commit changes**.
7. Otwórz **Settings → Pages**.
8. W **Build and deployment → Source** wybierz **Deploy from a branch**.
9. Wybierz gałąź **main**, folder **/(root)** i kliknij **Save**.
10. Po zakończeniu publikacji w Settings → Pages pojawi się adres strony i przycisk Visit site. Typowy adres: https://TWOJ-LOGIN.github.io/FIELD-SCANNER/.
11. Otwórz adres, wpisz 2013 i wybierz **Zobacz demo**, aby obejrzeć panel.

Oficjalna instrukcja: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

## Import pomiaru

W zakładce Import wybierz jednocześnie data.csv, gps.csv i config.json z jednego folderu POMIAR_X. W Windows zaznacz pliki z Ctrl; na telefonie użyj wyboru wielu plików. Przykład jest w examples/POMIAR_1. Nie musisz przesyłać pomiarów do repozytorium GitHub.

Każdy folder to sesja POMIAR_1, POMIAR_2 itd. Kolumna measurement w CSV to numer **próbki wewnątrz sesji**. Łączymy oba CSV po tej kolumnie, nie po kolejności wierszy. Numery próbek są unikalne w każdym pliku. Próbka bez pozycji nie będzie punktem na mapie. Pustą wartość zapisuj jako puste pole, nie jako zero. CSV używa przecinka jako separatora i kropki dziesiętnej. Kodowanie UTF-8, obsługiwany BOM i cytowane pola.

Minimalne data.csv dla METEO:

```csv
measurement,temperature,humidity,pressure
1,18.4,62,1014
```

Minimalne data.csv dla RADIO:

```csv
measurement,frequency,signal,quality
1,99.8,91,
```

gps.csv:

```csv
measurement,latitude,longitude,altitude
1,49.5600,22.2000,320
```

config.json:

```json
{
  "schema_version": "1.0",
  "measurement": "POMIAR_2",
  "station": "FIELD SCANNER",
  "head": "METEO",
  "description": "Pomiar terenowy"
}
```

Jednostki: temperatura °C, wilgotność %, ciśnienie hPa, wysokość m, wiatr m/s, direction w stopniach. Częstotliwość domyślnie MHz; inna jednostka wymaga frequency_unit w konfiguracji. signal_unit i quality_unit należy ustawić zgodnie z odbiornikiem (np. dBµV, dB, %). Program nie przelicza RSSI na procenty. Bez deklaracji pokazuje „jedn. odbiornika”. Wybór najlepszej częstotliwości korzysta z maksimum signal; jakość jest osobnym polem. Dane różnych odbiorników i jednostek wymagają kalibracji do porównań. started_at może zawierać datę/czas ISO 8601, ale nazwa sesji nadal musi być POMIAR_X. Wersja 1 nie zmienia kolejności próbek; wykresy pokazują wartości względem próbek.

## Mapa, raporty i historia

Mapa: wybierz warstwę, kliknij punkt. Dostępne są sygnał RADIO, temperatura, wilgotność, ciśnienie, wysokość, wiatr (gdy dostarczono dane) i GPS. Kolory są skalowane do zakresu aktywnego pomiaru; nie stanowią uniwersalnych progów. Kierunek anteny jest pokazywany w dymku, jeśli jest zapisany; sektory, strzałki i heatmapy są rozszerzeniami przyszłymi.

Raporty: wpisz uwagi, kliknij Drukuj / zapisz PDF, wybierz w przeglądarce Zapisz jako PDF. Poczekaj na załadowanie mapy przed drukowaniem; w razie potrzeby włącz grafikę tła. PDF tworzy przeglądarka.

Historia: dane przechowywane w localStorage tej przeglądarki. Nie synchronizują się między telefonem i komputerem. Eksport kopii zapisuje JSON; Wczytaj kopię przenosi historię. Import tego samego POMIAR_X proponuje zastąpienie. Nie importuj dwóch różnych sesji o tym samym numerze bez zmiany identyfikatora. Dane demo nie zapisują się w historii. Limit importu 20 MB; pojemność localStorage zwykle jest znacznie mniejsza, więc bardzo duże pomiary mogą nie zmieścić się — aplikacja zgłosi błąd, zachowując wcześniejsze dane.

## Bezpieczeństwo i internet

Hasło 2013 jest blokadą interfejsu, widoczną w kodzie JavaScript. Nie chroni publicznego repozytorium ani nie stanowi prawdziwego logowania. Nie wgrywaj prywatnych pomiarów do repozytorium. Import nie wysyła treści CSV na serwer. Leaflet jest ładowany z unpkg, a mapa z OpenStreetMap; mapa i pierwsze pobranie biblioteki wymagają internetu. Pobieranie podkładu ujawnia dostawcy przybliżony oglądany obszar. Wykresy są tworzone lokalnie bez dodatkowej biblioteki. Offline dostępne są analizy i współrzędne, jeśli strona została wcześniej pobrana lub jest uruchomiona lokalnie.

## Pliki i aktualizacje

- index.html — ekran logowania i szkielet panelu.
- style.css — styl, telefon i druk.
- js/data.js — parser i walidacja CSV/JSON, dane demonstracyjne.
- js/charts.js — wykresy SVG.
- js/app.js — nawigacja, historia, import, mapa, analizy i raporty.
- examples — trzy przykładowe pliki.
- .nojekyll — wyłącza przetwarzanie Jekyll.


Lokalny podgląd: otwórz index.html w przeglądarce. Dla najbardziej przewidywalnego zachowania uruchom z katalogu projektu `python -m http.server 8000` i wejdź na http://localhost:8000.
