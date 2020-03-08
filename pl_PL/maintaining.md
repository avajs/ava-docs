# Utrzymanie

## Postępowanie

**Bądź miły dla wszystkich.** Przeczytaj i przestrzegaj [Kodeksu postępowania](.github/CODE_OF_CONDUCT.md).

## Testowanie

* `npm test`: Podpowie kod i uruchomi cały pakiet testowy z pokryciem.
* `npx tap --no-cov test/fork.js --bail`: Uruchomi konkretny plik testowy i bail przy pierwszym niepowodzeniu (przydatne podczas wyszukiwania błędów).

## CI

* Testy czasami kończą się niepowodzeniem w systemie Windows. Dokładnie sprawdź błędy.
* Co najmniej jedno zadanie systemu Windows musi przejść.
* Wszystkie inne zadania muszą przejść.

## Aktualizowanie zależności

* Upewnij się, że nowe wersje zależności są zgodne z naszymi obsługiwanymi wersjami Node.js.
* Pozostaw zależność TypeScript bez zmian, aby uniknąć przypadkowego uszkodzenia.
* Otwórz PR z aktualizacjami i merguj, gdy CI przejdzie (patrz poprzedni rozdział).

## Aktualizowanie TypeScript

TypeScript sam nie obserwuje SemVer. W związku z tym może być konieczne wprowadzenie zmian w definicji typu, które z technicznego punktu widzenia niszczą zmiany dla użytkowników starszych wersji TypeScript. W porządku, ale powinniśmy być tego świadomi.

Aktualizuj zależność TypeScript tylko wtedy, gdy jest to naprawdę konieczne. Pomaga to uniknąć przypadkowego uszkodzenia. Na przykład nie będziemy przypadkowo polegać na nowszych funkcjach TypeScript.

Mówiąc o, używanie nowszych funkcji TypeScript można uznać za przełomową zmianę. Należy to oceniać indywidualnie dla każdego przypadku.

## Pull requests

* Nowe featury powinny pochodzić z testów i dokumentacji.
* Upewnij się, że [wytyczne współtworzenia](.github/CONTRIBUTING.md) są odhaczone.
* Squash commits gdy mergowanie.

## Eksperymenty

* Najpierw zaimplementuj przełomowe zmiany, wymagające zgody.
* Wcześniej dostarczaj nowe funkcje, traktując je jak eksperyment, wymagające zgody.

## Proces wydania

* Zaktualizuj zależności (patrz poprzednia sekcja).
* Jeśli [konieczny](docs/support-statement.md), zaktualizuj pole `engines` w `package.json`.
	* Usuń nieobsługiwane (lub wkrótce) wersje Node.js.
	* Wykonując główną wersję bump, upewnij się, że potrzebujesz najnowszej wersji każdej obsługiwanej wersji Node.js.
* Opublikuj nową wersję za pomocą [`np`](https://github.com/sindresorhus/np) z numerem wersji zgodnie z [SemVer](http://semver.org).
* Napisz [notatkę o wydaniu](https://github.com/avajs/ava/releases/new) zgodnie ze stylem poprzednich uwag do wydania.
