# Telegram-Flavored Markdown

TFM is an extended Markdown format for representing Telegram messages, inspired by [GitHub](https://github.github.com/gfm/)'s and [Notion](https://developers.notion.com/guides/data-apis/enhanced-markdown)'s Markdown formats.

This repository contains [TFM.md](./TFM.md) — format description (≈1000 tokens) to teach your LLM speak in TFM.

## FAQ

**Q: Telegram already has [MarkdownV2](https://core.telegram.org/bots/api#markdownv2-style), why not just use it?**

We believe MarkdownV2 parse mode supported by Telegram Bot API is poorly suited for LLMs for several reasons:

1. **Too Strict**: MarkdownV2 requires escaping almost every special character (`_`, `*`, `[`, `]`, etc.) if it isn't part of a formatting entity. LLMs frequently fail to escape characters consistently, causing the Telegram API to reject messages with `400 Bad Request` errors.

2. **Non-Standard**: MarkdownV2 differs from the common Markdown format that LLMs are overwhelmingly trained on:

   - Markdown: `**bold**`, `*italic*`, `~~strikethrough~~`
   - MarkdownV2: `*bold*`, `_italic_`, `~strikethrough~`

   This mismatch requires heavy system prompting and in LLMs not following the format correctly.

3. **Bizarre Syntax**: Advanced features like expandable block quotations use non-intuitive syntax (e.g., `**>The expandable block...||`) that combines empty bold entities and spoiler tags. LLMs struggle to replicate such syntax hacks consistently.
