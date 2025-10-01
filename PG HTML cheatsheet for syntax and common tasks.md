---
tags: 
- HTML
- PG
MOC: Technology
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[_0006 Programming MOC|Back to index]]
## HTML cheatsheet for syntax and common tasks

While using [HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML) it can be very handy to have an easy way to remember how to use HTML tags properly and how to apply them. MDN provides you with extended [HTML reference documentation](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements) as well as a deep instructional [set of HTML guides](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Structuring_content). However, in many cases we just need some quick hints as we go. That's the whole purpose of the cheat sheet, to give you some quick accurate ready to use code snippets for common usages.

**Note:**HTML tags must be used for their semantic value, not their appearance. It's always possible to totally change the look and feel of a given tag using [CSS](https://developer.mozilla.org/en-US/docs/Glossary/CSS) so, when using HTML, take the time to focus on the meaning rather than the appearance.

An "element" is a single part of a webpage. Some elements are large and hold smaller elements like containers. Some elements are small and are "nested" inside larger ones. By default, "inline elements" appear next to one another in a webpage. They take up only as much width as they need in a page and fit together horizontally like words in a sentence or books shelved side-by-side in a row. All inline elements can be placed within the `<body>` element.

| Usage | Element | Example |
| --- | --- | --- |
| A link | [`<a>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/a) |  |
| An image | [`<img>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/img) |  |
| An inline container | [`<span>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/span) |  |
| Emphasize text | [`<em>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/em) |  |
| Italic text | [`<i>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/i) |  |
| Bold text | [`<b>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/b) |  |
| Important text | [`<strong>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/strong) |  |
| Highlight text | [`<mark>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/mark) |  |
| Strikethrough text | [`<s>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/s) |  |
| Subscript | [`<sub>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/sub) |  |
| Small text | [`<small>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/small) |  |
| Address | [`<address>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/address) |  |
| Textual citation | [`<cite>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/cite) |  |
| Superscript | [`<sup>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/sup) |  |
| Inline quotation | [`<q>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/q) |  |
| A line break | [`<br>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/br) |  |
| A possible line break | [`<wbr>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/wbr) |  |
| Date | [`<time>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/time) |  |
| Code format | [`<code>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/code) |  |
| Audio | [`<audio>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/audio) |  |
| Video | [`<video>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/video) |  |

"Block elements," on the other hand, take up the entire width of a webpage. They also take up a full line of a webpage; they do not fit together side-by-side. Instead, they stack like paragraphs in an essay or toy blocks in a tower.

**Note:**Because this cheat sheet is limited to a few elements representing specific structures or having special semantics, the [`div`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/div) element is intentionally not included — because the `div` element doesn't represent anything and doesn't have any special semantics.

| Usage | Element | Example |
| --- | --- | --- |
| A simple paragraph | [`<p>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/p) |  |
| An extended quotation | [`<blockquote>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/blockquote) |  |
| Additional information | [`<details>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/details) |  |
| An unordered list | [`<ul>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ul) |  |
| An ordered list | [`<ol>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/ol) |  |
| A definition list | [`<dl>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/dl) |  |
| A horizontal rule | [`<hr>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/hr) |  |
| Text Heading | [<h1>-<h6>](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/Heading_Elements) |  |