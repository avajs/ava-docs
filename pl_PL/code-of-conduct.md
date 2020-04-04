# Wspieranie AVA

✨ Dziękujemy za Twój wkład w AVA! ✨

Należy pamiętać, że ten projekt został opublikowany z [Kodeksem postępowania autora](code-of-conduct.md). Uczestnicząc w tym projekcie, zgadzasz się przestrzegać jego warunków.

Tłumaczenia: [English](https://github.com/avajs/ava/blob/master/contributing.md), [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/contributing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/contributing.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/contributing.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/contributing.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/contributing.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/contributing.md)

## Jak mogę pomóc ?

### Popraw dokumentację

Jako użytkownik AVA jesteś idealnym kandydatem, aby pomóc nam ulepszyć naszą dokumentację: poprawki literówek, poprawki błędów, lepsze wyjaśnienia, więcej przykładów itp. Otwarte problemy dotyczące rzeczy, które można poprawić. [Pomóż przetłumaczyć nasze dokumenty.](https://github.com/avajs/ava-docs) Cokolwiek. Nawet ulepszenia wprowadzone w tym dokumencie.

Użyj [etykiety `docs`](https://github.com/avajs/ava/labels/docs) aby znaleźć sugestie dotyczące tego, co chcielibyśmy zobaczyć więcej w dokumentacji.

### Popraw problemy

Niektóre problemy powstają z brakujących, niemożliwych do odtworzenia lub po prostu nieprawidłowych informacji. Pomóż nam ułatwić ich rozwiązywanie. Zarządzanie problemami zajmuje dużo czasu, z twoją pomocą możemy poświęcić więcej czasu na naprawianie błędów i dodawanie funkcji.

### Przekaż nam swoją opinię na temat problemów

Zawsze jesteśmy zainteresowani innymi opiniami w dyskusjach na tematy. To dobra okazja, aby wpłynąć na przyszły kierunek AVA.

[Etykieta `question`](https://github.com/avajs/ava/labels/question) jest właściwym miejscem do prowadzenia bieżących dyskusji.

### Napisz kod

Możesz użyć etykiet, aby dowiedzieć się, gdzie możesz pomóc:

* [`blocked`](https://github.com/avajs/ava/labels/blocked) gdzie potrzebujemy pomocy, aby odblokować sytuację
* [`bug`](https://github.com/avajs/ava/labels/bug) dla znanych błędów które chcielibyśmy naprawić
* [`enhancement`](https://github.com/avajs/ava/labels/enhancement) są funkcje do dodania
* [`performance`](https://github.com/avajs/ava/labels/performance) oferuje pomysły na poprawę wydajności AVA

Etykieta [`help wanted`](https://github.com/avajs/ava/labels/help%20wanted) (prośba o pomoc) i [`good for beginner`](https://github.com/avajs/ava/labels/good%20for%20beginner) (dobre dla początkujących) są szczególnie przydatne.

Możesz znaleźć problem, który jest przypisany lub ma [etykietę `assigned`](https://github.com/avajs/ava/labels/assigned). Sprawdź przed rozpoczęciem tego problemu, ponieważ prawdopodobnie ktoś inny nad nim pracuje.

Chcielibyśmy poprawić [issues `priority`](https://github.com/avajs/ava/labels/priority) (prioritaires) pierwsze. Chcielibyśmy również zobaczyć postęp [issues `low-priority`](https://github.com/avajs/ava/labels/low%20priority) (niski priorytet). [issues `future`](https://github.com/avajs/ava/labels/future) to te, które chcielibyśmy mieć, ale nie od razu. Sprawdź je przed przystąpieniem do pracy, ponieważ w tej chwili możemy nie brać odpowiedzialności za te funkcje.

Jeśli aktualizujesz zależności, upewnij się, że używasz npm@5.6.0 i zobowiązaj się do aktualizacji pliku `package-lock.json`.

### Zobacz na nasz czat

Mamy [chat](https://spectrum.chat/ava). Przyjdź, szpieguj, porozmawiaj z nami i pomóż innym.

## Zgłoś problem

- Issues dotyczą problemów. Skorzystaj z naszego [chat](https://spectrum.chat/ava) lub [Stack Overflow](https://stackoverflow.com/questions/tagged/ava) po wsparcie.
- Sprawdź błąd w istniejących wyjściach przed otwarciem jednego.
- Pamiętaj o użyciu najnowszej wersji AVA.
- Użyj jasnego i opisowego tytułu (w języku angielskim).
- odaj jak najwięcej informacji: kroki, aby odtworzyć problem, komunikat o błędzie, wersję Node.js, system operacyjny itp. (W języku angielskim).
- Im więcej czasu poświęcisz na napisanie swojego problemu, tym szybciej go rozwiążemy.
- [Najlepszym sposobem na odroczenie wyniku jest udowodnienie go nieudanym testem.](https://twitter.com/sindresorhus/status/579306280495357953)

## Prześlij pull request

- Nietrywialne zmiany należy najpierw omówić w kwestii: aby uniknąć wykonywania niepotrzebnej pracy.
- W przypadku ważnych zadań radzimy jak najszybciej przedstawić swoje prace społeczności w celu uzyskania ich opinii. Otwórz pull request, gdy tylko zrobisz minimum niezbędne, aby zademonstrować swój pomysł. Na tym wczesnym etapie nie martw się, jeśli rzeczy nie są idealne lub w 100% skończone. Dodaj przedrostek [WIP] w tytule i opisz, co jeszcze musisz zrobić. Dzięki temu osoby patrzące na to nie szukają drobiazgów ani nie zgłaszają ulepszeń, które już wiesz, że musisz wprowadzić.
- Nowym funkcjom muszą towarzyszyć testy i dokumentacja (w języku angielskim).
- Nie dodawaj zmian niezwiązanych z pull requestem.
- Sprawdź (Lint) i przetestuj przed wysłaniem pull requestem, uruchamiając `$ npm test`.
- Wyślij pull request [branche](https://github.com/dchelimsky/rspec/wiki/Topic-Branches), nie na mastera.
- Użyj jasnego i opisowego tytułu dla pull request i zatwierdzeń.
- Napisz atrakcyjny opis, aby wyjaśnić, dlaczego powinniśmy zintegrować Twój pull request. Twoim zadaniem jest nas przekonać. Odpowiedź na pytanie „dlaczego” jest konieczne i podaj przypadki użycia.
- Możemy poprosić Cię o zmianę pull request. Nie jest konieczne otwieranie kolejnego żądania ściągnięcia. [Po prostu zaktualizuj ten, który już istnieje.](https://github.com/RichardLitt/knowledge/blob/master/github/amending-a-commit-guide.md)

Uwaga: kiedy modyfikujesz kod, pamiętaj, że motto AVA (skradzione z Pythona) to jedna metoda zrobienia czegoś. Na przykład żądanie dodania aliasu do interfejsu API ([w ten sposób](https://github.com/avajs/ava/pull/663)) zostanie najprawdopodobniej odrzucone, jeśli nie zostaną dodane inne znaczące korzyści.

*Czy chcesz wnieść swój pierwszy wkład w projekt typu open source? Nie szukaj dalej! AVA może być jednym z najbardziej przyjaznych projektów i społeczności. Przeczytaj post na blogu ["Making your first contribution"](https://medium.com/@vadimdemedes/making-your-first-contribution-de6576ddb190) aby zacząć poprawnie i zrobić swój pierwszy udział w AVA !*
