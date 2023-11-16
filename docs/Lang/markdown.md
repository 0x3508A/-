# Markdown Language

- Cheat Sheet: <https://markdown.land/markdown-cheat-sheet>
    - Detailed [Below](#markdown-cheatsheet).

## Tables

```markdown
| Item         | Price | # In stock |
| ------------ | :---: | ---------: |
| Juicy Apples | 1.99  |        739 |
| Bananas      | 1.89  |          6 |
```

The `:--:` allows for *centering* and `---:` allows for *right justify*.

## Reference type links

```markdown
[This website about Markdown][Case-insensitive reference link to Markdown.land]
[The Python.land tutorial with a numbered link][1]
Or leave it empty and use the link text itself, like this: [Python Land].

Text and other Markdown markup, to demonstrate that the reference links can be put anywhere.

Usually, they are placed at the bottom of a document. Because they are reference links, they
won't show up by themselves. So you won't see the links below.


  [Case-insensitive reference link to Markdown.land]: https://www.markdown.land
  [1]: https://python.land/python-tutorial
  [Python Land]: http://python.land
```

Notice the special space and blank lines above the footer section.
These are mandatory.

## Combine bold and italics

```markdown
This is how you create **_bold italics_** text.
This is how you create ___bold italics___ text.
This is how you create ***bold italics*** text.
```

## Strike through

```markdown
This is how you create ~~strikethrough~~ text.
```

## In-document links

To add in-document jump like links we can use the additional link tags.

```html
<a link="my-link"></a>
```

The same can be called in Markdown as

```markdown
[Link to My Content](#my-link)
```

This method is easier to use and helps to easily keep the things together on the same document.
Across document the standard linking works better.

#### **Note** Cross document links do not support special characters or links in Headings.

Be careful about this.

## Markdown Cheatsheet

Basic Markdown cheatsheet
<https://github.com/segersniels/cheat-sheet-mix/blob/master/markdown.md>

### Headers

```markdown
# H1
## H2
### H3
#### H4
##### H5
###### H6

Alternatively, for H1 and H2, an underline-ish style:

Alt-H1
======

Alt-H2
------
```

### Emphasis

```markdown
Emphasis, aka italics, with *asterisks*.

Strong emphasis, aka bold, with **asterisks**.
```

### Lists

(In this example, leading and trailing spaces are shown with with dots: ⋅)

```markdown
1. First ordered list item
2. Another item
⋅⋅* Unordered sub-list.
1. Actual numbers don't matter, just that it's a number
⋅⋅1. Ordered sub-list
4. And another item.

⋅⋅⋅You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

⋅⋅⋅To have a line break without a paragraph, you will need to use two trailing spaces.⋅⋅
⋅⋅⋅Note that this line is separate, but within the same paragraph.⋅⋅

* Unordered list can use asterisks
- Or minuses
+ Or pluses
```

### Links

There are two ways to create links.

```markdown
[I'm an inline-style link](https://www.google.com)

[I'm an inline-style link with title](https://www.google.com "Google's Homepage")

[You can use numbers for reference-style link definitions][1]

URLs and URLs in angle brackets will automatically get turned into links in the preview.
http://www.example.com or <http://www.example.com>

[1]: https://www.qownnotes.org
```

### Inline code and code blocks

```markdown
Inline `code` has `back-ticks around` it.
```

Blocks of code are either fenced by lines with three back-ticks, or are indented with four spaces.

### 4-Spaces fence

```markdown
	s = "Code with space indent"
	print s
```

### Backtick fence

    ```
    Code goes here
    Code goes here
    ```

### Backtick fence with code highlighting

```bash
# I am a comment
cd Notes
```

### Tables

Tables aren't part of the core Markdown spec, but the QOwnNotes preview supports them.

```markdown
Colons can be used to align columns.

| Tables        |      Are      |  Cool |
| ------------- | :-----------: | ----: |
| col 3 is      | right-aligned | $1600 |
| col 2 is      |   centered    |   $12 |
| zebra stripes |   are neat    |    $1 |

There must be at least 3 dashes separating each header cell.
The outer pipes (|) are optional, and you don't need to make the
raw Markdown line up prettily. You can also use inline Markdown.

| Markdown | Less      | Pretty     |
| -------- | --------- | ---------- |
| *Still*  | `renders` | **nicely** |
| 1        | 2         | 3          |
```

### Blockquotes

```markdown
> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote.
```

### Horizontal Rule

```markdown
Three or more...

---

Hyphens

***

Asterisks

___

Underscores
```

### Line Breaks

```markdown
Here's a line for us to start with.

This line is separated from the one above by two newlines, so it will be a *separate paragraph*.

This line is also begins a separate paragraph, but...
This line is only separated by two trailing spaces and a single newline, so it's a separate line in the *same paragraph*.
```

### Comments

```markdown
[comment]: # (This comment will not appear in the preview)
```

[comment]: # (This comment will not appear in the preview)

They can also be specified in HTML:

```html
<!-- This is also a Markdown Comment that would not appear in preview -->
```

<!-- This is also a Markdown Comment that would not appear in preview -->

### Checkbox lists

```markdown
- [x] done
- [ ] todo
```

---

Credit: [markdown-here](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
License: [CC-BY](https://creativecommons.org/licenses/by/3.0/)