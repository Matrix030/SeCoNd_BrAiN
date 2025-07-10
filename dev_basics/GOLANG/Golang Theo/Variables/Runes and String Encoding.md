In many programming languages (cough, C, cough), a "character" is a single byte. Using [ASCII](https://www.asciitable.com/) encoding, the standard for the C programming language, we can represent 128 characters with 7 bits. This is enough for the English alphabet, numbers, and some special characters.

In Go, strings are just sequences of bytes: they can hold arbitrary data. However, Go also has a special type, [`rune`](https://go.dev/blog/strings), which is an alias for `int32`. This means that a `rune` is a 32-bit integer, which is large enough to hold any [Unicode](https://home.unicode.org/) code point.

When you're working with strings, you need to be aware of the encoding (bytes -> representation). Go uses [UTF-8](https://en.wikipedia.org/wiki/UTF-8) encoding, which is a variable-length encoding.

## What Does This Mean?

There are 2 main takeaways:

1. When you need to work with individual characters in a string, you should use the `rune` type. It breaks strings up into their individual characters, which can be more than one byte long.
2. We can include a wide variety of Unicode characters in our strings, such as emojis and Chinese characters, and Go will handle them just fine.
# Easier explaination
### 🧠 What’s the big idea?

#### In short:

- A **string** in Go is a sequence of **bytes**, not necessarily characters.
- A **rune** in Go is a fancy word for a single **Unicode character** — even if that character takes up **multiple bytes**.
### 🤖 What does Go use for text?

Go uses **UTF-8**, which is a way to store all kinds of characters — from `A` to `🚀` — in a compact form. Some characters (like `A`) need **1 byte**, others (like `🐻`) need **4 bytes**.

### ⚠️ Problem: Not all characters are 1 byte

When you use regular indexing (like `s[0]`) on a string, you're getting **bytes**, not full characters.

If you want to safely handle things like emojis, Chinese characters, etc., use **runes**, not raw bytes.
```go
package main

import (
    "fmt"
    "unicode/utf8"
)

func main() {
    name := "🐻"

    fmt.Println("Bytes:", len(name))        // 4 bytes
    fmt.Println("Runes:", utf8.RuneCountInString(name))  // 1 rune (1 character)
}

```
```output
Bytes: 4
Runes: 1
```
So even though it looks like one emoji (🐻), it's **4 bytes** long under the hood — but Go can still treat it as **one character** using runes.

### ✅ Takeaways (TL;DR)

- Use `len(str)` → counts **bytes**.
- Use `utf8.RuneCountInString(str)` → counts **characters** (runes).
- Emojis and many non-English characters take more than 1 byte.
- Always use **runes** if you're dealing with individual characters — not `str[i]`.

Let me know if you want a visual or mental model for how UTF-8 encodes characters!

# what if i want to use slicing for this text then?: "this is a bear 🐻"
Great question! Slicing strings in Go can be **tricky** when you have emojis or non-ASCII characters — because they may take **more than 1 byte**.

---

### ❗ Problem:

If you do this:

```go
s := "this is a bear 🐻"
slice := s[:14]
fmt.Println(slice)
```

You might accidentally **cut a multi-byte character in half**, which leads to invalid or weird-looking output.

---

### ✅ The Safe Way: Convert to Runes First

To slice by characters (not bytes), **convert the string to a `[]rune` first**:

```go
s := "this is a bear 🐻"
runes := []rune(s)
slice := string(runes[:14])  // safely get the first 14 characters
fmt.Println(slice)
```

✅ Output:

```
this is a bear
```

🧠 Note: The emoji "🐻" is the 15th character here (even though it’s 4 bytes), so slicing the first 14 characters **excludes** it safely.

---

### 🧾 Summary:

| What you want                    | What to do                         |
| -------------------------------- | ---------------------------------- |
| Slice by bytes (fast, dangerous) | `s[:N]`                            |
| Slice by characters (safe)       | `[]rune(s)[:N]` then `string(...)` |

Let me know if you want a visual of how the bytes and characters look under the hood!