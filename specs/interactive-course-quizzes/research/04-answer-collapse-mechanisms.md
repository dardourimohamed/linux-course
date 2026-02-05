# Research: Answer Collapse Mechanisms

## Option 1: Native HTML5 `<details>`/`<summary>`

### Syntax

```html
<details>
<summary>Show Answer</summary>

The answer content goes here.

</details>
```

### Pros
- **Native browser support** - No JavaScript required
- **Accessible** - Works with screen readers
- **Zero dependencies** - No plugins or preprocessors
- **Fast** - Instant rendering

### Cons
- Basic styling (can be enhanced with CSS)
- No analytics/tracking built-in

### Styling Options

Can be styled with custom CSS in `book.toml`:
```toml
[output.html]
additional-css = ["custom.css"]
```

## Option 2: JavaScript-based Reveal

### Example Pattern

```html
<div class="quiz">
  <p>Question here?</p>
  <button onclick="toggleAnswer('q1')">Show Answer</button>
  <div id="q1" style="display:none">Answer here</div>
</div>
```

### Pros
- Full control over appearance
- Can add animations
- Can track clicks for analytics

### Cons
- Requires JavaScript
- More complex to maintain
- Accessibility concerns if not implemented carefully

## Option 3: mdbook-quiz Plugin

### Features
- Schema-based quiz format
- Built-in validation
- Multiple question types
- Interactive loading

### Use Case
Best when you need validation, scoring, or more complex interactivity

## Recommendation

For this use case (Linux course with collapsed answers), the **native HTML5 `<details>`/`<summary>` approach** is recommended because:

1. Simplicity - No setup required
2. Works immediately in mdBook
3. Fully accessible
4. Can be styled with CSS if needed
5. Readers can reveal at their own pace

## Sources

- [Organizing information with collapsed sections (GitHub)](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/organizing-information-with-collapsed-sections)
- [HTML5 <details> in GitHub (gist)](https://gist.github.com/ericclemmons/b146fe5da72ca1f706b2ef72a20ac39d)
- [mdBook-specific features](https://rust-lang.github.io/mdBook/format/mdbook.html)
