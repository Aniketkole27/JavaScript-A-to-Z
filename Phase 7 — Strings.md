<h1 align="center">JavaScript A→Z Revision</h1>
<p align="center"><b>Phase 7 — Strings</b></p>

<p align="center">
  <img src="https://img.shields.io/badge/JavaScript-ES2023-F7DF1E?logo=javascript&logoColor=black" />
  <img src="https://img.shields.io/badge/Phase-7%20of%20N-blue" />
  <img src="https://img.shields.io/badge/Focus-Methods%20%2B%20Classic%20Problems-orange" />
</p>

> Strings are **immutable** — every method here returns a **new** string, none of them modify the original.

## 📑 Contents
- [String Methods](#string-methods)
- [`slice` vs `substring` — the gotcha](#slice-vs-substring--the-gotcha)
- [🔧 Practice Problems](#-practice-problems)
  - [1. Reverse a String](#1-reverse-a-string)
  - [2. Count Characters](#2-count-characters)
  - [3. Remove Duplicates](#3-remove-duplicate-characters)
  - [4. Check Palindrome](#4-check-palindrome)
  - [5. Most Frequent Character](#5-find-most-frequent-character)
- [🧪 Self-Test](#-self-test)

---

## String Methods

**Code**
```js
const str = "  Hello World  ";

console.log(str.length);                 // 16 -> includes the spaces

console.log(str.toUpperCase());            // "  HELLO WORLD  "
console.log(str.toLowerCase());             // "  hello world  "
console.log(str.trim());                     // "Hello World" -> removes leading/trailing whitespace

console.log(str.includes("World"));            // true  -> contains?
console.log(str.trim().startsWith("Hello"));     // true  -> begins with?
console.log(str.trim().endsWith("World"));        // true  -> ends with?
console.log(str.indexOf("World"));                  // 8 -> position, -1 if not found

console.log("JavaScript".slice(0, 4));                 // "Java"  -> extract by index, supports negative
console.log("JavaScript".slice(-6));                     // "Script" -> from the end
console.log("JavaScript".substring(0, 4));                 // "Java"  -> similar, no negative support

console.log("Hello".replace("l", "L"));                       // "HeLlo" -> replaces FIRST match only
console.log("Hello".replaceAll("l", "L"));                       // "HeLLo" -> replaces ALL matches

console.log("a,b,c".split(","));                                    // ["a","b","c"]
console.log("Hello".split(""));                                       // ["H","e","l","l","o"]

console.log("Hello".charAt(1));                                          // "e"
console.log("Hello"[1]);                                                   // "e" -> bracket access also works

console.log("Hello".concat(" ", "World"));                                  // "Hello World"

console.log("5".padStart(3, "0"));                                            // "005"
console.log("5".padEnd(3, "0"));                                                // "500"
```

**Explain**
- Strings are **immutable primitives** — `str.toUpperCase()` returns a new string; `str` itself never changes.
- `padStart`/`padEnd` are commonly used for formatting: zero-padding numbers, aligning table output.
- `replace` only touches the first occurrence unless you pass a **global regex** (`/l/g`) — `replaceAll` was added specifically to skip that regex requirement for simple strings.

---

## `slice` vs `substring` — the gotcha

| | `slice(start, end)` | `substring(start, end)` |
|---|---|---|
| Negative indices | ✅ counts from end | ❌ treated as `0` |
| `start > end` | returns `""` | ❌ auto-swaps the two args |

```js
console.log("Hello".slice(-3));         // "llo"
console.log("Hello".substring(-3));      // "Hello" -> negative treated as 0

console.log("Hello".slice(3, 1));         // "" -> start after end = empty
console.log("Hello".substring(3, 1));      // "el" -> silently swaps to substring(1,3)
```

**Explain**
Default to `slice` — it behaves predictably and works identically on arrays too, so you only need to remember one mental model.

---

## 🔧 Practice Problems

### 1. Reverse a String
```js
function reverseString(str) {
  return str.split("").reverse().join("");
}
console.log(reverseString("hello")); // "olleh"

// Without built-ins, for interview follow-up
function reverseManual(str) {
  let result = "";
  for (let i = str.length - 1; i >= 0; i--) {
    result += str[i];
  }
  return result;
}
console.log(reverseManual("hello")); // "olleh"
```

### 2. Count Characters
```js
function countChars(str) {
  const counts = {};
  for (const char of str) {
    counts[char] = (counts[char] || 0) + 1;
  }
  return counts;
}
console.log(countChars("hello"));
// { h: 1, e: 1, l: 2, o: 1 }
```

### 3. Remove Duplicate Characters
```js
function removeDuplicates(str) {
  return [...new Set(str)].join(""); // Set keeps only unique values, insertion order preserved
}
console.log(removeDuplicates("programming")); // "progamin"

// Without Set
function removeDuplicatesManual(str) {
  let result = "";
  for (const char of str) {
    if (!result.includes(char)) result += char;
  }
  return result;
}
console.log(removeDuplicatesManual("programming")); // "progamin"
```

### 4. Check Palindrome
```js
function isPalindrome(str) {
  const clean = str.toLowerCase().replace(/[^a-z0-9]/g, ""); // strip spaces/punctuation
  return clean === clean.split("").reverse().join("");
}
console.log(isPalindrome("Madam"));                 // true
console.log(isPalindrome("A man a plan a canal Panama")); // true
console.log(isPalindrome("Hello"));                    // false

// Two-pointer version — O(1) space, no extra string built
function isPalindromeTwoPointer(str) {
  const clean = str.toLowerCase().replace(/[^a-z0-9]/g, "");
  let left = 0, right = clean.length - 1;
  while (left < right) {
    if (clean[left] !== clean[right]) return false;
    left++;
    right--;
  }
  return true;
}
```

### 5. Find Most Frequent Character
```js
function mostFrequentChar(str) {
  const counts = {};
  for (const char of str) {
    counts[char] = (counts[char] || 0) + 1;
  }

  let maxChar = "";
  let maxCount = 0;
  for (const [char, count] of Object.entries(counts)) {
    if (count > maxCount) {
      maxChar = char;
      maxCount = count;
    }
  }
  return { char: maxChar, count: maxCount };
}
console.log(mostFrequentChar("javascript"));
// { char: "a", count: 2 }
```

**Explain**
- Problems 2, 3, 5 all lean on the same pattern: **build a frequency map**, then reason over it. This single pattern (hash map counting) solves the vast majority of string/array interview questions.
- `isPalindrome` strips non-alphanumeric characters first — always ask the interviewer whether spaces/punctuation/case should count.

---

## 🧪 Self-Test

```js
console.log("  JavaScript  ".trim().length);
console.log("hello world".split(" ").map(w => w[0].toUpperCase() + w.slice(1)).join(" "));
console.log("aabbccdd".split("").filter((c, i, arr) => arr.indexOf(c) === i).join(""));
console.log(reverseString("racecar") === "racecar");
console.log("7".padStart(4, "0"));
```

<details>
<summary>Show answers</summary>

```
10             // "JavaScript".length

"Hello World"   // capitalize each word

"abcd"           // dedupe via indexOf === i trick

true              // racecar reversed is itself

"0007"
```
</details>

---

<p align="center"><i>Next → <code>08-async-js.md</code></i></p>
