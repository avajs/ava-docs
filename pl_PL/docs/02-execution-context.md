# Kontekst wykonania (`t` argument)

Tłumaczenia: [Français](https://github.com/avajs/ava-docs/blob/main/fr_FR/docs/02-execution-context.md)

Każdy test lub hook jest wywoływany z kontekstem wykonania. Zgodnie z konwencją jest nazywany `t`.

```js
const test = require('ava');

test('my passing test', t => {
	t.pass();
});
```

Każdy test lub hook otrzymuje inny obiekt. Zawiera [asercje](./03-assertions.md), a także metody i właściwości wymienione poniżej.

## `t.title`

Tytuł testu.

## `t.context`

Zawiera stan współdzielony z hooks.

## `t.plan(count)`

Zaplanuj, ile asercji jest w teście. Test zakończy się niepowodzeniem, jeśli faktyczna liczba asercji nie będzie zgodna z liczbą planowanych asercji. Zobacz [planowanie asercji](./03-assertions.md#assertion-planning).

## `t.end()`

Zakończ test. Działa tylko z `test.cb()`.

## `t.log(...values)`

Rejestruj wartości kontekstowo obok wyniku testu zamiast natychmiast je drukować do `stdout`. Zachowuje się trochę jak `console.log`, ale bez obsługi tokenów zastępczych.

## `t.timeout(ms)`

Ustaw limit czasu testu w milisekundach. Test zakończy się niepowodzeniem, jeśli ten limit czasu zostanie przekroczony. Limit czasu jest resetowany przy każdej asercji.
