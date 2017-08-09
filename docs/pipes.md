## Built-in Pipes

### async

```html
<span>{{ observable_or_promise_expression | async }}</span>
```

The `async` pipe subscribes to an `Observable` or `Promise` and returns the latest value it has emitted. When a new value is emitted, the `async` pipe marks the component to be checked for changes. When the component gets destroyed, the `async` pipe unsubscribes automatically to avoid potential memory leaks.

### date

```html
<span>{{ dateObj | date:'mmss' }}</span>
```

Formats a date according to locale rules.

*   `expression` is a date object or a number (milliseconds since UTC epoch) or an ISO string ([https://www.w3.org/TR/NOTE-datetime](https://www.w3.org/TR/NOTE-datetime)).
*   `format` indicates which date/time components to include. The format can be predefined as shown below or custom as shown in the table.
    *   `'medium'`: equivalent to `'yMMMdjms'` (e.g. `Sep 3, 2010, 12:05:08 PM` for `en-US`)
    *   `'short'`: equivalent to `'yMdjm'` (e.g. `9/3/2010, 12:05 PM` for `en-US`)
    *   `'fullDate'`: equivalent to `'yMMMMEEEEd'` (e.g. `Friday, September 3, 2010` for `en-US`)
    *   `'longDate'`: equivalent to `'yMMMMd'` (e.g. `September 3, 2010` for `en-US`)
    *   `'mediumDate'`: equivalent to `'yMMMd'` (e.g. `Sep 3, 2010` for `en-US`)
    *   `'shortDate'`: equivalent to `'yMd'` (e.g. `9/3/2010` for `en-US`)
    *   `'mediumTime'`: equivalent to `'jms'` (e.g. `12:05:08 PM` for `en-US`)
    *   `'shortTime'`: equivalent to `'jm'` (e.g. `12:05 PM` for `en-US`)

#### Modifiers Table

| Component | Symbol | Narrow | Short Form | Long Form | Numeric | 2-digit |
| --- | --- | --- | --- | --- | --- | --- |
| era | G | G (A) | GGG (AD) | GGGG (Anno Domini) | - | - |
| year | y | - | - | - | y (2015) | yy (15) |
| month | M | L (S) | MMM (Sep) | MMMM (September) | M (9) | MM (09) |
| day | d | - | - | - | d (3) | dd (03) |
| weekday | E | E (S) | EEE (Sun) | EEEE (Sunday) | - | - |
| hour | j | - | - | - | j (13) | jj (13) |
| hour12 | h | - | - | - | h (1 PM) | hh (01 PM) |
| hour24 | H | - | - | - | H (13) | HH (13) |
| minute | m | - | - | - | m (5) | mm (05) |
| second | s | - | - | - | s (9) | ss (09) |
| timezone | z | - | - | z (Pacific Standard Time) | - | - |
| timezone | Z | - | Z (GMT-8:00) | - | - | - |
| timezone | a | - | a (PM) | - | - | - |

### i18nPlural

```html
<span>{{ expression | i18nPlural:mapping }}</span>
```

Maps a value to a string that pluralizes the value according to locale rules.

*   `expression` is a number.
*   `mapping` is an object that mimics the ICU format, see [http://userguide.icu-project.org/formatparse/messages](http://userguide.icu-project.org/formatparse/messages)

```ts
@Component({
  selector: 'i18n-plural-pipe',
  template: `<div>{{ messages.length | i18nPlural: messageMapping }}</div>`
})
export class I18nPluralPipeComponent {
  messages: any[] = ['Message 1'];
  messageMapping:
      {[k: string]: string} = {
          '=0': 'No messages.',
          '=1': 'One message.',
          'other': '# messages.'};
}
```

### i18nSelect

```html
<span>{{ expression | i18nSelect:mapping }}</span>
```

Generic selector that displays the string that matches the current value.

```ts
@Component({
    selector: 'i18n-select-pipe',
    template: `{{gender | i18nSelect: inviteMap}} `})
export class I18nSelectPipeComponent {
  gender: string = 'male';
  inviteMap: any = {
    'male': 'Invite him.',
    'female': 'Invite her.',
    'other': 'Invite them.'};
}
```

### json

```html
<span>{{ expression | json }}</span>
```
Converts value into string using JSON.stringify. Useful for debugging.

### lowercase

```html
<span>{{ string | lowercase }}</span>
```

Transforms text to lowercase.

### currency

```html
<p>A: {{number | currency:'USD':false}}</p>
<p>B: {{number | currency:'USD':true:'4.2-2'}}</p>
```

Formats a number as currency using locale rules.

Use `currency` to format a number as currency.

*   `currencyCode` is the [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) currency code, such as `USD` for the US dollar and `EUR` for the euro.
*   `symbolDisplay` is a boolean indicating whether to use the currency symbol or code.
    *   `true`: use symbol (e.g. `<div class="description").
    *   `false` (default): use code (e.g. `USD`).
*   `digitInfo` See `DecimalPipe` for detailed description.

### number

```html
<p>e (3.1-5): {{e | number:'3.1-5'}}</p>
```

Formats a number as text. Group sizing and separator and other locale-specific configurations are based on the active locale.

*   `digitInfo` is a `string` which has a following format:  
    `{minIntegerDigits}.{minFractionDigits}-{maxFractionDigits}`
*   `minIntegerDigits` is the minimum number of integer digits to use. Defaults to `1`.
*   `minFractionDigits` is the minimum number of digits after fraction. Defaults to `0`.
*   `maxFractionDigits` is the maximum number of digits after fraction. Defaults to `3`.

### percent

```html
<p>B: {{b | percent:'4.3-5'}}</p>
```

Formats a number as percentage. See DecimalPipe for detailed description.

### slice

```html
<span>{{ array_or_string_expression | slice:start[:end] }}</span>
<li *ngFor="let i of collection | slice:1:3">{{i}}</li>
```

Creates a new List or String containing a subset (slice) of the elements.

Where the input expression is a `List` or `String`, and:

*   `start`: The starting index of the subset to return.
    *   a positive integer: return the item at `start` index and all items after in the list or string expression.
    *   a negative integer: return the item at `start` index from the end and all items after in the list or string expression.
    *   if positive and greater than the size of the expression: return an empty list or string.
    *   if negative and greater than the size of the expression: return entire list or string.
*   `end`: The ending index of the subset to return.

    *   omitted: return all items until the end.
    *   if positive: return all items before `end` index of the list or string.
    *   if negative: return all items before `end` index from the end of the list or string.

All behavior is based on the expected behavior of the JavaScript API Array.prototype.slice() and String.prototype.slice().

### uppercase

```html
<span>{{ string | uppercase }}</span>
```

Transforms text to uppercase.

### titlecase

```html
<span>{{ string | titlecase }}</span>
```

Transforms text to titlecase.

## Custom Pipes

```ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({name: 'exponentialStrength'})
export class ExponentialStrengthPipe implements PipeTransform {
  transform(value: number, exponent: string): number {
    let exp = parseFloat(exponent);
    return Math.pow(value, isNaN(exp) ? 1 : exp);
  }
}
```
```html
<span>{{ 2 |  exponentialStrength: 10}}}</span>
```

* A pipe is a class decorated with pipe metadata.
* The pipe class implements the `PipeTransform` interface's `transform` method that accepts an input value followed by optional parameters and returns the transformed value.
* There will be one additional argument to the `transform` method for each parameter passed to the pipe. Your pipe has one such parameter: the `exponent`.
* To tell Angular that this is a pipe, you apply the `@Pipe` decorator, which you import from the core Angular library.
* The `@Pipe` decorator allows you to define the pipe name that you'll use within template expressions. It must be a valid JavaScript identifier. Your pipe's name is `exponentialStrength`.