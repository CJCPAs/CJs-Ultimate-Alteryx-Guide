# Contributing to CJ's Alteryx Guide

Thank you for considering contributing to this project! This is a community-driven, open source resource for learning Alteryx, and contributions are essential to keeping it accurate and useful.

---

## Important: Accuracy Standards

This guide aims to be helpful and accurate. Because this is a community-maintained resource covering technical software that is regularly updated, we ask all contributors to follow these accuracy standards:

### Before Contributing

1. **Verify your information** against official Alteryx documentation when possible
2. **Test code and formulas** to ensure they work as described
3. **Note version-specific behavior** if features differ between Alteryx versions
4. **Acknowledge limitations** — no guide can cover every edge case

### What to Avoid

- **Overclaiming** — Don't claim this guide covers "everything" or is "complete"
- **Unverified information** — If you're unsure, note it or ask for review
- **Outdated content** — Check that information reflects current Alteryx versions
- **Absolute statements** — Use "typically," "generally," or "in most cases" when appropriate

---

## How to Contribute

### Reporting Issues

Found an error, typo, or outdated information? Please open an issue:

1. Go to the Issues tab
2. Click "New Issue"
3. Describe the problem clearly
4. Include the file path if applicable
5. Suggest a fix if you have one
6. **Cite sources** if you're reporting inaccurate information

### Suggesting Improvements

Have ideas for new content or improvements?

1. Open an issue with the "enhancement" label
2. Describe your suggestion
3. Explain why it would be valuable
4. Be specific about what should be added or changed

### Submitting Changes

Ready to contribute content directly?

1. Fork the repository
2. Create a new branch (`git checkout -b feature/your-feature-name`)
3. Make your changes
4. **Verify accuracy** of your changes
5. Commit with clear messages
6. Push to your fork
7. Open a Pull Request

---

## Content Guidelines

### Writing Style

- **Clear and concise** — Avoid jargon when possible
- **Practical** — Include real-world examples
- **Structured** — Use headers, lists, and tables
- **Consistent** — Follow existing formatting
- **Humble** — Acknowledge limitations and edge cases

### Accuracy Requirements

All contributions should:

- Be accurate and tested where applicable
- **Cite official Alteryx documentation** when describing features
- Note when behavior may vary by version or configuration
- Include examples that have been verified to work
- Not make claims of completeness ("covers everything")

### Formatting Standards

#### Headers
```markdown
# Main Title (H1 - one per file)
## Section (H2)
### Subsection (H3)
```

#### Code Blocks
Use fenced code blocks with language hints:
```markdown
```sql
SELECT * FROM table
```
```

#### Tables
Use markdown tables for structured information:
```markdown
| Column 1 | Column 2 |
|----------|----------|
| Data 1   | Data 2   |
```

#### Links
Use relative links for internal navigation:
```markdown
[Link Text](../path/to/file.md)
```

**Always link to official documentation** when referencing specific Alteryx features:
```markdown
For official documentation, see [Alteryx Help](https://help.alteryx.com/).
```

### File Organization

- Place files in appropriate directories
- Use numbered prefixes for ordering (01-, 02-, etc.)
- Keep filenames lowercase with hyphens
- Include README.md in each directory

---

## What We're Looking For

### High Priority
- **Error corrections** — Fix inaccuracies in existing content
- **Updated content** — Reflect changes in new Alteryx versions
- Real-world use case examples
- Workflow best practices
- Performance optimization tips
- Troubleshooting solutions

### Always Welcome
- Typo fixes
- Clarifications
- Additional examples
- New tool documentation
- Sample workflows
- Links to official documentation

### Before You Start
- Check existing issues to avoid duplicates
- For large changes, open an issue first to discuss
- Review existing content structure
- **Verify your information is accurate**

---

## Pull Request Process

1. **Title**: Use clear, descriptive titles
2. **Description**: Explain what and why
3. **Scope**: Keep changes focused
4. **Testing**: Verify accuracy
5. **Sources**: Cite official documentation where relevant
6. **Review**: Respond to feedback

### PR Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix / Error correction
- [ ] New content
- [ ] Improvement / Clarification
- [ ] Documentation update

## Accuracy Checklist
- [ ] I have verified this information is accurate
- [ ] I have tested any code/formulas included
- [ ] I have noted version-specific behavior if applicable
- [ ] I have linked to official documentation where relevant
- [ ] Content does not overclaim completeness

## Sources
List any official documentation or sources referenced
```

---

## Code of Conduct

### Be Respectful
- Treat all contributors with respect
- Welcome newcomers
- Provide constructive feedback

### Be Collaborative
- Work together toward quality content
- Share knowledge freely
- Credit others' contributions

### Be Accurate
- Prioritize correctness over quantity
- Welcome corrections to your own contributions
- Acknowledge when you're uncertain

### Be Professional
- Keep discussions on topic
- Avoid personal attacks
- Focus on improving the guide

---

## Recognition

Contributors will be:
- Listed in project acknowledgments
- Credited in commit history
- Thanked in release notes for significant contributions

---

## Questions?

- Open an issue with the "question" label
- Check existing issues for answers
- Review the documentation
- Consult [official Alteryx resources](https://help.alteryx.com/) for authoritative answers

---

## Disclaimer for Contributors

By contributing to this project, you acknowledge that:

1. Your contributions will be licensed under the MIT License
2. You have the right to submit the content you're contributing
3. You have made reasonable efforts to ensure accuracy
4. The maintainers may edit contributions for accuracy, style, or clarity

---

**Together, we can make quality Alteryx learning resources accessible to everyone.**

*Thank you for helping improve this community resource!*
