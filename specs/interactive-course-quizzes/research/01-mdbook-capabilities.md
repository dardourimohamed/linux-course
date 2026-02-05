# Research: mdBook Capabilities for Collapsible Content

## Key Finding: Native HTML5 Support

mdBook natively supports HTML5 `<details>` and `<summary>` tags directly in Markdown files.

### Syntax Example

```markdown
<details>
<summary>Click to expand!</summary>

## Content inside
This content is hidden by default.

</details>
```

### Advantages of Native HTML5 Approach

- **No dependencies** - Works out of the box
- **Accessible** - Built-in browser support for screen readers
- **Simple** - No configuration needed
- **Portable** - Works in any browser

## Alternative: mdbook-admonish

**Repository:** [tommilligan/mdbook-admonish](https://tommilligan.github.io/mdbook-admonish/)

A preprocessor that adds Material Design-style callout blocks with collapsible support.

### Features
- Styled blocks (info, warning, danger, example, etc.)
- Collapsible admonition bodies
- Configurable via `book.toml`

### Configuration
```toml
[preprocessor.admonish]
command = "mdbook-admonish"
```

## Sources

- [mdBook-specific features documentation](https://rust-lang.github.io/mdBook/format/mdbook.html)
- [Organizing information with collapsed sections (GitHub)](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/organizing-information-with-collapsed-sections)
- [mdbook-admonish documentation](https://tommilligan.github.io/mdbook-admonish/)
