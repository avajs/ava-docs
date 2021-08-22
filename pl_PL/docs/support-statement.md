# Obsługiwane wersje Node.js

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/support-statement.md)

AVA obsługuje najnowszą wersję dowolnej głównej wersji, która [jest obsługiwana przez sam Node.js](https://github.com/nodejs/Release#release-schedule).

*Obsługiwanie* oznacza tutaj, że uruchamiamy nasz pakiet testowy w podanych wersjach Node.js i przyjmujemy pull request, aby naprawić wszelkie błędy (pod warunkiem, że nie są to znane błędy w samym Node.js, które zostaną natychmiast naprawione). w konsekwencji, *porzucenie wsparcia* oznacza, że usuniemy te wersje Node.js z naszej matrycy testowej i nie będziemy już akceptować określonych pull requestów w celu naprawy błędów w tych wersjach.

Kiedy rezygnujemy z obsługi wersji głównej objętej LTS, podbijamy główny numer wersji AVA.

Zrezygnujemy z obsługi wersji Node.js o nieparzystych numerach (np. `11` lub `13`) *bez* podbijania głównego numeru wersji AVA.

Staramy się unikać *przypadkowego* porzucania obsługi nieaktualnych wersji Node.js. Jeśli takie uszkodzenie wystąpi, zaakceptujemy pull request w celu przywrócenia funkcjonalności. Możemy zdecydować się na wycofanie szkodliwej wersji AVA i zamiast tego podbić główny numer wersji AVA.

Za każdym razem, gdy podbijamy główny numer wersji AVA, *robimy* jawne porzucenie obsługi najnowszych wersji Node.js. To gwarantuje, że możemy polegać na cofniętych interfejsach API lub dostępności nowszych wersji V8 w późniejszych wersjach Node.js, w samym AVA lub w jednej z naszych zależności.

Możemy zrezygnować z obsługi wersji Node.js w wersji przedpremierowej wersji głównej, jeśli oczekuje się, że nowa wersja AVA ustabilizuje się w okolicach daty zakończenia wersji Node.js lub później pytanie.

Eksperymentalne funkcje włączone w konfiguracji `nonSemVerExperiments` mogą zostać zmienione lub usunięte w dowolnym momencie.
