# Introduction to Alteryx

[Home](../../README.md) > [Getting Started](./README.md) > Introduction

---

## What is Alteryx?

Alteryx is a powerful data analytics platform that enables users to prepare, blend, and analyze data without writing code. It uses a visual, drag-and-drop interface where you connect "tools" to create "workflows" that process your data.

Think of it as building a data pipeline visually - you can see exactly how your data flows and transforms from start to finish.

## Why Use Alteryx?

### Speed
Tasks that take hours in Excel or require complex SQL queries can be done in minutes with Alteryx. Once you build a workflow, it can be run repeatedly with a single click.

### No Coding Required
While Alteryx supports Python, R, and SQL for advanced users, the vast majority of tasks can be accomplished through the visual interface alone.

### Repeatability
Save your workflows and run them whenever you need. Update the input data, click Run, and get updated results instantly.

### Transparency
Every step of your data transformation is visible on the canvas. Anyone can open your workflow and understand exactly what's happening to the data.

### Scalability
Handle datasets with millions of rows that would crash Excel. Connect directly to databases, cloud storage, and APIs.

## Who Uses Alteryx?

- **Data Analysts** - Automate repetitive reporting tasks
- **Business Intelligence Teams** - Prepare data for visualization tools
- **Finance Professionals** - Consolidate and reconcile financial data
- **Marketing Analysts** - Blend customer data from multiple sources
- **Operations Teams** - Process and validate operational data
- **Anyone working with data** - No programming background needed

## Core Concepts

### Workflows
A workflow is your data process - a series of connected tools that read, transform, and output data. Workflows are saved as `.yxmd` files.

```
[Input Data] → [Select] → [Filter] → [Formula] → [Output Data]
```

### Tools
Tools are the building blocks of workflows. Each tool performs a specific function:
- **Input Data** - Reads files or database tables
- **Filter** - Keeps rows matching criteria
- **Join** - Combines data from two sources
- **Formula** - Creates calculated fields
- **Output Data** - Writes results to files

### Canvas
The canvas is your workspace - the visual area where you drag tools and connect them to build workflows.

### Configuration Window
When you click a tool, the Configuration window shows its settings. This is where you specify what the tool should do.

### Results Window
After running a workflow, the Results window shows the output data at any point in your workflow. Click any tool to see its output.

## What Can You Do with Alteryx?

### Data Preparation
- Clean messy data (fix dates, standardize text, handle nulls)
- Remove duplicates
- Filter and sort records
- Split or combine columns

### Data Blending
- Join data from multiple files
- Append datasets together
- Look up values from reference tables
- Merge data from different systems

### Data Analysis
- Calculate summaries and aggregations
- Create running totals and rankings
- Perform statistical analysis
- Build predictive models

### Automation
- Schedule workflows to run automatically
- Process files from folders
- Send email notifications
- Update databases and reports

### Reporting
- Generate formatted reports
- Create visualizations
- Export to Excel, PDF, or other formats
- Build interactive apps

## Alteryx Products

### Alteryx Designer
The desktop application where you build workflows. This is what most users work with daily.

### Alteryx Server
Enterprise solution for scheduling workflows, sharing with teams, and governance.

### Alteryx Connect
Data catalog for discovering and understanding available data sources.

### Alteryx Auto Insights
Automated analytics and insights generation.

### Alteryx Machine Learning
No-code machine learning model building.

---

## Getting Started Checklist

- [ ] [Install Alteryx Designer](02-Installation.md)
- [ ] [Learn the interface](03-Interface-Overview.md)
- [ ] [Build your first workflow](04-First-Workflow.md)
- [ ] [Understand data types](05-Data-Types.md)

---

## Key Takeaways

1. **Alteryx is visual** - Build data processes by connecting tools on a canvas
2. **No coding required** - Most tasks need zero programming knowledge
3. **Workflows are reusable** - Build once, run many times
4. **Everything is transparent** - See exactly how data transforms
5. **It scales** - Handle large datasets that overwhelm Excel

---

[Next: Installation & Setup →](02-Installation.md)
