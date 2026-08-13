# JavaScript A→Z Revision

## Phase 7 — Strings ⭐⭐⭐

> **Goal:** Master JavaScript strings and their commonly used methods. You should be able to manipulate, search, transform, and analyze strings without relying on documentation.

**Importance:** ⭐⭐⭐⭐
**Difficulty:** Beginner → Intermediate
**Interview Relevance:** Very High

> ⚠️ **Important:** Strings in JavaScript are **immutable**. String methods generally return a new string instead of modifying the original string.

---

## 📚 Contents

1. [What Is a String?](#1-what-is-a-string)
2. [String Immutability](#2-string-immutability)
3. [Basic String Methods](#3-basic-string-methods)

   * [length](#length)
   * [toUpperCase()](#touppercase)
   * [toLowerCase()](#tolowercase)
   * [trim()](#trim)
   * [includes()](#includes)
   * [startsWith()](#startswith)
   * [endsWith()](#endswith)
   * [indexOf()](#indexof)
   * [slice()](#slice)
   * [substring()](#substring)
   * [replace()](#replace)
   * [replaceAll()](#replaceall)
   * [split()](#split)
   * [charAt()](#charat)
   * [concat()](#concat)
   * [padStart()](#padstart)
   * [padEnd()](#padend)
4. [Important String Method Differences](#4-important-string-method-differences)
5. [Practice Problem 1 — Reverse a String](#5-practice-problem-1--reverse-a-string)
6. [Practice Problem 2 — Count Characters](#6-practice-problem-2--count-characters)
7. [Practice Problem 3 — Remove Duplicates](#7-practice-problem-3--remove-duplicates)
8. [Practice Problem 4 — Check Palindrome](#8-practice-problem-4--check-palindrome)
9. [Practice Problem 5 — Most Frequent Character](#9-practice-problem-5--most-frequent-character)
10. [Quick Revision](#10-quick-revision)
11. [Interview Questions](#11-interview-questions)
12. [Must Remember](#12-must-remember)
13. [Practice Checklist](#13-practice-checklist)

---

# 1. What Is a String?

A **string** is a sequence of characters used to represent text.

```js id="8n1q4m"
const name = "Aniket";
```

Strings can be created using:

```js id="d8m7x2"
const a = "Hello";
const b = 'Hello';
const c = `Hello`;
```

The backtick syntax creates a **template literal**.

---

## Template Literals

Template literals allow us to embed expressions inside strings.

```js id="k4p9s6"
const name = "Aniket";
const age = 21;

const message = `My name is ${name} and I am ${age} years old.`;

console.log(message);
```

Output:

```text id="z6x2c8"
My name is Aniket and I am 21 years old.
```

---

## String Indexing

Strings are indexed from `0`.

```js id="m7v3q1"
const name = "Aniket";
```

Conceptually:

```text id="n2k8p4"
A   n   i   k   e   t
0   1   2   3   4   5
```

Access a character:

```js id="x5c9r7"
console.log(name[0]);
```

Output:

```text id="f8m2w6"
A
```

---

# 2. String Immutability

Strings are **immutable** in JavaScript.

This means you cannot directly change an existing character inside a string.

```js id="q3v7m9"
let name = "Aniket";

name[0] = "R";

console.log(name);
```

Output remains:

```text id="k5x8p2"
Aniket
```

To change the string, create a new string:

```js id="j9m4c6"
name = "Rahul";
```

---

## Important

Methods such as:

```js id="b7r2x5"
toUpperCase()
trim()
replace()
slice()
```

do not modify the original string.

They return a new string.

```js id="v8n3q1"
const name = "aniket";

const upper = name.toUpperCase();

console.log(name);
console.log(upper);
```

Output:

```text id="s6k4m9"
aniket
ANIKET
```

---

# 3. Basic String Methods

---

# `length`

Returns the number of characters in a string.

```js id="h2p7v5"
const name = "Aniket";

console.log(name.length);
```

Output:

```text id="r8m3x1"
6
```

### Important

`length` is a **property**, not a function.

Correct:

```js id="n5c8q2"
name.length;
```

Not:

```js id="z4m7p9"
name.length();
```

---

# `toUpperCase()`

Converts a string to uppercase.

```js id="y3k8m1"
const name = "Aniket";

console.log(name.toUpperCase());
```

Output:

```text id="v6p2q9"
ANIKET
```

Original remains unchanged:

```js id="c7x4r5"
console.log(name);
```

Output:

```text id="m9n2w6"
Aniket
```

---

# `toLowerCase()`

Converts a string to lowercase.

```js id="p4v8x2"
const name = "ANIKET";

console.log(name.toLowerCase());
```

Output:

```text id="q7m3c9"
aniket
```

---

# `trim()`

Removes whitespace from the **beginning and end** of a string.

```js id="r5n8k2"
const username = "   Aniket   ";

console.log(username.trim());
```

Output:

```text id="x3m7v1"
Aniket
```

### Important

`trim()` does not remove spaces in the middle.

```js id="d8q2p6"
const text = "Hello   World";

console.log(text.trim());
```

Output:

```text id="f4k9m3"
Hello   World
```

---

## Real-World Use

Very common when handling form input:

```js id="s7x4c8"
const name = formData.name.trim();
```

This prevents accidental leading/trailing spaces.

---

# `includes()`

Checks whether a string contains another string.

```js id="q6m2v9"
const message = "JavaScript is powerful";

console.log(message.includes("JavaScript"));
```

Output:

```text id="p3x8k5"
true
```

Case-sensitive:

```js id="n7c4m1"
console.log(message.includes("javascript"));
```

Output:

```text id="r9v2q6"
false
```

---

# `startsWith()`

Checks whether a string starts with a specified substring.

```js id="w5k8p3"
const url = "https://example.com";

console.log(url.startsWith("https"));
```

Output:

```text id="m2x7c9"
true
```

### Real-World Use

```js id="a4v9n6"
if (url.startsWith("https://")) {
    console.log("Secure URL");
}
```

---

# `endsWith()`

Checks whether a string ends with a specified substring.

```js id="k3q8m5"
const file = "profile.png";

console.log(file.endsWith(".png"));
```

Output:

```text id="x7c2v9"
true
```

### Real-World Use

Checking file extensions:

```js id="n5m8r4"
file.endsWith(".pdf");
file.endsWith(".jpg");
file.endsWith(".png");
```

---

# `indexOf()`

Returns the index of the first occurrence of a substring.

```js id="c8p3v6"
const text = "JavaScript";

console.log(text.indexOf("Script"));
```

Output:

```text id="q4m7x2"
4
```

If the substring is not found:

```js id="z6n2k8"
console.log(text.indexOf("Python"));
```

Output:

```text id="b9v5r1"
-1
```

---

## Finding a Character

```js id="p2x8m4"
const text = "banana";

console.log(text.indexOf("a"));
```

Output:

```text id="f7c3n9"
1
```

It returns the **first occurrence**.

---

# `slice()`

`slice()` extracts part of a string and returns a new string.

### Syntax

```js id="r8m2q5"
string.slice(start, end);
```

The `end` index is not included.

```js id="v4x7c1"
const text = "JavaScript";

console.log(text.slice(0, 4));
```

Output:

```text id="k9p3m6"
Java
```

---

## Negative Index

`slice()` supports negative indexes.

```js id="n5c8x2"
const text = "JavaScript";

console.log(text.slice(-6));
```

Output:

```text id="q7m4v9"
Script
```

---

# `substring()`

`substring()` also extracts part of a string.

```js id="b3x8k5"
const text = "JavaScript";

console.log(text.substring(0, 4));
```

Output:

```text id="m6q2v7"
Java
```

---

## `slice()` vs `substring()`

This is important.

### Negative Values

`slice()` supports negative indexes:

```js id="x9m4c2"
"JavaScript".slice(-6);
```

Result:

```text id="z7p3n8"
"Script"
```

`substring()` treats negative values as `0`:

```js id="q5v8m1"
"JavaScript".substring(-6);
```

Result:

```text id="r2c9x4"
"JavaScript"
```

### Easy Rule

> Prefer `slice()` for modern JavaScript because its behavior with negative indexes is more intuitive and consistent with array `slice()`.

---

# `replace()`

Replaces the **first matching occurrence**.

```js id="m7x3q8"
const text = "Hello World World";

const result = text.replace("World", "JavaScript");

console.log(result);
```

Output:

```text id="v4n9c2"
Hello JavaScript World
```

Only the first `"World"` was replaced.

---

## Using Regular Expression

```js id="k8p2m5"
const text = "cat cat cat";

const result = text.replace(/cat/g, "dog");

console.log(result);
```

Output:

```text id="x3q7v1"
dog dog dog
```

---

# `replaceAll()`

Replaces all matching occurrences.

```js id="n5c8r2"
const text = "cat cat cat";

const result = text.replaceAll("cat", "dog");

console.log(result);
```

Output:

```text id="j7m4x9"
dog dog dog
```

### Difference

```text id="q2v8p5"
replace()
    ↓
First match by default

replaceAll()
    ↓
All matches
```

---

# `split()`

Converts a string into an array.

```js id="c6m9x3"
const text = "HTML,CSS,JavaScript";

const skills = text.split(",");

console.log(skills);
```

Output:

```js id="p4v7n2"
["HTML", "CSS", "JavaScript"]
```

---

## Split by Space

```js id="w8k3m5"
const text = "Hello World";

console.log(text.split(" "));
```

Output:

```js id="z6q2x9"
["Hello", "World"]
```

---

## Split Into Characters

```js id="r5m8c1"
const text = "Hello";

console.log(text.split(""));
```

Output:

```js id="n3v7p4"
["H", "e", "l", "l", "o"]
```

> For simple ASCII text this is common, but `split("")` is not a fully Unicode-safe way to iterate over all human-perceived characters.

---

# `charAt()`

Returns the character at a specified index.

```js id="x4q8m2"
const text = "Aniket";

console.log(text.charAt(0));
```

Output:

```text id="p7v3n9"
A
```

Equivalent common syntax:

```js id="k5m8c1"
text[0];
```

---

## `charAt()` vs Bracket Access

```js id="s2x7q4"
text.charAt(100);
```

returns:

```text id="f9m3v6"
""
```

while:

```js id="n8c2p5"
text[100];
```

returns:

```text id="w4q7m1"
undefined
```

---

# `concat()`

Combines strings.

```js id="m6v3x8"
const firstName = "Aniket";
const lastName = "Kole";

const fullName = firstName.concat(" ", lastName);

console.log(fullName);
```

Output:

```text id="r9p2c5"
Aniket Kole
```

### Modern Alternative

Template literals are usually easier to read:

```js id="k7m4x1"
const fullName = `${firstName} ${lastName}`;
```

---

# `padStart()`

Adds characters to the beginning until the string reaches the specified length.

```js id="v8q3n6"
const number = "42";

console.log(number.padStart(5, "0"));
```

Output:

```text id="m2x7p9"
00042
```

### Real-World Use

Formatting numbers:

```js id="c5r8m3"
const id = "42";

const formattedId = id.padStart(6, "0");

console.log(formattedId);
```

Output:

```text id="q9v4k1"
000042
```

---

# `padEnd()`

Adds characters to the end until the string reaches the specified length.

```js id="n6x2m8"
const text = "Hi";

console.log(text.padEnd(5, "."));
```

Output:

```text id="p4c7v9"
Hi...
```

---

# 4. Important String Method Differences

## `slice()` vs `substring()`

| Feature              | `slice()` | `substring()`   |
| -------------------- | --------- | --------------- |
| Extracts portion     | ✅         | ✅               |
| Negative indexes     | Supports  | Converts to `0` |
| End excluded         | ✅         | ✅               |
| Common modern choice | ✅         | Less common     |

---

## `replace()` vs `replaceAll()`

```text id="j8x3m6"
replace()
    ↓
First matching occurrence by default

replaceAll()
    ↓
All matching occurrences
```

---

## `includes()` vs `indexOf()`

```text id="q4v7n2"
includes()
    ↓
true / false

indexOf()
    ↓
index / -1
```

---

## `charAt()` vs `[]`

```js id="x6m9c3"
text.charAt(0);
text[0];
```

Both can retrieve a character, but their behavior differs when the index is out of range.

---

## `trim()` vs `replace()`

`trim()` removes whitespace from the beginning and end.

```js id="n3p8v5"
"  Hello  ".trim();
```

Result:

```text id="z7q2m4"
"Hello"
```

It does not remove internal whitespace.

---

# 5. Practice Problem 1 — Reverse a String

### Problem

Reverse:

```text id="k4m8x2"
"Aniket"
```

Expected:

```text id="v6p3q9"
"tekinA"
```

---

## Approach 1 — `split()`, `reverse()`, `join()`

```js id="c8n2m5"
function reverseString(str) {
    return str
        .split("")
        .reverse()
        .join("");
}

console.log(reverseString("Aniket"));
```

Output:

```text id="r7x4p1"
tekinA
```

---

## Approach 2 — Loop

```js id="m5q8v3"
function reverseString(str) {
    let result = "";

    for (let i = str.length - 1; i >= 0; i--) {
        result += str[i];
    }

    return result;
}
```

### Interview Point

Know how to solve this **without using `reverse()`**.

---

# 6. Practice Problem 2 — Count Characters

### Problem

Count the number of characters:

```js id="n8x3m7"
const text = "hello";
```

Expected:

```text id="q5v9c2"
5
```

The simplest solution:

```js id="r4m7p1"
function countCharacters(str) {
    return str.length;
}
```

---

## Count Each Character

A more useful interview problem is:

```text id="x8c3n6"
"hello"
```

Expected:

```js id="p2m7v4"
{
    h: 1,
    e: 1,
    l: 2,
    o: 1
}
```

### Solution

```js id="k6q9x2"
function countCharacters(str) {
    const frequency = {};

    for (const char of str) {
        frequency[char] = (frequency[char] || 0) + 1;
    }

    return frequency;
}

console.log(countCharacters("hello"));
```

---

## How It Works

For `"hello"`:

```text id="c4m8v1"
h → 1
e → 1
l → 1
l → 2
o → 1
```

Final:

```js id="w7x2p5"
{
    h: 1,
    e: 1,
    l: 2,
    o: 1
}
```

---

# 7. Practice Problem 3 — Remove Duplicates

### Problem

Given:

```text id="m3q8v6"
"programming"
```

Remove duplicate characters.

One possible result:

```text id="r7x4n2"
"progamin"
```

---

## Using `Set`

A `Set` stores unique values.

```js id="c5m9p1"
function removeDuplicates(str) {
    return [...new Set(str)].join("");
}

console.log(removeDuplicates("programming"));
```

---

## Without `Set`

For interview practice, implement it manually:

```js id="n8v2q7"
function removeDuplicates(str) {
    let result = "";

    for (const char of str) {
        if (!result.includes(char)) {
            result += char;
        }
    }

    return result;
}
```

### Why Practice Both?

The `Set` solution tests your knowledge of built-in data structures.

The manual solution tests:

* Loops
* Strings
* Conditions
* Searching
* Complexity

---

# 8. Practice Problem 4 — Check Palindrome

A **palindrome** reads the same forward and backward.

Examples:

```text id="x3m7p8"
madam
level
racecar
```

Not palindrome:

```text id="q6v2n4"
hello
javascript
```

---

## Basic Solution

```js id="r8c3m5"
function isPalindrome(str) {
    const reversed = str
        .split("")
        .reverse()
        .join("");

    return str === reversed;
}

console.log(isPalindrome("madam"));
```

Output:

```text id="j4x9p2"
true
```

---

## Without `reverse()`

```js id="v7m3q8"
function isPalindrome(str) {
    let left = 0;
    let right = str.length - 1;

    while (left < right) {
        if (str[left] !== str[right]) {
            return false;
        }

        left++;
        right--;
    }

    return true;
}
```

This is a better interview exercise because it uses the **two-pointer technique**.

---

## Case-Insensitive Palindrome

```js id="k2x8m4"
function isPalindrome(str) {
    str = str.toLowerCase();

    let left = 0;
    let right = str.length - 1;

    while (left < right) {
        if (str[left] !== str[right]) {
            return false;
        }

        left++;
        right--;
    }

    return true;
}
```

---

# 9. Practice Problem 5 — Most Frequent Character

### Problem

Given:

```text id="p5m8x2"
"javascript"
```

Find the character that appears most frequently.

---

## Approach

First create a frequency map.

```js id="c7n3q9"
function mostFrequentCharacter(str) {
    const frequency = {};

    for (const char of str) {
        frequency[char] = (frequency[char] || 0) + 1;
    }

    let maxChar = "";
    let maxCount = 0;

    for (const char in frequency) {
        if (frequency[char] > maxCount) {
            maxCount = frequency[char];
            maxChar = char;
        }
    }

    return maxChar;
}

console.log(mostFrequentCharacter("javascript"));
```

---

## How It Works

First:

```text id="m4x8q1"
j → 1
a → 2
v → 1
s → 1
c → 1
r → 1
i → 1
p → 1
t → 1
```

Then find the largest count.

Result:

```text id="n7c3m5"
a
```

---

# 10. Quick Revision

## Basic

```text id="x8m4q2"
length       → string length
toUpperCase  → uppercase
toLowerCase  → lowercase
trim         → remove outer whitespace
```

---

## Searching

```text id="p6v2n8"
includes     → contains?
startsWith   → starts with?
endsWith     → ends with?
indexOf      → first position
```

---

## Extracting

```text id="c3m7x9"
slice        → extract portion
substring    → extract portion
charAt       → character at index
```

---

## Modifying

```text id="q5n8m2"
replace      → replace first match by default
replaceAll   → replace all matches
concat       → combine strings
```

---

## Converting

```text id="v4x9c6"
split        → string → array
```

---

## Formatting

```text id="m8q3p1"
padStart     → pad beginning
padEnd       → pad ending
```

---

# 11. Interview Questions

## Basic

1. What is a string in JavaScript?
2. Are JavaScript strings mutable or immutable?
3. How do you find the length of a string?
4. What does `toUpperCase()` do?
5. What does `trim()` do?
6. What does `includes()` return?
7. What does `indexOf()` return when a value is not found?
8. What does `split()` do?
9. What does `charAt()` do?
10. What does `padStart()` do?

---

## Intermediate

11. What is the difference between `slice()` and `substring()`?
12. What is the difference between `replace()` and `replaceAll()`?
13. What is the difference between `includes()` and `indexOf()`?
14. Does `trim()` remove spaces from the middle of a string?
15. Why are strings immutable?
16. How do you reverse a string?
17. How do you check whether a string is a palindrome?
18. How do you count the frequency of each character?
19. How do you remove duplicate characters?
20. How do you find the most frequent character?

---

## ⭐ Advanced Practice

21. Reverse a string without using `reverse()`.
22. Check if two strings are anagrams.
23. Find the first non-repeating character.
24. Find the first repeating character.
25. Count vowels and consonants.
26. Capitalize the first letter of every word.
27. Reverse the words in a sentence.
28. Find the longest word in a sentence.
29. Find the longest substring without repeating characters.
30. Compress a string such as `"aaabbc"` → `"a3b2c1"`.

---

# 12. Must Remember

## ⭐ Strings Are Immutable

```js id="z3m8q5"
const text = "Hello";

text[0] = "Y";

console.log(text);
```

Still:

```text id="x7c2p9"
Hello
```

Methods return new strings.

---

## ⭐ Search

```text id="m4n8x1"
includes()
    ↓
true / false

indexOf()
    ↓
index / -1

startsWith()
    ↓
true / false

endsWith()
    ↓
true / false
```

---

## ⭐ Extract

```text id="q6v3m9"
slice(start, end)
substring(start, end)
```

Remember:

> `slice()` supports negative indexes.

---

## ⭐ Replace

```text id="p8x2c4"
replace()
    ↓
first match by default

replaceAll()
    ↓
all matches
```

---

## ⭐ Convert

```text id="n5m7q3"
"hello world".split(" ")
```

becomes:

```js id="r9x4c1"
["hello", "world"]
```

And the reverse process can be done with:

```js id="v2m8p6"
["hello", "world"].join(" ")
```

Result:

```text id="k4q7x9"
"hello world"
```

---

# 🧠 Final Mental Model

Think of string methods by **purpose**, not alphabetically:

```text id="d7m3x8"
                    STRING
                       │
       ┌───────────────┼────────────────┐
       │               │                │
     INSPECT         SEARCH           MODIFY
       │               │                │
     length          includes         replace
                     startsWith       replaceAll
                     endsWith
                     indexOf
       │
       └──────────────────────────────────
                       │
                    EXTRACT
                       │
                  slice / substring
                       │
       ┌───────────────┴───────────────┐
       │                               │
    CONVERT                         FORMAT
       │                               │
     split                         padStart
     join                          padEnd
```

---

# 🔥 Practice Without Documentation

Try solving these **without looking at the methods first**:

### 1. Reverse

```js id="m8q4v2"
reverseString("Aniket");
// "tekinA"
```

### 2. Character Frequency

```js id="x5c9n7"
countCharacters("hello");
// { h: 1, e: 1, l: 2, o: 1 }
```

### 3. Remove Duplicates

```js id="p7m3x8"
removeDuplicates("programming");
// "progamin"
```

### 4. Palindrome

```js id="q4v9c2"
isPalindrome("racecar");
// true
```

### 5. Most Frequent Character

```js id="n6x2m5"
mostFrequentCharacter("javascript");
// "a"
```

---

## 🎯 Phase 7 Checklist

* [ ] I know `length`.
* [ ] I know `toUpperCase()`.
* [ ] I know `toLowerCase()`.
* [ ] I know `trim()`.
* [ ] I know `includes()`.
* [ ] I know `startsWith()`.
* [ ] I know `endsWith()`.
* [ ] I know `indexOf()`.
* [ ] I know `slice()`.
* [ ] I know `substring()`.
* [ ] I know `replace()`.
* [ ] I know `replaceAll()`.
* [ ] I know `split()`.
* [ ] I know `charAt()`.
* [ ] I know `concat()`.
* [ ] I know `padStart()`.
* [ ] I know `padEnd()`.
* [ ] I understand string immutability.
* [ ] I can reverse a string.
* [ ] I can count character frequencies.
* [ ] I can remove duplicate characters.
* [ ] I can check for a palindrome.
* [ ] I can find the most frequent character.
* [ ] I can solve string problems without relying on documentation.

---

**Phase 7 Status:** 🟡 Revise + Practice

**Next:** Phase 8 — Numbers + Math + Date/Time
