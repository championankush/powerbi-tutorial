# Contributing to PowerBI Guide 🤝

Thank you for your interest in contributing to the PowerBI Complete Tutorial Guide! This document provides guidelines and information for contributors.

## How to Contribute

### Types of Contributions

We welcome various types of contributions:

1. **Content Improvements**
   - Fix typos and grammatical errors
   - Improve explanations and examples
   - Add missing information
   - Update outdated content

2. **New Tutorials**
   - Create new tutorial topics
   - Add advanced techniques
   - Include real-world examples
   - Provide step-by-step guides

3. **Code Examples**
   - Add DAX formulas
   - Provide Power Query transformations
   - Include sample datasets
   - Create complete projects

4. **Documentation**
   - Improve README files
   - Add troubleshooting guides
   - Create best practices documents
   - Update resource links

## Getting Started

### Prerequisites
- Basic knowledge of PowerBI
- Familiarity with Markdown
- Understanding of DAX and Power Query

### Setup
1. Fork the repository
2. Clone your fork locally
3. Create a new branch for your changes
4. Make your changes
5. Test your content
6. Submit a pull request

## Content Guidelines

### Writing Style
- Use clear, concise language
- Include practical examples
- Provide step-by-step instructions
- Use consistent formatting

### Code Examples
- Use proper syntax highlighting
- Include comments for complex code
- Test all DAX formulas
- Provide sample data when needed

### File Organization
- Follow the existing folder structure
- Use descriptive file names
- Include proper metadata
- Link related content

## Markdown Guidelines

### Headers
```markdown
# Main Title
## Section Title
### Subsection Title
#### Sub-subsection Title
```

### Code Blocks
```dax
// DAX code example
Total Sales = SUM(Sales[Amount])
```

```m
// Power Query example
= Table.AddColumn(Source, "NewColumn", each [Column1] + [Column2])
```

### Tables
```markdown
| Column 1 | Column 2 | Column 3 |
|----------|----------|----------|
| Data 1   | Data 2   | Data 3   |
```

### Links
```markdown
[Link Text](url)
[Internal Link](../relative/path)
```

## DAX Code Standards

### Naming Conventions
- Use descriptive measure names
- Include units in names when relevant
- Use consistent capitalization
- Avoid abbreviations

### Formatting
```dax
// Good formatting
Total Sales Amount = 
VAR CurrentSales = SUM(Sales[Amount])
VAR PreviousSales = CALCULATE(
    SUM(Sales[Amount]),
    PREVIOUSMONTH(Sales[Date])
)
VAR Growth = CurrentSales - PreviousSales
RETURN
    Growth

// Avoid
TotalSales=SUM(Sales[Amount])
```

### Comments
- Add comments for complex logic
- Explain business rules
- Document assumptions
- Include examples

## Power Query Standards

### M Code Formatting
```m
// Good formatting
let
    Source = Excel.Workbook(File.Contents("data.xlsx")),
    RawData = Source{[Name="Sheet1"]}[Data],
    ChangedTypes = Table.TransformColumnTypes(
        RawData,
        {
            {"Date", type date},
            {"Amount", type number}
        }
    )
in
    ChangedTypes
```

### Query Naming
- Use descriptive query names
- Include data source in name
- Add version numbers if needed
- Use consistent naming patterns

## Project Structure

### Adding New Content
1. **Tutorials**: Add to `tutorials/` folder
2. **DAX Examples**: Add to `dax/` folder
3. **Power Query**: Add to `power-query/` folder
4. **Visualizations**: Add to `visualizations/` folder
5. **Projects**: Add to `projects/` folder
6. **Resources**: Add to `resources/` folder

### File Naming
- Use kebab-case for file names
- Include topic in filename
- Use descriptive names
- Add version numbers if needed

## Review Process

### Before Submitting
1. **Self-Review**: Check your changes
2. **Testing**: Verify all examples work
3. **Spelling**: Check for typos
4. **Links**: Verify all links work
5. **Formatting**: Ensure consistent formatting

### Pull Request Guidelines
1. **Clear Title**: Describe the change
2. **Detailed Description**: Explain what and why
3. **Related Issues**: Link to relevant issues
4. **Screenshots**: Include visuals when helpful
5. **Testing**: Describe how you tested

## Code of Conduct

### Our Standards
- Be respectful and inclusive
- Use welcoming and inclusive language
- Be collaborative and constructive
- Focus on what is best for the community

### Unacceptable Behavior
- Harassment or discrimination
- Trolling or insulting comments
- Publishing others' private information
- Other unethical or unprofessional conduct

## Getting Help

### Questions?
- Open an issue for questions
- Use the discussion forum
- Contact maintainers directly
- Check existing documentation

### Resources
- [PowerBI Documentation](https://docs.microsoft.com/en-us/power-bi/)
- [DAX Reference](https://docs.microsoft.com/en-us/dax/)
- [Power Query Documentation](https://docs.microsoft.com/en-us/power-query/)

## Recognition

### Contributors
- All contributors will be recognized
- Names added to contributors list
- Special recognition for major contributions
- Featured in release notes

### Types of Recognition
- **Content Creator**: New tutorials and guides
- **Code Contributor**: DAX and M code examples
- **Reviewer**: Content review and feedback
- **Maintainer**: Ongoing project maintenance

## License

By contributing, you agree that your contributions will be licensed under the same license as the project.

## Contact

- **Issues**: Use GitHub issues
- **Discussions**: Use GitHub discussions
- **Email**: [Your email]
- **Community**: [Community forum link]

---

**Thank you for contributing to the PowerBI community!** 🚀 