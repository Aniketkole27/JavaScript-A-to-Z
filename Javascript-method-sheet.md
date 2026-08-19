# JavaScript Methods Cheat Sheet

> **Purpose:** Quick revision of commonly used JavaScript built-in methods, categorized by topic with a simple definition and a small example for every method.

* * *

## 📚 Contents

* [Array Methods](#-array-methods)
  * [Iteration](#iteration)
  * [Searching](#searching)
  * [Adding / Removing](#adding--removing)
  * [Rearranging](#rearranging)
  * [Combining / Flattening](#combining--flattening)
  * [Conversion](#conversion)
* [String Methods](#-string-methods)
  * [Searching](#searching-1)
  * [Extracting](#extracting)
  * [Modifying](#modifying)
  * [Case Conversion](#case-conversion)
  * [Splitting](#splitting)
* [Math Methods](#-math-methods)
* [Date Methods](#-date-methods)
  * [Getters](#getters)
  * [Setters](#setters)
  * [Formatting](#formatting)
* [Number Methods](#-number-methods)
* [Object Methods](#-object-methods)
* [JSON Methods](#-json-methods)
* [Set Methods](#-set-methods)
* [Map Methods](#-map-methods)
* [Timer Methods](#-timer-methods)
* [URL Methods](#-url-methods)
* [Promise Methods](#-promise-methods)
* [Symbol Methods](#-symbol-methods)
* [Interview Priority](#-interview-priority)

* * *

# 📦 Array Methods

## Iteration

### `forEach()`

**Definition:** Executes a function once for every array element.

    const nums = [1, 2, 3];
    
    nums.forEach(num => console.log(num));
    // 1
    // 2
    // 3

### `map()`

**Definition:** Transforms every element and returns a new array.

    const nums = [1, 2, 3];
    
    const doubled = nums.map(num => num * 2);
    
    console.log(doubled);
    // [2, 4, 6]

### `filter()`

**Definition:** Returns a new array containing elements that pass a condition.

    const nums = [1, 2, 3, 4];
    
    const even = nums.filter(num => num % 2 === 0);
    
    console.log(even);
    // [2, 4]

### `reduce()`

**Definition:** Reduces an array into a single accumulated value.

    const nums = [1, 2, 3];
    
    const sum = nums.reduce((total, num) => total + num, 0);
    
    console.log(sum);
    // 6

### `reduceRight()`

**Definition:** Reduces an array from right to left.

    const letters = ["a", "b", "c"];
    
    const result = letters.reduceRight(
        (acc, letter) => acc + letter,
        ""
    );
    
    console.log(result);
    // "cba"

### `find()`

**Definition:** Returns the first element that matches a condition.

    const nums = [5, 10, 15];
    
    const result = nums.find(num => num > 8);
    
    console.log(result);
    // 10

### `findIndex()`

**Definition:** Returns the index of the first element that matches a condition.

    const nums = [5, 10, 15];
    
    const index = nums.findIndex(num => num > 8);
    
    console.log(index);
    // 1

### `findLast()`

**Definition:** Returns the last element that matches a condition.

    const nums = [5, 10, 15, 20];
    
    const result = nums.findLast(num => num > 8);
    
    console.log(result);
    // 20

### `findLastIndex()`

**Definition:** Returns the index of the last element that matches a condition.

    const nums = [5, 10, 15, 20];
    
    const index = nums.findLastIndex(num => num > 8);
    
    console.log(index);
    // 3

### `some()`

**Definition:** Returns `true` if at least one element passes the condition.

    const nums = [1, 3, 4];
    
    console.log(nums.some(num => num % 2 === 0));
    // true

### `every()`

**Definition:** Returns `true` if every element passes the condition.

    const nums = [2, 4, 6];
    
    console.log(nums.every(num => num % 2 === 0));
    // true

### `flatMap()`

**Definition:** Maps each element and then flattens the result by one level.

    const nums = [1, 2, 3];
    
    const result = nums.flatMap(num => [num, num * 2]);
    
    console.log(result);
    // [1, 2, 2, 4, 3, 6]

* * *

## Searching

### `includes()`

**Definition:** Checks whether an element exists in the array.

    const fruits = ["apple", "banana"];
    
    console.log(fruits.includes("banana"));
    // true

### `indexOf()`

**Definition:** Returns the first index of a specified element.

    const nums = [10, 20, 30];
    
    console.log(nums.indexOf(20));
    // 1

### `lastIndexOf()`

**Definition:** Returns the last index of a specified element.

    const nums = [10, 20, 10];
    
    console.log(nums.lastIndexOf(10));
    // 2

* * *

## Adding / Removing

### `push()`

**Definition:** Adds one or more elements to the end and returns the new length.

    const nums = [1, 2];
    
    nums.push(3);
    
    console.log(nums);
    // [1, 2, 3]

> ⚠️ Mutates the original array.

### `pop()`

**Definition:** Removes and returns the last element.

    const nums = [1, 2, 3];
    
    const last = nums.pop();
    
    console.log(last);
    // 3

> ⚠️ Mutates the original array.

### `shift()`

**Definition:** Removes and returns the first element.

    const nums = [1, 2, 3];
    
    const first = nums.shift();
    
    console.log(first);
    // 1

> ⚠️ Mutates the original array.

### `unshift()`

**Definition:** Adds one or more elements to the beginning and returns the new length.

    const nums = [2, 3];
    
    nums.unshift(1);
    
    console.log(nums);
    // [1, 2, 3]

> ⚠️ Mutates the original array.

### `splice()`

**Definition:** Adds, removes, or replaces elements at a specified position.

    const nums = [1, 2, 4];
    
    nums.splice(2, 0, 3);
    
    console.log(nums);
    // [1, 2, 3, 4]

> ⚠️ Mutates the original array.

### `slice()`

**Definition:** Returns a shallow copy of part of an array without modifying the original.

    const nums = [1, 2, 3, 4];
    
    const result = nums.slice(1, 3);
    
    console.log(result);
    // [2, 3]

* * *

## Rearranging

### `sort()`

**Definition:** Sorts an array in place and mutates the original array.

    const nums = [30, 10, 20];
    
    nums.sort((a, b) => a - b);
    
    console.log(nums);
    // [10, 20, 30]

> ⚠️ Mutates the original array.

### `reverse()`

**Definition:** Reverses an array in place.

    const nums = [1, 2, 3];
    
    nums.reverse();
    
    console.log(nums);
    // [3, 2, 1]

> ⚠️ Mutates the original array.

### `toSorted()`

**Definition:** Returns a sorted copy without mutating the original array.

    const nums = [30, 10, 20];
    
    const sorted = nums.toSorted((a, b) => a - b);
    
    console.log(sorted);
    // [10, 20, 30]

### `toReversed()`

**Definition:** Returns a reversed copy without mutating the original array.

    const nums = [1, 2, 3];
    
    const reversed = nums.toReversed();
    
    console.log(reversed);
    // [3, 2, 1]

### `with()`

**Definition:** Returns a copy of an array with one element replaced.

    const nums = [10, 20, 30];
    
    const result = nums.with(1, 99);
    
    console.log(result);
    // [10, 99, 30]

### `copyWithin()`

**Definition:** Copies part of an array to another position within the same array.

    const nums = [1, 2, 3, 4];
    
    nums.copyWithin(1, 2);
    
    console.log(nums);
    // [1, 3, 4, 4]

> ⚠️ Mutates the original array.

### `fill()`

**Definition:** Replaces array elements with a static value within a specified range.

    const nums = [1, 2, 3];
    
    nums.fill(0);
    
    console.log(nums);
    // [0, 0, 0]

> ⚠️ Mutates the original array.

* * *

## Combining / Flattening

### `concat()`

**Definition:** Combines arrays and values into a new array.

    const a = [1, 2];
    const b = [3, 4];
    
    const result = a.concat(b);
    
    console.log(result);
    // [1, 2, 3, 4]

### `flat()`

**Definition:** Flattens nested arrays by a specified depth.

    const nums = [1, [2, [3]]];
    
    console.log(nums.flat(2));
    // [1, 2, 3]

### `join()`

**Definition:** Converts array elements into a string using a separator.

    const words = ["Hello", "World"];
    
    console.log(words.join(" "));
    // "Hello World"

* * *

## Conversion

### `entries()`

**Definition:** Returns an iterator containing `[index, value]` pairs.

    const fruits = ["apple", "banana"];
    
    console.log([...fruits.entries()]);
    // [[0, "apple"], [1, "banana"]]

### `keys()`

**Definition:** Returns an iterator containing array indexes.

    const fruits = ["apple", "banana"];
    
    console.log([...fruits.keys()]);
    // [0, 1]

### `values()`

**Definition:** Returns an iterator containing array values.

    const fruits = ["apple", "banana"];
    
    console.log([...fruits.values()]);
    // ["apple", "banana"]

### `toString()`

**Definition:** Converts array elements into a comma-separated string.

    const nums = [1, 2, 3];
    
    console.log(nums.toString());
    // "1,2,3"

### `toLocaleString()`

**Definition:** Converts array elements to a locale-sensitive string representation.

    const nums = [1000, 2000];
    
    console.log(nums.toLocaleString("en-US"));
    // "1,000,2,000"

* * *

# 🔤 String Methods

> ⚠️ Strings are immutable in JavaScript; string methods return new strings instead of modifying the original.

## Searching

### `includes()`

**Definition:** Checks whether a substring exists in the string.

    const text = "Hello World";
    
    console.log(text.includes("World"));
    // true

### `startsWith()`

**Definition:** Checks whether a string starts with a specified substring.

    const text = "JavaScript";
    
    console.log(text.startsWith("Java"));
    // true

### `endsWith()`

**Definition:** Checks whether a string ends with a specified substring.

    const file = "photo.png";
    
    console.log(file.endsWith(".png"));
    // true

### `indexOf()`

**Definition:** Returns the index of the first occurrence of a substring.

    const text = "Hello World";
    
    console.log(text.indexOf("World"));
    // 6

### `lastIndexOf()`

**Definition:** Returns the index of the last occurrence of a substring.

    const text = "hello hello";
    
    console.log(text.lastIndexOf("hello"));
    // 6

### `search()`

**Definition:** Searches a string using a regular expression and returns the matching index.

    const text = "Hello123";
    
    console.log(text.search(/\d/));
    // 5

### `match()`

**Definition:** Returns matches found using a regular expression.

    const text = "I have 10 apples";
    
    console.log(text.match(/\d+/));
    // ["10"]

### `matchAll()`

**Definition:** Returns an iterator containing all matches of a global regular expression.

    const text = "a1 b2";
    
    const matches = [...text.matchAll(/\d/g)];
    
    console.log(matches.map(match => match[0]));
    // ["1", "2"]

* * *

## Extracting

### `slice()`

**Definition:** Extracts a portion of a string and returns a new string.

    const text = "JavaScript";
    
    console.log(text.slice(0, 4));
    // "Java"

### `substring()`

**Definition:** Returns characters between two indexes.

    const text = "JavaScript";
    
    console.log(text.substring(0, 4));
    // "Java"

### `at()`

**Definition:** Returns the character at an index and supports negative indexes.

    const text = "Hello";
    
    console.log(text.at(-1));
    // "o"

### `charAt()`

**Definition:** Returns the character at a specified index.

    const text = "Hello";
    
    console.log(text.charAt(1));
    // "e"

### `charCodeAt()`

**Definition:** Returns the UTF-16 code unit at a specified index.

    console.log("A".charCodeAt(0));
    // 65

### `codePointAt()`

**Definition:** Returns the Unicode code point at a specified index.

    console.log("😀".codePointAt(0));
    // 128512

* * *

## Modifying

### `replace()`

**Definition:** Replaces the first matching occurrence unless a global pattern is used.

    const text = "Hello World";
    
    console.log(text.replace("World", "JS"));
    // "Hello JS"

### `replaceAll()`

**Definition:** Replaces all matching occurrences.

    const text = "cat cat";
    
    console.log(text.replaceAll("cat", "dog"));
    // "dog dog"

### `concat()`

**Definition:** Combines strings into a new string.

    const first = "Hello";
    const second = "World";
    
    console.log(first.concat(" ", second));
    // "Hello World"

### `repeat()`

**Definition:** Returns a new string containing the original repeated a specified number of times.

    console.log("Hi ".repeat(3));
    // "Hi Hi Hi "

### `trim()`

**Definition:** Removes whitespace from both ends of a string.

    console.log("  Hello  ".trim());
    // "Hello"

### `trimStart()`

**Definition:** Removes whitespace from the beginning of a string.

    console.log("  Hello".trimStart());
    // "Hello"

### `trimEnd()`

**Definition:** Removes whitespace from the end of a string.

    console.log("Hello  ".trimEnd());
    // "Hello"

### `padStart()`

**Definition:** Pads the beginning of a string until it reaches a target length.

    console.log("5".padStart(3, "0"));
    // "005"

### `padEnd()`

**Definition:** Pads the end of a string until it reaches a target length.

    console.log("5".padEnd(3, "0"));
    // "500"

* * *

## Case Conversion

### `toUpperCase()`

**Definition:** Converts a string to uppercase.

    console.log("hello".toUpperCase());
    // "HELLO"

### `toLowerCase()`

**Definition:** Converts a string to lowercase.

    console.log("HELLO".toLowerCase());
    // "hello"

### `toLocaleUpperCase()`

**Definition:** Converts a string to uppercase using locale-sensitive rules.

    console.log("hello".toLocaleUpperCase("en-US"));
    // "HELLO"

### `toLocaleLowerCase()`

**Definition:** Converts a string to lowercase using locale-sensitive rules.

    console.log("HELLO".toLocaleLowerCase("en-US"));
    // "hello"

* * *

## Splitting

### `split()`

**Definition:** Splits a string into an array using a separator.

    const text = "HTML,CSS,JS";
    
    console.log(text.split(","));
    // ["HTML", "CSS", "JS"]

* * *

# 🔢 Math Methods

### `Math.abs()`

**Definition:** Returns the absolute value of a number.

    console.log(Math.abs(-10));
    // 10

### `Math.ceil()`

**Definition:** Rounds a number upward to the nearest integer.

    console.log(Math.ceil(4.2));
    // 5

### `Math.floor()`

**Definition:** Rounds a number downward to the nearest integer.

    console.log(Math.floor(4.9));
    // 4

### `Math.round()`

**Definition:** Rounds a number to the nearest integer.

    console.log(Math.round(4.6));
    // 5

### `Math.trunc()`

**Definition:** Removes the decimal portion without rounding.

    console.log(Math.trunc(4.9));
    // 4

### `Math.max()`

**Definition:** Returns the largest number from the provided arguments.

    console.log(Math.max(10, 20, 5));
    // 20

### `Math.min()`

**Definition:** Returns the smallest number from the provided arguments.

    console.log(Math.min(10, 20, 5));
    // 5

### `Math.random()`

**Definition:** Returns a pseudo-random number from `0` up to but not including `1`.

    console.log(Math.random());
    // Example: 0.7342

### `Math.pow()`

**Definition:** Raises a number to a specified power.

    console.log(Math.pow(2, 3));
    // 8

### `Math.sqrt()`

**Definition:** Returns the square root of a number.

    console.log(Math.sqrt(25));
    // 5

### `Math.cbrt()`

**Definition:** Returns the cube root of a number.

    console.log(Math.cbrt(27));
    // 3

### `Math.exp()`

**Definition:** Returns `e` raised to the specified power.

    console.log(Math.exp(1));
    // 2.718...

### `Math.log()`

**Definition:** Returns the natural logarithm of a number.

    console.log(Math.log(Math.E));
    // 1

### `Math.log10()`

**Definition:** Returns the base-10 logarithm of a number.

    console.log(Math.log10(100));
    // 2

### `Math.sin()`

**Definition:** Returns the sine of an angle in radians.

    console.log(Math.sin(Math.PI / 2));
    // 1

### `Math.cos()`

**Definition:** Returns the cosine of an angle in radians.

    console.log(Math.cos(0));
    // 1

### `Math.tan()`

**Definition:** Returns the tangent of an angle in radians.

    console.log(Math.tan(0));
    // 0

* * *

# 📅 Date Methods

## Getters

### `getDate()`

**Definition:** Returns the day of the month.

    const date = new Date("2026-08-19");
    
    console.log(date.getDate());
    // 19

### `getDay()`

**Definition:** Returns the day of the week as a number from `0` to `6`.

    const date = new Date("2026-08-19");
    
    console.log(date.getDay());
    // Example: 3

### `getMonth()`

**Definition:** Returns the month as a zero-based number from `0` to `11`.

    const date = new Date("2026-08-19");
    
    console.log(date.getMonth());
    // 7

> ⚠️ January is `0`, February is `1`, and December is `11`.

### `getFullYear()`

**Definition:** Returns the full year.

    const date = new Date("2026-08-19");
    
    console.log(date.getFullYear());
    // 2026

### `getHours()`

**Definition:** Returns the hour from the local date.

    const date = new Date();
    
    console.log(date.getHours());
    // Example: 14

### `getMinutes()`

**Definition:** Returns the minutes from the local date.

    const date = new Date();
    
    console.log(date.getMinutes());
    // Example: 30

### `getSeconds()`

**Definition:** Returns the seconds from the local date.

    const date = new Date();
    
    console.log(date.getSeconds());
    // Example: 45

### `getMilliseconds()`

**Definition:** Returns the milliseconds from the local date.

    const date = new Date();
    
    console.log(date.getMilliseconds());
    // Example: 123

### `getTime()`

**Definition:** Returns the timestamp in milliseconds since January 1, 1970 UTC.

    const date = new Date();
    
    console.log(date.getTime());
    // Example: 1787...

* * *

## Setters

### `setDate()`

**Definition:** Sets the day of the month.

    const date = new Date();
    
    date.setDate(25);
    
    console.log(date.getDate());
    // 25

### `setMonth()`

**Definition:** Sets the month using a zero-based value.

    const date = new Date();
    
    date.setMonth(0);
    
    console.log(date.getMonth());
    // 0

### `setFullYear()`

**Definition:** Sets the full year.

    const date = new Date();
    
    date.setFullYear(2030);
    
    console.log(date.getFullYear());
    // 2030

### `setHours()`

**Definition:** Sets the hour.

    const date = new Date();
    
    date.setHours(10);
    
    console.log(date.getHours());
    // 10

### `setMinutes()`

**Definition:** Sets the minutes.

    const date = new Date();
    
    date.setMinutes(30);
    
    console.log(date.getMinutes());
    // 30

### `setSeconds()`

**Definition:** Sets the seconds.

    const date = new Date();
    
    date.setSeconds(20);
    
    console.log(date.getSeconds());
    // 20

### `setMilliseconds()`

**Definition:** Sets the milliseconds.

    const date = new Date();
    
    date.setMilliseconds(500);
    
    console.log(date.getMilliseconds());
    // 500

### `setTime()`

**Definition:** Sets the date using a timestamp in milliseconds.

    const date = new Date();
    
    date.setTime(0);
    
    console.log(date.toISOString());
    // "1970-01-01T00:00:00.000Z"

* * *

## Formatting

### `toString()`

**Definition:** Converts a Date into a readable date-time string.

    const date = new Date();
    
    console.log(date.toString());
    // Example: "Wed Aug 19 2026 ..."

### `toDateString()`

**Definition:** Returns only the date portion as a readable string.

    const date = new Date();
    
    console.log(date.toDateString());
    // Example: "Wed Aug 19 2026"

### `toTimeString()`

**Definition:** Returns only the time portion as a readable string.

    const date = new Date();
    
    console.log(date.toTimeString());
    // Example: "14:30:00 GMT+0530 ..."

### `toISOString()`

**Definition:** Returns the date as an ISO 8601 string in UTC.

    const date = new Date("2026-08-19T00:00:00Z");
    
    console.log(date.toISOString());
    // "2026-08-19T00:00:00.000Z"

### `toLocaleDateString()`

**Definition:** Returns a locale-sensitive date string.

    const date = new Date("2026-08-19");
    
    console.log(
        date.toLocaleDateString("en-IN")
    );
    // Example: "19/8/2026"

### `toLocaleTimeString()`

**Definition:** Returns a locale-sensitive time string.

    const date = new Date();
    
    console.log(
        date.toLocaleTimeString("en-IN")
    );
    // Example: "2:30:00 pm"

### `toLocaleString()`

**Definition:** Returns a locale-sensitive date and time string.

    const date = new Date();
    
    console.log(
        date.toLocaleString("en-IN")
    );
    // Example: "19/8/2026, 2:30:00 pm"

* * *

# 🔢 Number Methods

### `toFixed()`

**Definition:** Returns a number formatted with a specified number of decimal places.

    const price = 99.456;
    
    console.log(price.toFixed(2));
    // "99.46"

> ⚠️ Returns a **string**, not a number.

### `toPrecision()`

**Definition:** Returns a number formatted to a specified number of significant digits.

    const num = 123.456;
    
    console.log(num.toPrecision(4));
    // "123.5"

### `toExponential()`

**Definition:** Returns a number using exponential notation.

    const num = 1234;
    
    console.log(num.toExponential(2));
    // "1.23e+3"

### `toString()`

**Definition:** Converts a number into a string.

    const num = 100;
    
    console.log(num.toString());
    // "100"

### `valueOf()`

**Definition:** Returns the primitive numeric value of a Number object or number.

    const num = new Number(10);
    
    console.log(num.valueOf());
    // 10

* * *

# 🌍 Object Methods

### `Object.keys()`

**Definition:** Returns an array containing an object's own enumerable property names.

    const user = {
        name: "Aniket",
        age: 21
    };
    
    console.log(Object.keys(user));
    // ["name", "age"]

### `Object.values()`

**Definition:** Returns an array containing an object's own enumerable property values.

    const user = {
        name: "Aniket",
        age: 21
    };
    
    console.log(Object.values(user));
    // ["Aniket", 21]

### `Object.entries()`

**Definition:** Returns an array containing `[key, value]` pairs.

    const user = {
        name: "Aniket",
        age: 21
    };
    
    console.log(Object.entries(user));
    // [["name", "Aniket"], ["age", 21]]

### `Object.fromEntries()`

**Definition:** Creates an object from an iterable of key-value pairs.

    const entries = [
        ["name", "Aniket"],
        ["age", 21]
    ];
    
    console.log(Object.fromEntries(entries));
    // { name: "Aniket", age: 21 }

### `Object.assign()`

**Definition:** Copies enumerable properties from source objects into a target object.

    const user = { name: "Aniket" };
    const extra = { age: 21 };
    
    const result = Object.assign({}, user, extra);
    
    console.log(result);
    // { name: "Aniket", age: 21 }

### `Object.freeze()`

**Definition:** Prevents adding, removing, or changing properties of an object.

    const user = Object.freeze({
        name: "Aniket"
    });
    
    user.name = "Rahul";
    
    console.log(user.name);
    // "Aniket"

> ⚠️ `Object.freeze()` is shallow.

### `Object.seal()`

**Definition:** Prevents adding or removing properties but allows existing properties to be changed.

    const user = Object.seal({
        name: "Aniket"
    });
    
    user.name = "Rahul";
    
    console.log(user.name);
    // "Rahul"

### `Object.hasOwn()`

**Definition:** Checks whether an object has a specified own property.

    const user = {
        name: "Aniket"
    };
    
    console.log(Object.hasOwn(user, "name"));
    // true

### `Object.create()`

**Definition:** Creates a new object using the specified object as its prototype.

    const person = {
        greet() {
            return "Hello";
        }
    };
    
    const user = Object.create(person);
    
    console.log(user.greet());
    // "Hello"

### `Object.is()`

**Definition:** Determines whether two values are the same using `SameValue` comparison.

    console.log(Object.is(10, 10));
    // true
    
    console.log(Object.is(NaN, NaN));
    // true

* * *

# 🗂️ JSON Methods

### `JSON.parse()`

**Definition:** Converts a JSON string into a JavaScript value.

    const json = '{"name":"Aniket"}';
    
    const user = JSON.parse(json);
    
    console.log(user.name);
    // "Aniket"

### `JSON.stringify()`

**Definition:** Converts a JavaScript value into a JSON string.

    const user = {
        name: "Aniket"
    };
    
    const json = JSON.stringify(user);
    
    console.log(json);
    // '{"name":"Aniket"}'

* * *

# 📦 Set Methods

### `add()`

**Definition:** Adds a value to a Set.

    const numbers = new Set();
    
    numbers.add(10);
    
    console.log(numbers);
    // Set(1) { 10 }

### `delete()`

**Definition:** Removes a value from a Set.

    const numbers = new Set([1, 2, 3]);
    
    numbers.delete(2);
    
    console.log(numbers);
    // Set(2) { 1, 3 }

### `has()`

**Definition:** Checks whether a value exists in a Set.

    const numbers = new Set([1, 2, 3]);
    
    console.log(numbers.has(2));
    // true

### `clear()`

**Definition:** Removes all values from a Set.

    const numbers = new Set([1, 2, 3]);
    
    numbers.clear();
    
    console.log(numbers.size);
    // 0

### `values()`

**Definition:** Returns an iterator containing Set values.

    const numbers = new Set([1, 2, 3]);
    
    console.log([...numbers.values()]);
    // [1, 2, 3]

### `keys()`

**Definition:** Returns an iterator containing Set values; for Set, it is equivalent to `values()`.

    const numbers = new Set([1, 2]);
    
    console.log([...numbers.keys()]);
    // [1, 2]

### `entries()`

**Definition:** Returns an iterator containing `[value, value]` pairs.

    const numbers = new Set([1, 2]);
    
    console.log([...numbers.entries()]);
    // [[1, 1], [2, 2]]

### `forEach()`

**Definition:** Executes a function once for each Set value.

    const numbers = new Set([1, 2, 3]);
    
    numbers.forEach(num => {
        console.log(num);
    });
    // 1
    // 2
    // 3

* * *

# 🗺️ Map Methods

### `set()`

**Definition:** Adds or updates a key-value pair in a Map.

    const users = new Map();
    
    users.set(1, "Aniket");
    
    console.log(users.get(1));
    // "Aniket"

### `get()`

**Definition:** Returns the value associated with a key.

    const users = new Map([
        [1, "Aniket"]
    ]);
    
    console.log(users.get(1));
    // "Aniket"

### `has()`

**Definition:** Checks whether a key exists in a Map.

    const users = new Map([
        [1, "Aniket"]
    ]);
    
    console.log(users.has(1));
    // true

### `delete()`

**Definition:** Removes an entry from a Map using its key.

    const users = new Map([
        [1, "Aniket"],
        [2, "Rahul"]
    ]);
    
    users.delete(2);
    
    console.log(users.size);
    // 1

### `clear()`

**Definition:** Removes all entries from a Map.

    const users = new Map([
        [1, "Aniket"]
    ]);
    
    users.clear();
    
    console.log(users.size);
    // 0

### `keys()`

**Definition:** Returns an iterator containing Map keys.

    const users = new Map([
        [1, "Aniket"],
        [2, "Rahul"]
    ]);
    
    console.log([...users.keys()]);
    // [1, 2]

### `values()`

**Definition:** Returns an iterator containing Map values.

    const users = new Map([
        [1, "Aniket"],
        [2, "Rahul"]
    ]);
    
    console.log([...users.values()]);
    // ["Aniket", "Rahul"]

### `entries()`

**Definition:** Returns an iterator containing `[key, value]` pairs.

    const users = new Map([
        [1, "Aniket"]
    ]);
    
    console.log([...users.entries()]);
    // [[1, "Aniket"]]

### `forEach()`

**Definition:** Executes a function once for each Map entry.

    const users = new Map([
        [1, "Aniket"],
        [2, "Rahul"]
    ]);
    
    users.forEach((name, id) => {
        console.log(id, name);
    });
    // 1 "Aniket"
    // 2 "Rahul"

* * *

# ⏰ Timer Methods

### `setTimeout()`

**Definition:** Schedules a callback to run once after at least the specified delay.

    setTimeout(() => {
        console.log("Hello");
    }, 1000);

### `clearTimeout()`

**Definition:** Cancels a timeout scheduled with `setTimeout()`.

    const timer = setTimeout(() => {
        console.log("Hello");
    }, 1000);
    
    clearTimeout(timer);

### `setInterval()`

**Definition:** Schedules a callback to run repeatedly after each interval.

    const timer = setInterval(() => {
        console.log("Running...");
    }, 1000);

### `clearInterval()`

**Definition:** Cancels an interval scheduled with `setInterval()`.

    const timer = setInterval(() => {
        console.log("Running...");
    }, 1000);
    
    clearInterval(timer);

* * *

# 🌐 URL Methods

### `new URL()`

**Definition:** Creates a URL object from a URL string.

    const url = new URL(
        "https://example.com/products?page=2"
    );
    
    console.log(url.pathname);
    // "/products"

### `searchParams.get()`

**Definition:** Returns the value of a query parameter.

    const url = new URL(
        "https://example.com?page=2"
    );
    
    console.log(url.searchParams.get("page"));
    // "2"

### `searchParams.set()`

**Definition:** Sets or replaces the value of a query parameter.

    const url = new URL(
        "https://example.com?page=2"
    );
    
    url.searchParams.set("page", "3");
    
    console.log(url.searchParams.get("page"));
    // "3"

### `searchParams.append()`

**Definition:** Adds another value for a query parameter.

    const url = new URL(
        "https://example.com?tag=js"
    );
    
    url.searchParams.append("tag", "react");
    
    console.log(url.searchParams.toString());
    // "tag=js&tag=react"

### `searchParams.delete()`

**Definition:** Removes a query parameter.

    const url = new URL(
        "https://example.com?page=2"
    );
    
    url.searchParams.delete("page");
    
    console.log(url.searchParams.toString());
    // ""

### `searchParams.has()`

**Definition:** Checks whether a query parameter exists.

    const url = new URL(
        "https://example.com?page=2"
    );
    
    console.log(url.searchParams.has("page"));
    // true

* * *

# ⚡ Promise Methods

## Instance Methods

### `then()`

**Definition:** Handles a fulfilled Promise and returns a new Promise.

    Promise.resolve(10)
        .then(value => value * 2)
        .then(value => console.log(value));
    
    // 20

### `catch()`

**Definition:** Handles a rejected Promise.

    Promise.reject("Error")
        .catch(error => console.log(error));
    
    // "Error"

### `finally()`

**Definition:** Runs after a Promise settles regardless of whether it fulfills or rejects.

    Promise.resolve("Done")
        .finally(() => {
            console.log("Finished");
        });
    
    // Finished

* * *

## Static Methods

### `Promise.all()`

**Definition:** Fulfills when all Promises fulfill and rejects if any Promise rejects.

    Promise.all([
        Promise.resolve("A"),
        Promise.resolve("B")
    ]).then(result => {
        console.log(result);
    });
    
    // ["A", "B"]

### `Promise.allSettled()`

**Definition:** Waits for all Promises to settle and returns every result.

    Promise.allSettled([
        Promise.resolve("A"),
        Promise.reject("Error")
    ]).then(result => {
        console.log(result);
    });
    
    // [
    //   { status: "fulfilled", value: "A" },
    //   { status: "rejected", reason: "Error" }
    // ]

### `Promise.any()`

**Definition:** Fulfills when the first Promise fulfills and rejects only when all Promises reject.

    Promise.any([
        Promise.reject("Error"),
        Promise.resolve("Success")
    ]).then(result => {
        console.log(result);
    });
    
    // "Success"

### `Promise.race()`

**Definition:** Settles with the first Promise that settles.

    Promise.race([
        Promise.resolve("First"),
        Promise.resolve("Second")
    ]).then(result => {
        console.log(result);
    });
    
    // "First"

### `Promise.resolve()`

**Definition:** Creates or returns a fulfilled Promise for a given value.

    Promise.resolve("Hello")
        .then(value => console.log(value));
    
    // "Hello"

### `Promise.reject()`

**Definition:** Creates a rejected Promise with the given reason.

    Promise.reject("Something went wrong")
        .catch(error => console.log(error));
    
    // "Something went wrong"

* * *

# 🔑 Symbol Methods

### `Symbol.for()`

**Definition:** Returns an existing global symbol for a key or creates one if it does not exist.

    const a = Symbol.for("user");
    const b = Symbol.for("user");
    
    console.log(a === b);
    // true

### `Symbol.keyFor()`

**Definition:** Returns the key associated with a global symbol.

    const symbol = Symbol.for("user");
    
    console.log(Symbol.keyFor(symbol));
    // "user"

* * *

# 💡 Interview Priority

For frontend and React interviews, master these first.

## 📦 Arrays

    map()
    filter()
    reduce()
    find()
    findIndex()
    some()
    every()
    sort()
    slice()
    splice()
    flat()
    flatMap()

## 🔤 Strings

    split()
    replace()
    replaceAll()
    slice()
    trim()
    includes()
    startsWith()
    endsWith()

## 🌍 Objects

    Object.keys()
    Object.values()
    Object.entries()
    Object.fromEntries()
    Object.assign()
    Object.freeze()
    Object.hasOwn()

## ⚡ Promises

    then()
    catch()
    finally()
    
    Promise.all()
    Promise.race()
    Promise.allSettled()
    Promise.any()

## 📅 Date

    getTime()
    toISOString()
    toLocaleDateString()
    toLocaleTimeString()

## 🔢 Math

    Math.random()
    Math.floor()
    Math.ceil()
    Math.round()
    Math.max()
    Math.min()

* * *

# 🧠 Must Remember

> `map()` → Transform every element and return a new array.

> `filter()` → Select elements that satisfy a condition.

> `reduce()` → Accumulate an array into a single result.

> `find()` → Return the first matching element.

> `some()` → Check whether at least one element matches.

> `every()` → Check whether all elements match.

> `slice()` → Copy/extract without mutation.

> `splice()` → Add/remove/replace and mutate.

> `sort()` / `reverse()` → Mutate the original array.

> `toSorted()` / `toReversed()` → Return new arrays.

> Strings are immutable.

> `Object.keys()` → Keys.

> `Object.values()` → Values.

> `Object.entries()` → Key-value pairs.

> `JSON.stringify()` → JavaScript value → JSON string.

> `JSON.parse()` → JSON string → JavaScript value.

> `Promise.all()` → All must fulfill.

> `Promise.allSettled()` → All must settle.

> `Promise.race()` → First settlement wins.

> `Promise.any()` → First fulfillment wins.

* * *

# 🎯 Practice Checklist

## Arrays

* [ ] I can explain `map()`.
* [ ] I can explain `filter()`.
* [ ] I can explain `reduce()`.
* [ ] I can explain `find()`.
* [ ] I understand `some()` and `every()`.
* [ ] I know `slice()` vs `splice()`.
* [ ] I know which array methods mutate.
* [ ] I understand `sort()` vs `toSorted()`.
* [ ] I understand `flat()` and `flatMap()`.

## Strings

* [ ] I understand `split()`.
* [ ] I understand `slice()` vs `substring()`.
* [ ] I understand `replace()` vs `replaceAll()`.
* [ ] I understand string immutability.
* [ ] I can use searching methods.

## Objects

* [ ] I know `Object.keys()`.
* [ ] I know `Object.values()`.
* [ ] I know `Object.entries()`.
* [ ] I know `Object.fromEntries()`.
* [ ] I understand `Object.assign()`.
* [ ] I understand `freeze()` vs `seal()`.

## Promises

* [ ] I know `then()`, `catch()`, and `finally()`.
* [ ] I understand `Promise.all()`.
* [ ] I understand `Promise.allSettled()`.
* [ ] I understand `Promise.race()`.
* [ ] I understand `Promise.any()`.

## Other

* [ ] I know common Math methods.
* [ ] I know common Date methods.
* [ ] I know `JSON.parse()` and `JSON.stringify()`.
* [ ] I know Set methods.
* [ ] I know Map methods.
* [ ] I know timer methods.
* [ ] I know basic URL methods.
* [ ] I know Symbol methods.

* * *

**Revision Level:** ⭐⭐⭐⭐⭐

**Focus:** Don't just memorize the method names. For every method, remember **what it does, what it returns, whether it mutates the original value, and when to use it.**
