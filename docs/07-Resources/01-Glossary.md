# Glossary of Terms

[Home](../../README.md) > [Resources](./README.md) > Glossary

---

## A

### Alias
A reusable reference name for a data connection. Allows changing the actual connection in one place while maintaining the same reference throughout workflows.

### Anchor
The connection point on a tool. **Input anchors** receive data (left side), **output anchors** send data (right side).

### Analytic App
An Alteryx workflow with a user interface that allows users to input parameters before running. Saved as .yxwz files.

### Auto Field
A tool that automatically optimizes data types based on actual values, reducing memory usage.

---

## B

### Batch Macro
A macro that runs multiple times, once for each group of control parameter values.

### Blob
Binary Large Object. A data type for storing binary data like images or documents.

### Browse Tool
A tool that displays data in the Results window for inspection. Useful for debugging.

---

## C

### Canvas
The main workspace area where you build workflows by placing and connecting tools.

### Configuration Window
The panel that shows settings for the currently selected tool.

### Connection
The line linking two tools, representing data flow from output anchor to input anchor.

### Container
A grouping mechanism that visually organizes tools and can be enabled/disabled as a unit.

### Control Parameter
In batch macros, the input that defines what changes between iterations.

---

## D

### Data Cleansing Tool
A preparation tool that handles common cleaning tasks: trimming whitespace, handling nulls, standardizing case.

### Data Type
The kind of value a field can hold: String, Int32, Double, DateTime, Bool, etc.

### Designer
Alteryx Designer - the desktop application for building workflows.

### Download Tool
A tool that makes HTTP requests to web services and APIs.

---

## E

### Expression
A formula written in Alteryx's expression language, used in Formula, Filter, and other tools.

---

## F

### Field
A column in your data. Also called a "column" or "variable."

### Filter Tool
A preparation tool that splits data into True (matching) and False (non-matching) streams based on a condition.

### Fixed Decimal
A numeric data type with exact precision, ideal for currency and financial calculations.

### Formula Tool
A preparation tool for creating new fields and modifying existing fields using expressions.

---

## G

### Gallery
Alteryx Analytics Gallery - a web-based platform for sharing and running workflows.

### Group By
In Summarize and other tools, the fields used to partition data before aggregation.

---

## I

### In-Database (In-DB)
Processing mode where operations execute inside the database rather than in Alteryx.

### Inner Join
A join type that returns only rows with matching keys in both inputs.

### Input Anchor
The connection point where a tool receives data (typically on the left side of a tool).

### Input Data Tool
A tool for reading data from files, databases, and other sources.

### Interface Tools
Tools used to create user-configurable options in macros and analytic apps.

### Iterative Macro
A macro that runs repeatedly until a condition is met.

---

## J

### Join Tool
A tool that combines two data streams based on matching key fields, similar to SQL JOINs.

---

## L

### Left Outer Join
A join type that returns all rows from the left input, with matching data from the right (or nulls if no match).

---

## M

### Macro
A workflow packaged as a reusable tool. Types include Standard, Batch, and Iterative.

### Macro Input
A tool that defines an input connection point for a macro.

### Macro Output
A tool that defines an output connection point for a macro.

### Message Tool
A tool that outputs messages to the workflow log during execution.

### Metadata
Information about data structure: field names, types, and sizes.

### Multi-Row Formula
A formula tool that can reference values from previous or subsequent rows.

---

## N

### Null
The absence of a value. Different from empty string or zero.

---

## O

### ODBC
Open Database Connectivity - a standard for connecting to databases.

### Output Anchor
The connection point where a tool sends data (typically on the right side of a tool).

### Output Data Tool
A tool for writing data to files, databases, and other destinations.

---

## P

### Parse
To extract structured data from text, like splitting "John Smith" into first and last names.

### Predictive Tools
Tools for statistical analysis and machine learning, requiring the Predictive Analytics add-on.

---

## R

### Record
A row in your data.

### Regex (Regular Expression)
A pattern-matching syntax for text operations. Used in Formula, RegEx, and other tools.

### Results Window
The panel at the bottom of Designer that shows data and messages after running a workflow.

### Right Outer Join
A join type that returns all rows from the right input, with matching data from the left.

### Running Total
A tool that calculates cumulative values across rows.

---

## S

### Sample Tool
A tool that takes a subset of rows for testing or sampling purposes.

### Select Tool
A preparation tool for choosing, renaming, reordering, and retyping fields.

### Server
Alteryx Server - enterprise platform for scheduling, sharing, and governing workflows.

### Sort Tool
A tool that orders rows by one or more field values.

### Spatial Object
A data type representing geographic data like points, lines, or polygons.

### Standard Macro
A basic reusable workflow that runs once when called.

### Stream
Data flowing through a workflow connection. Sometimes called a "data stream."

### Summarize Tool
A tool for grouping and aggregating data (sum, count, average, etc.).

---

## T

### Test Tool
A tool for validating data against multiple conditions with custom error messages.

### Tool
A building block in Alteryx that performs a specific function. Drag tools to the canvas and connect them.

### Tool Palette
The panel on the left side of Designer containing all available tools, organized by category.

### Transpose
Converting columns to rows. The opposite of Cross Tab (pivot).

---

## U

### Union Tool
A tool that stacks multiple data streams vertically (combining rows).

### Unique Tool
A tool that removes duplicate rows based on specified key fields.

---

## V

### V_String / V_WString
Variable-length string types that allocate only the memory needed for actual values.

---

## W

### Wildcard
A pattern using * or ? to match multiple files (e.g., "*.csv" matches all CSV files).

### Workflow
A file containing connected tools that process data. Standard workflows are .yxmd files.

### WString
Wide String - a string type supporting Unicode characters for international text.

---

## Y

### YXDB
Alteryx's native file format. Efficient for storage and fast for reading/writing.

### YXMC
File extension for Alteryx macros.

### YXMD
File extension for standard Alteryx workflows.

### YXWZ
File extension for Alteryx Analytic Apps.

---

[← Resources](./README.md) | [Next: Keyboard Shortcuts →](02-Shortcuts.md)
