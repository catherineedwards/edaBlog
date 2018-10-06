# Pseudocode

## Overview

The [calculator solution from freeCodeCamp](https://codepen.io/freeCodeCamp/pen/EPNZYW) comprises 3 files:

- HTML file - sets the layout of the calculator
- CSS file - sets the style of the calculator (notably uses the [Bourbon Sass toolset](https://www.bourbon.io/) by [Thoughtbot](https://thoughtbot.com/))
- JS file - defines the mechanisms of the calculator

I'm not going to pseudocode through the HTML or CSS files; I will just pseudocode through the JS file.

What I will note from the HTML file is that the buttons for numbers and operands(`÷`,`x`, `+`, and `-`) are `button` elements with text defined.

There is also text input field with its input disabled called `#answer`:

​ `input(type='text' disabled)#answer`

This is where all of our outputs are going to be displayed.

## JS calculator pseudocode

### Variables

Three variables are defined at the beginning:

- `entries` - an empty array
- `total` - an empty value (i.e. `0`)
- `temp` - an empty string

### I need a $(), $(), a $() is what I need...

I came across the `$` dollar sign for the first time in this file. [According to this article](https://www.thoughtco.com/and-in-javascript-2037515), this dollar sign is an _identifier_ - it identifies an object in the same way a name would. In fact, the dollar sign is commonly used as a shortcut to the function _document.getElementByID()_. Therefore, `$()` is a function that references an element from the DOM if you pass it to the id of that element.

So,

```javascript
$("button").on('click', function() {
	var val = $(this).text();
}
```

Assigns the value of text from the thing that was clicked to the variable `val`.

Also, this function holds _all_ of the calculator functions in itself! This means that the function is always listening for the buttons being clicked, and updating the variable `val` to hold the text value of the button that was last clicked.

Therefore, our actions are defined by the values stored in `val`.

### NaN が何?

If `val` is not Not-a-Number (i.e. it's a number, [employing a double negative](https://www.youtube.com/watch?v=3jNVlBInVEA)), or `val` is a decimal point `.`, the value provided is added to `temp`.

We take `val` values from `temp` and add them to `#answer` to display. We do this by using `substring()`.

### AC/DC

If `val` is equal to `AC`, we re-initialise/clear the values of `entries`, `temp` and `total`.

We also reinitialise/clear the `val` being displayed in `#answer`.

This is the "All Clear" function.

### CEase and desist

If `val` is equal to AC, we only re-initialise `temp` values.

We also reinitialise/clear the `val` being displayed in `#answer`.

This is the "Clear Entry" function.

### Ch-ch-ch-changes - there's gonna have to be a multiplier

Javascript does not recognise `x` as an operand for multiplication, but we are using `x` in the user interface of our calculator.

If `val` is equal to `x`, we need to:

- push all values stored in `temp` to `entries`
- push `*` to `entries`, and
- reintialise/clear `temp`.

### Divide and conquer

Similarly, Javascript does not recognise `÷` as an operand for division, but we are using `÷` in the user interface of our calculator.

If `val` is equal to `÷`, we need to:

- push all values stored in `temp` to `entries`
- push `/` to `entries`, and
- reinitialise/clear `temp`.

### Equality for all

Once the user clicks the equals button `=`, we need to perform the calculation based on what is in `temp` and `entries`.

First, push all entries from `temp` into `entries`.

Then, create a new variable that holds our result. In this calculator, it's called `nt`, but I am not sure of the thinking behind that naming. I might call it `result` in my calculator for clarity's take.

We need to look through all of the values in our `entries` array and understand what's been pushed into there by the buttons. We will need to create a `for` loop for looking through `entries.length`.

We will also create two new variables: one for looking at the next number after our symbol in the array (`entries[i+1]`), and one for our symbols in `entries[i]`. We will call them `nextNum` and `symbol` respectively.

In this `for` loop, we will be checking what the value is, and saying what to do with it:

- If the symbol is an addition `+`, we solve for `result += nextNum`
- If the symbol is a subtraction `-`, we solve for `result -= nextNum`
- If the symbol is a multiplication `*`, we solve for `result *= nextNum`
- If the symbol is a division `/`, we solve for `result /= nextNum`

### Don't be so negative!

If the `result` is less than zero, we will need to return `result` as an absolute number, and put the `-` symbol in front of the number.

### And the answer is...!

After looping through all the `entries` array, we will need to:

- push our `result` to `#answer` (passing it as an argument for `val`),
- reinitialise/clear our `entries` array, and
- reinitialise/clear our `temp` string.

### Don't push me - I'm a number on the edge!

If the user pushes a button straight after finishing their calculation, we don't want more numbers going straight to the `#answer` field. That's not how calculators are expected to work.

Instead, we want to let the user start another calculation.

If the user pushes anything `else` after our `if` statements for our operands/`symbols`, we need to:

- push `temp` to `entries`
- push `val` to `entries`, and
- reinitialise/clear our `temp` string.
