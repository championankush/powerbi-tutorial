# Custom Visuals Development Guide 🛠️

## Introduction

Custom visuals allow you to extend PowerBI with unique, interactive visualizations. This guide covers the development process, tools, and best practices.

## 1. PowerBI Custom Visuals Overview
- Custom visuals are JavaScript-based visualizations
- Use PowerBI Visuals API and developer tools
- Publish to PowerBI marketplace or use privately

## 2. Development Tools
- **Node.js** and **npm**
- **PowerBI Visuals Tools (pbiviz)**: `npm install -g powerbi-visuals-tools`
- **TypeScript** for development
- **Visual Studio Code** or similar editor

## 3. Creating a Custom Visual
1. Install pbiviz: `npm install -g powerbi-visuals-tools`
2. Create project: `pbiviz new MyCustomVisual`
3. Develop visual in TypeScript/JavaScript
4. Test locally: `pbiviz start`
5. Package: `pbiviz package`
6. Import `.pbiviz` file into PowerBI Desktop

## 4. PowerBI Visuals API
- Access data, settings, and formatting
- Handle user interactions (selection, filtering)
- Use capabilities.json for data roles and properties

## 5. Best Practices
- Follow accessibility guidelines
- Optimize for performance
- Document code and usage
- Test on different screen sizes
- Use version control (Git)

## 6. Publishing
- Submit to PowerBI marketplace (optional)
- Use privately by sharing `.pbiviz` file

## Resources
- [PowerBI Custom Visuals Docs](https://docs.microsoft.com/en-us/power-bi/developer/visuals/)
- [PowerBI Visuals Tools](https://github.com/microsoft/PowerBI-visuals-tools)
- [Custom Visuals Marketplace](https://appsource.microsoft.com/en-us/marketplace/apps?product=power-bi-visuals)

---

**Create unique, interactive PowerBI visuals for your organization or the world!** 🛠️ 