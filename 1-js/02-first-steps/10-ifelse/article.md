# Bedingungsoperatoren: if, '?'

Manchmal müssen wir unterschiedliche Aktionen ausführen, abhängig von verschiedenen Bedingungen.

Um dies zu tun können wir die `if` Anweisung und den Bedingungsoperator `?` verwenden, der auch "Fragezeichen"-Operator genannt wird.

## Die "if" Anweisung

Die `if(...)` Anweisung wertet eine Bedingung in Klammern aus und führt, falls das Ergebis `true` ist, einen Code-Block aus.

Zum Beispiel:

```js run
let year = prompt('In which year was ECMAScript-2015 specification published?', '');

*!*
if (year == 2015) alert( 'You are right!' );
*/!*
```

In obigen Beispiel ist die Bedingung eine einfache Gleichheitsprüfung (`year == 2015`), aber sie kann auch weitaus komplexer sein.

Falls wir mehr als eine Anweisung ausführen wollen, müssen wir unseren Code-Block in geschweifte Klammern einschließen:

```js
if (year == 2015) {
  alert( "That's correct!" );
  alert( "You're so smart!" );
}
```

Wir empfehlen, deine Code-Blöcke jedes Mal in geschweifte Klammern `{}` einzuschließen wenn du eine `if` Anweisung verwendest, selbst wenn nur eine Anweisung ausgeführt wird. Dieses Vorgehen erhöht die Lesbarkeit.

## Boolean Umwandlung

Die `if (…)` Anweisung wertet den Ausdruck in Klammern aus und wandelt das Ergebnis in ein Boolean.

Erinnern wir uns an die Umwandlungsregeln aus dem Kapitel <info:type-conversions>:

- Eine Zahl `0`, eine leere Zeichenkette `""`, `null`, `undefined`, und `NaN` werden alle `false`. Deswegen werden sie "falsy" Werte genannt.
- Andere Werte werden `true`, folglich heißen sie "truthy".

Somit würde der Code unter dieser Bedingung niemals ausgeführt werden:

```js
if (0) { // 0 is falsy
  ...
}
```

...und innerhalb dieser Bedingung -- wird er es stets:

```js
if (1) { // 1 is truthy
  ...
}
```

Wir können auch vor-ausgewertete boolsche Werte an `if` übergeben, wie hier:

```js
let cond = (year == 2015); // equality evaluates to true or false

if (cond) {
  ...
}
```

## Die "else" Klausel

Die `if` Anweisung darf einen optionalen "else"-Block enthalten. Er wird ausgeführt, wenn die Bedinung unwahr ist.

Zum Beispiel:
```js run
let year = prompt('In which year was the ECMAScript-2015 specification published?', '');

if (year == 2015) {
  alert( 'You guessed it right!' );
} else {
  alert( 'How can you be so wrong?' ); // any value except 2015
}
```

## Mehrere Bedinungen: "else if"

Manchmal möchten wir mehrere Varianten einer Bedingung prüfen. Die `else if` Klausel lässt uns dies tun.

Zum Beispiel:

```js run
let year = prompt('In which year was the ECMAScript-2015 specification published?', '');

if (year < 2015) {
  alert( 'Too early...' );
} else if (year > 2015) {
  alert( 'Too late' );
} else {
  alert( 'Exactly!' );
}
```

In obigen Code prüft JavaScript erst `year < 2015`. Ist dies _falsy_ (s.o.), springt es zur nächsten Bedingung `year > 2015`. Ist dies ebenfalls _falsy_, zeigt es den letzten `alert` an.

Es kann mehrere `else if` Blöcke geben. Das letzte `else` ist optional.

## Bedingungsoperator '?'

Manchmal müssen wir einer Variablen eine Wert abhängig von einer Bedingung zuweisen.

Beispielsweise:

```js run no-beautify
let accessAllowed;
let age = prompt('How old are you?', '');

*!*
if (age > 18) {
  accessAllowed = true;
} else {
  accessAllowed = false;
}
*/!*

alert(accessAllowed);
```

Der sogenannte "Bedingungs" oder "Fragezeichen"-Operator lässt uns das auf einer kürzeren und einfacheren Weise tun.

Der Operator wird mittels eines Fragezeichen `?` dargestellt. Manchmal wird er als "ternary" bezeichnet, weil der Operator drei Operanden hat. Er ist sogar der einzige Operator in JavaScript, der so viele hat.

Die Syntax ist:
```js
let result = condition ? value1 : value2;
```

The `condition` is evaluated: if it's truthy then `value1` is returned, otherwise -- `value2`.
Die `condition` wird ausgewertet: ist sie _truthy_ wird `value1` zurück gegeben, anderenfalls -- `value2`.

Zum Beispiel:

```js
let accessAllowed = (age > 18) ? true : false;
```

Technisch können wir die Klammern um `age > 18` weglassen. Der Fragezeichenoperator hat eine niedrige Präzedenz, somit wird er nach dem Vergleich `>` ausgeführt.

This example will do the same thing as the previous one:

```js
// the comparison operator "age > 18" executes first anyway
// (no need to wrap it into parentheses)
let accessAllowed = age > 18 ? true : false;
```

But parentheses make the code more readable, so we recommend using them.

````smart
In the example above, you can avoid using the question mark operator because the comparison itself returns `true/false`:

```js
// the same
let accessAllowed = age > 18;
```
````

## Multiple '?'

A sequence of question mark operators `?` can return a value that depends on more than one condition.

For instance:
```js run
let age = prompt('age?', 18);

let message = (age < 3) ? 'Hi, baby!' :
  (age < 18) ? 'Hello!' :
  (age < 100) ? 'Greetings!' :
  'What an unusual age!';

alert( message );
```

It may be difficult at first to grasp what's going on. But after a closer look, we can see that it's just an ordinary sequence of tests:

1. The first question mark checks whether `age < 3`.
2. If true -- it returns `'Hi, baby!'`. Otherwise, it continues to the expression after the colon '":"', checking `age < 18`.
3. If that's true -- it returns `'Hello!'`. Otherwise, it continues to the expression after the next colon '":"', checking `age < 100`.
4. If that's true -- it returns `'Greetings!'`. Otherwise, it continues to the expression after the last colon '":"', returning `'What an unusual age!'`.

Here's how this looks using `if..else`:

```js
if (age < 3) {
  message = 'Hi, baby!';
} else if (age < 18) {
  message = 'Hello!';
} else if (age < 100) {
  message = 'Greetings!';
} else {
  message = 'What an unusual age!';
}
```

## Non-traditional use of '?'

Sometimes the question mark `?` is used as a replacement for `if`:

```js run no-beautify
let company = prompt('Which company created JavaScript?', '');

*!*
(company == 'Netscape') ?
   alert('Right!') : alert('Wrong.');
*/!*
```

Depending on the condition `company == 'Netscape'`, either the first or the second expression after the `?` gets executed and shows an alert.

We don't assign a result to a variable here. Instead, we execute different code depending on the condition.

**It's not recommended to use the question mark operator in this way.**

The notation is shorter than the equivalent `if` statement, which appeals to some programmers. But it is less readable.

Here is the same code using `if` for comparison:

```js run no-beautify
let company = prompt('Which company created JavaScript?', '');

*!*
if (company == 'Netscape') {
  alert('Right!');
} else {
  alert('Wrong.');
}
*/!*
```

Our eyes scan the code vertically. Code blocks which span several lines are easier to understand than a long, horizontal instruction set.

The purpose of the question mark operator `?` is to return one value or another depending on its condition. Please use it for exactly that. Use `if` when you need to execute different branches of code.
