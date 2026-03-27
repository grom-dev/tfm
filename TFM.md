# TFM — Telegram-Flavored Markdown

TFM is an extended Markdown format for Telegram messages. It produces rich text rendered inside Telegram messages.

## Escaping

Use backslashes to escape special characters outside of code blocks: `\` `*` `~` `` ` `` `$` `[` `]` `<` `>` `{` `}` `|` `^`

Do **not** escape characters inside code spans or code blocks. Their content is literal.

## Inline formatting

| Format        | Syntax                    | Notes                                        |
| ------------- | ------------------------- | -------------------------------------------- |
| Bold          | `**text**`                |                                              |
| Italic        | `_text_`                  |                                              |
| Strikethrough | `~~text~~`                |                                              |
| Underline     | `<u>text</u>`             |                                              |
| Spoiler       | `<spoiler>text</spoiler>` | Hidden until user taps                       |
| Inline code   | `` `code` ``              | Tapping copies text to clipboard             |
| Link          | `[label](URL)`            | Supports URLs, deep-links (`t.me/`, `tg://`) |
| Custom emoji  | `![alt](id)`              | See [Custom emoji](#custom-emoji)            |

## Blocks

### Lists

```
- item
- item

1. item
2. item
```

Lists cannot be nested.

### Code block

````
```language
code
```
````

`language` is optional. Content is literal — do not escape inside.

### Blockquote

```
> line one
> line two
```

The `<blockquote>` tag is equivalent and additionally supports the `expandable` attribute:

```
<blockquote expandable>
Long content collapsed by default and expanded on tap.
</blockquote>
```

Blockquotes cannot be nested inside other blockquotes.

### Custom emoji

Syntax looks like an image but is **not** an image — it inserts a Telegram custom emoji (static or animated):

```
![alt](id)
```

- `alt` — UTF-8 emoji shown as fallback (old clients, OS notifications)
- `id` — numeric custom emoji ID (e.g. `5368324170671202286`)

### Date and time

Renders as an interactive, localized and timezone-aware timestamp. Users can tap it to add a calendar event or reminder.

```
<time unix="TIMESTAMP" format="FORMAT">fallback text</time>
```

- `unix` — UNIX timestamp in seconds (required)
- `format` — optional string matching `r|w?[dD]?[tT]?`
- `fallback text` — shown where interactive time is unsupported (e.g. OS notifications)

Format control characters:

| Char | Output example       | Notes                        |
| ---- | -------------------- | ---------------------------- |
| `r`  | "in 2 hours"         | Relative. Cannot combine.    |
| `w`  | "Wednesday"          | Day of week                  |
| `d`  | "17.03.22"           | Short date                   |
| `D`  | "March 17, 2022"     | Long date                    |
| `t`  | "22:45"              | Short time                   |
| `T`  | "22:45:00"           | Long time                    |

If `format` is omitted or empty, `fallback text` is shown as plain text but remains tappable.

Valid format examples: `d`, `Dt`, `wDT`, `r`

## Nesting

- Bold, italic, underline, strikethrough, and spoiler may be freely nested within each other.
- Inline code and code blocks do not support inner formatting — content is literal.
- Links, custom emoji, and `<time>` cannot be nested inside one another.
- Blockquotes cannot be nested inside other blockquotes.

## Not supported

Do **not** use these — they have no equivalent in Telegram messages:

- Headings (`#`, `##`, etc.)
- Images (`![alt](url)` with a URL — see [Custom emoji](#custom-emoji) for the similar but distinct syntax)
- Tables
- Task lists, footnotes, highlights

## Complete example

```
**Project update** — here's what's happening:

- Shipped the _new parser_ ~~last week~~ yesterday
- Docs are at [tfm.dev](https://tfm.dev)
- PIN: `AX-7743` (tap to copy)

<blockquote expandable>
**Full changelog:**

Added support for <u>underline</u> and <spoiler>spoiler</spoiler> text.

Fixed escaping of `\*` and `\_` outside code blocks.
</blockquote>

Next sync: <time unix="1711540800" format="wDt">Wed, Mar 27</time>

Reactions welcome: ![❤️](5368324170671202286)
```
