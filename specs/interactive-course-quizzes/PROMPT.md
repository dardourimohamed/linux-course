# Objective

Add interactive quiz questions to all 13 chapters of the Linux mastery course (src/ directory) using the mdbook-quiz preprocessor. Quizzes should be placed at the END of each chapter in a dedicated "## Chapter Quiz" section, before the existing "## Exercises" section.

# Key Requirements

1. **Plugin Setup**
   - Install `mdbook-quiz` via cargo
   - Configure in book.toml: `[preprocessor.quiz]`

2. **Quiz Format (per question)**
   ```
   {{#quiz}}
   Question text here?

   A) Option one
   B) Option two
   C) Option three
   D) Option four

   {{#explain}}
   Explanation that teaches why the answer is correct...
   {{/explain}}
   {{/quiz}}
   ```

3. **Placement**
   - Each chapter gets a "## Chapter Quiz" section
   - Place BEFORE the existing "## Exercises" section
   - After the "## Summary" section

4. **Question Guidelines**
   - 5-10 questions per chapter (7-10 for CLI-heavy chapters)
   - Multiple choice with 4 options (A, B, C, D)
   - Style: playful and challenging
   - Include explanations that teach, not just state the answer
   - Cover all major topics from the chapter

5. **Scope: All 13 chapters**
   - Part I: Foundations (chapters 1-3)
   - Part II: CLI Mastery (chapters 4-7)
   - Part III: System Administration (chapters 8-11)
   - Part IV: DevOps (chapters 12-13)

# Acceptance Criteria

- [ ] All 13 chapters have a "## Chapter Quiz" section
- [ ] Quizzes appear before "## Exercises" sections
- [ ] Each chapter has 5-10 quiz questions
- [ ] All questions use valid {{#quiz}} syntax
- [ ] `mdbook build` completes without errors
- [ ] Built book shows interactive quiz elements
- [ ] Questions are challenging and include explanations

# Reference

See `specs/interactive-course-quizzes/` for detailed design and research:
- `design.md` - Full architecture and technical details
- `plan.md` - 10-step implementation guide
- `research/` - Technology choices and best practices
