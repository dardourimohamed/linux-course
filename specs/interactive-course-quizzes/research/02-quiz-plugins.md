# Research: mdBook Quiz Plugins

## Primary Option: mdbook-quiz

**Repository:** [cognitive-engineering-lab/mdbook-quiz](https://github.com/cognitive-engineering-lab/mdbook-quiz)
**Demo:** [cel.cs.brown.edu/mdbook-quiz](https://cel.cs.brown.edu/mdbook-quiz/)

### What It Does

An mdBook preprocessor that adds interactive quizzes with:
- Multiple question types (multiple choice, short answer)
- HTML elements with quiz schema metadata
- Validation features
- Interactive loading when page loads

### Configuration

Add to `book.toml`:
```toml
[preprocessor.quiz]
validate = true  # Enable validation features
```

### Installation

```bash
cargo install mdbook-quiz
```

## Secondary Option: mdbook-exercises

**Repository:** [guyernest/mdbook-exercises](https://github.com/guyernest/mdbook-exercises)
**Crates.io:** [mdbook-exercises](https://lib.rs/crates/mdbook-exercises)

### Features
- Interactive exercise blocks
- Hints and solutions
- Test execution
- Optional Rust Playground integration

### Use Case
Better suited for coding exercises with test validation rather than simple quiz questions.

## Comparison

| Feature | mdbook-quiz | mdbook-exercises |
|---------|-------------|------------------|
| Question Types | Multiple choice, short answer | Coding exercises |
| Validation | Built-in validation | Test-based |
| Complexity | Lower | Higher |
| Best For | Concept checks | Coding practice |

## Sources

- [mdbook-quiz GitHub](https://github.com/cognitive-engineering-lab/mdbook-quiz)
- [mdbook-quiz documentation](https://cel.cs.brown.edu/mdbook-quiz/)
- [mdbook-exercises GitHub](https://github.com/guyernest/mdbook-exercises)
- [mdbooks.code-maven.com/preprocessor-quiz](https://mdbooks.code-maven.com/preprocessor-quiz)
