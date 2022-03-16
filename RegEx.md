# **Regular Expressions**

## **syntax: /RegEx/flags**

- ( // ) - RegEx literal

- RegEx - search pattern ( literal characters, meta characters )

- Literal Characters - all alphanumeric characters.

- Metacharacters - ( `. ? ! ^ $ * + - = \ | : {} [] ()` )

- Flags - `g, i, m, u, y, s`
  - `g` - global
  - `i` - case insensitive
  - `m` - multiline
  - `s` - single line (default)
  - `u` - ungreedy
  - `y` - extended

---

## **Characters**

### **Wildcard Metacharacter ( `.` )**

---

`/./` - matches any character except line brakes.

Text: `"9.00, 9500, 9-00, 9110"`

```text
/9.00/g
```

Match: `9.00 9500 9-00`

---

### **Escaping Metacharacter ( `\` )**

---

- Only escape metacharacter

- `/\./` - literal period not wildcard metacharacter "`.`"

- `/\\/g` - backslash literal character

Text: `"9.00 9500 9-00"`

```text
/9\.00/g
```

Match: `9.00`

Text: `"/etc/"`

```text
/\/etc\//
```

Match: `/etc/`

---

### **Example**

Text: `"resume1.txt, resume2.txt, resume3_txt.zip"`

```text
/resume.\.txt/g
```

Match: `resume1.txt resume2.txt`

---

### **|| Space || Tab (`\t`) || Line return (`\r, \n, \r\n`) ||**

---

```text
/" "/
```

Match: `" "`

```text
/.\t.\t./
```

Match: `1<tab>2<tab>3`

```text
/c\nd/
```

- `\r` (Carriage Return), `\n` (Line Feed)

Match: `c<CRLF>d`

---

### **Character set**

- Character set metacharacter ( `[]` )

- `/[aeiou]/` - matches any one vowel `"a", "e", "i", "o", "u"`

- `/gr[ea]y/` - matches `grey gray`, but not `"greay"`

---

### **Character Ranges**

---

- Character Ranges metacharacter ( `-` )

- Character Range metacharacter outside a character set ( `[]` ) act as a literal character

- `[0-9]` - 0 to 9, `[a-z]` - a to z, `[A-Z]` - A to Z

- `[60-99]` - 6, [0-9], 9 (i.e. from 0 to 9)

Text: `555-666-7890`

```text
/[0-9][0-9][0-9]-[0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]/g
```

Match: `555-666-7890`

---

### **Negative Character Set**

- Metacharacter ( `^` ) - Negates a entire character set

- Not any one of several characters

- Add `^` as the first character inside a character set

- `/^aeiou/g` matches any one consonant (non-vowel)

- `/see[^mn]/g` matches `"seek"`, `"see "` and `"sees"` but not `"seem"`, `"seen"` or `"see"`

Text: `"It seems I see the sea I seek"`

```text
/see[^mn]/g
```

Match: `"see "` `"seek"`

---

### **Metacharacters inside a Character sets**

- Most metacharacters inside a character set are already escaped

- `/h[a.]t/` matches `"hat"`, `"h.t"` but not `"hot"`

- `13[-/]03[-/]2020` matches `"13-03-2020, 13/03/2020/"`

- **Exceptions :** ( `] ^ - \` )

Text: `"index[0], index(3), index(8]"`

```text
/index[[(][0-9][\])]/g
```

Match: `"index[0]"` `"index(3)"` `"index(8]"`

Text: `"file01 file-1 file\1 file_1"`

```text
/file[0_\-\\][0-9]/g
```

Match: `"file01"` `"file-1"` `"file\1"` `"file_1"`

---

### **Shorthand Character Sets**

| Shorthand | Meaning            | Equivalent      |
| --------- | ------------------ | --------------- |
| `\d`      | Digit              | `[0-9]`         |
| `\w`      | Word Character     | `[a-zA-Z0-9_]`  |
| `\s`      | Whitespace         | `[\t\n\r ]`     |
| `\D`      | Not Digit          | `[^0-9]`        |
| `\W`      | Not Word Character | `[^a-zA-Z0-9_]` |
| `\S`      | Not Whitespace     | `[^\t\n\r ]`    |

`/\d\d\d\d/` matches `"1999"`, but not `"text"`

`/\w\w\w/` matches `"ABC"`, `"aBc"`, `"12a"`, and `"a__"`, but not `"a-a"`

`/\w\s\w\w/` matches `"I am"`, but not `"Am I"`

`/\D/` same as both `/[^0-9]/` `/[^\d]/`

### **Caution**

- **`\w` - `[a-zA-Z0-9_]`**

  - Underscore is a word character

  - Hyphen is not a word character

- **`/[^\d\s]/`** is not same as **`/[\D\S]/`**

  - `/[^\d\s]/` = Not Digit or Space Character

  - `/[\D\S]/` = Not Digit or Not Space Character

`/[\w\-]/` matches any word character or hyphen (useful)

---

### **Repetition Metacharacter**

| Metacharacter | Meaning                            |
| ------------- | ---------------------------------- |
| `*`           | Preceding item, zero or more times |
| `+`           | Preceding item, one or more times  |
| `?`           | Preceding item, zero or one time   |

`/.+/` - Matches any string of characters except a line return

Text: `"Good Morning. Good Day. Good Evening. Good Night_"`

```text
/Good .+_/g
```

Match: `"Good Morning. Good Day. Good Evening. Good Night_"`

Text: `"apple apples applessss"`

```text
/apples?/g
```

Match: `"apple"` `"apples"` `"apples"`

```text
/apples*/g
```

Match: `"apple"` `"apples"` `"applessss"`

```text
/apples+/g
```

Match: `"apples"` `"applessss"`

---

### **Quantified Repetition**

| Metacharacter | Meaning                                       |
| ------------- | --------------------------------------------- |
| {             | Start quantified repetition of preceding item |
| }             | End quantified repetition of preceding item   |

### **Three Syntaxes**

1. **`{min, max}`**

   - `/\d{4, 8}/` - matches numbers with four to eight characters

2. **`{min}`**

   - `/\d{4}/` - matches numbers with exactly four digits (min is max)

3. **`{min,}`**
   - `/\d{4,}/` - matches numbers with four or more digits (infinite)

- `/\d{0,}/` - is same as `/\d*/`

- `/\d{1,}/` - is same as `/\d+/`

- `/\d{3}-\d{3}-\d{4}/` - matches `123-456-7890`

- `/A{1,2} bonds/` - matches `A bonds`, and `AA bonds`

Text: `"1. a 2. ab 3. abc 4. abcd 5. abcde 6. abcdef"`

```text
/\d. \w{3,5}/g
```

Match: `"3. abc "` `"4. abcd "` `"5. abcde "`

---

### Greedy Expressions

- Standard repetition quantifiers are greedy

- Expression tries to match the longest possible string

- Defers to achieving overall match

- `/.+\.jpg/` matches "filename.jpg"

- The `+` is greedy, but "gives back" the `.jpg` to make the match

- Gives back as little as possible

- `/.*[0-9]+/` matches "Page 266"

Text: `"asdf", "asdf", "asdf, asdf."`

```text
/".+", ".+"/g
```

1. `".+"` - `"asdf", "asdf",`
2. `".+"` - `"asdf, asdf."`

Match: `"asdf", "asdf", "asdf, asdf."`

---

### Lazy Expression

| Metacharacter | Meaning                        |
| ------------- | ------------------------------ |
| ?             | Make preceding quantifier lazy |

- Lazy Expression `( *?, +?, {min, max}?, ?? )`

- Instructs quantifier to use a "lazy strategy" for making choices

- Match as little as possible before giving control to the next expression part

Text: `"asdf", "asdf", "asdf, asdf."`

```text
/".+?", ".+?"/g
```

Match: `"asdf", "asdf"`

---

### Grouping Metacharacters

| Metacharacter | Meaning                |
| ------------- | ---------------------- |
| (             | Start Group Expression |
| )             | End Group Expression   |

- A list of three use cases for grouping metacharacters.

  1. Repetition Operators
  2. Alternation
  3. Capturing for replacing

- `/(abc)+/` matches `abc` and `abcabcabc`

- `/(in)?dependent/` matches `independent` and `dependent`

- `/run(s)?/` is same as `/runs?/`

### Capturing for replacing

- `(\d{3})-(\d{3}-\d{3})` matches `555-666-7890`

  - `(\d{3})` - $1
  - `(\d{3}-\d{3})` - $2
  - `($1) $2` - "`(555) 666-7890`"

- Non capturing group `(?:)` - `(?:.+)`

### Alternation Metacharacter ( `|` - OR operator)

- ( `|` ) matches previous or next expression
- Ordered, leftmost expression gets precedence
- `/apple|orange/g` matches `apple` and `orange`
- `/w(ie|ei)rd/g` matches `wierd` and `weird`
- `/abc|def|ghi/g` matches `abc`, `def` and `ghi`
- `/apple(juice|sauce)/g` is not the same as `/applejuice|sauce/g`
- `/(AA|BB|CC){4}/g` matches `AABBCCAA CCBBCCBB AABBCCCC`

Text: `AABB AACC BBCC ACCBBA CCCC`

```text
/(AA|BB|CC){2}/g
```

Match: `AABB AACC BBCC CCBB CCCC`

Text: `does not does no do not do no do nothing does nothing`

```text
/[Dd]o(es)? (nothing|not|no)/
```

Match: `"does not" "does no" "do not" "do no" "do nothing" "does nothing"`

- Order `(nothing|not|no)` matters
- `(no|not|nothing)` matches only `no`

---

### Anchors

Start and End Anchors

| Metacharacter | Meaning                            |
| ------------- | ---------------------------------- |
| ^             | Start of string/line               |
| $             | End of string/line                 |
| \A            | Start of string, never end of line |
| \Z            | End of string, never end of line   |

- Reference a position, not an actual character

- Zero Width

- `/^apple/` or `/\Aapple/` (apple must be the first word)

- `\A` and `\Z` are not yet supported by JavaScript

- `/^apple/` matches `apple` only if it's beginning of a line (multiline flag)

Text: `apple to apples to appless`

```text
/apple(ss|s)$/
```

Match: `appless`

Text: `apple to apples to appless`

```text
/^apple(ss|s)?/
```

Match: `apple`

### Line Breaks and Multiline mode

Text:

```text
Apples and pears.
oranges
bananas
mangoes
```

```text
/^[\w ,.]+$/g
```

Match:

```text
Apples and pears.
```

Text:

```text
Apples and pears.
oranges
bananas
mangoes
```

```text
/^[\w ,.]+$/gm
```

Match:

```text
Apples and pears.
oranges
bananas
mangoes
```

---

### Word Boundaries

| Metacharacter | Meaning                           |
| ------------- | --------------------------------- |
| \b            | Word Boundary (start/end of word) |
| \B            | Not a Word Boundary               |

- Reference a position, not an actual character

String: `This is a test`

- Before the first word character in the string ( `\bThis` )

- After the last word character in the string ( `test\b` )

- Word character `[ a-zA-Z0-9_ ]`

- Between a Word character and a Non Word character ( `This\b \bis\b \ba\b \btest` )

- `/\b\w+\b` matches four words in text `"This is a test."`

  - `\bThis\b`

  - `\bis\b`

  - `\ba\b`

  - `\btest\b`

- `/\bNew\bYork\b/` does not match `New York`

- `/\bNew\b \bYork\b/` matches `New York`

- `/\B\w+\B/` finds two matches in text `"This is a test."`

  - `hi`
  - `es`

- `/\b\w+s\b/` matches `apples` in `"We picked apples."`

---

### Look Arounds

---

### Capturing Group

- Capturing Group - `(...)` ( $1 ( \1 ) )
- Non Capturing Group - `(?:...)`
- Named Group - `(?<firstGroup>...)` ( $firstGroup ( \firstGroup ) )
- If conditionals - `(($1)|($2)) (?(2)...| else...)`
- Look Arounds
  - Positive lookahead: `(?=)`, Negative lookahead: `(?!)`
  - Positive lookbehind: `(?<=)`, Negative lookbehind: `(?<!)`

---

### Lookaheads

- Positive lookahead: `(?=)`, Negative lookahead: `(?!)`

- Match an expression in a group but do not capture it; works as a boundary

- Only Static lengths are supported in lookahead groups

Text: `"53222 22222-1234 23a23323 12345-1234 33333-1234"`

```text
/\d{5}(?=-\d{4})/g
```

Match: `2222` `12345` `33333`

Text: `"53222 22222-1234 23a23323 12345-1234"`

```text
/\d{5}(?!-\d{4})/g
```

Match: `53222` `23323`

---

### Lookbehinds

- Positive lookbehind: `(?<=)`, Negative lookbehind: `(?<!)`

- Match an expression in a group but do not capture it; works as a boundary

- Only Static lengths are supported in lookbehind groups

Text:

```text
<li><code>$1</code></li>
<li><code>$2</code></li>
<code>$3</code>
<code>$4</code>
```

```text
/(?<=<li><code>).+?(?=<\/code>)/g
```

Match: `$1` `$2`

```text
<li><code>$1</code></li>
<li><code>$2</code></li>
<code>$3</code>
<code>$4</code>
```

```text
/(?<!<li><code>)(?<=<code>).+?(?=<\/code>)/g
```

Match: `$3` `$4`

---

### **If conditionals**

- Named and Nested Conditionals

Text:

```text
----------
          ID: mod_gd
    Function: pkg.installed
        Name: php-gd
      Result: True
     Comment: All specified packages are already installed
     Started: 16:19:04.848951
    Duration: 14.303 ms
     Changes:
----------
          ID: mod_mbstring
    Function: pkg.installed
        Name: php-mbstring
      Result: True
     Comment: All specified packages are already installed
     Started: 16:19:04.863402
    Duration: 13.866 ms
     Changes:
----------
```

```text
/(ID|(?<fn>Function)|(?<dur>Duration)): (?(fn)\w+?\.\w+|(?(dur)\d+\.\d+ ms|\w+))/g
```

Match:

`ID: mod_gd`

`Function: pkg.installed`

`Duration: 14.303 ms`

`ID: mod_mbstring`

`Function: pkg.installed`

`Duration: 13.866 ms`
