# Building Macros

[Home](../../README.md) > [Advanced Topics](./README.md) > Macros

---

## What is a Macro?

A macro is a workflow packaged as a reusable tool. Build it once, use it anywhere - like creating your own custom Alteryx tool.

---

## Types of Macros

### Standard Macro
Runs once, like a regular workflow.
- Encapsulates common operations
- Accepts input, produces output
- Most common type

### Batch Macro
Runs multiple times with different parameters.
- Processes groups of data
- Iterates through control values
- Good for repeating operations

### Iterative Macro
Runs repeatedly until a condition is met.
- Loops until criteria satisfied
- Used for convergence problems
- Recursive-like operations

### Location Optimizer Macro
Specialized for spatial analysis.
- Finds optimal locations
- Used with Trade Area tools

---

## Creating a Standard Macro

### Step 1: Build the Workflow
Create the logic you want to reuse:
```
[Input]──[Select]──[Filter]──[Formula]──[Output]
```

### Step 2: Add Macro Input/Output
Replace Input Data with **Macro Input** tool:
1. Delete Input Data
2. Add Macro Input from Interface category
3. Connect to your workflow

Replace output with **Macro Output** tool:
1. Delete Output Data
2. Add Macro Output
3. Connect from your workflow

### Step 3: Save as Macro
1. **File** > **Save As**
2. Change file type to **Macro (.yxmc)**
3. Save to accessible location

### Step 4: Use the Macro
1. Right-click canvas > **Insert** > **Macro**
2. Browse to your .yxmc file
3. Use like any other tool

---

## Macro Interface Tools

### Macro Input
Brings data into the macro:
```
Configuration:
- Input Name: Data
- Anchor Abbreviation: I
```

### Macro Output
Sends data out of the macro:
```
Configuration:
- Output Name: Results
- Anchor Abbreviation: O
```

### Interface Tools
Add user-configurable options:

| Tool | Purpose |
|------|---------|
| **Text Box** | User text input |
| **Drop Down** | Selection from list |
| **Check Box** | True/False option |
| **File Browse** | File selection |
| **Numeric Up Down** | Number input |
| **List Box** | Multiple selection |

---

## Adding User Configuration

### Example: Configurable Filter Macro

```
1. Macro Input → brings in data
2. Filter tool → filter condition
3. Interface: Drop Down → user selects column
4. Interface: Text Box → user enters value
5. Action tool → connects interface to Filter
6. Macro Output → sends filtered data
```

### Action Tool
Connects interface inputs to tool configurations:
```
Interface Tool → Action → Target Tool
(User input)     (Maps)   (Updates config)
```

Action Types:
- Update Value
- Update Select
- Update String
- Update Connection

---

## Batch Macros

### Purpose
Run the same process for different groups or parameters.

### Structure
```
[Control Parameter]──┐
                     ├──[Your Logic]──[Macro Output]
[Macro Input]────────┘
```

### Control Parameter
Defines what changes each iteration:
```
Example: Run report for each region
Control Parameter: Region
Values: North, South, East, West
```

### Creating Batch Macro
1. Add Control Parameter tool
2. Add Macro Input for data
3. Build processing logic
4. Add Macro Output
5. Save as .yxmc

### Using Batch Macro
The batch macro appears with two inputs:
- Control input (parameters)
- Data input

---

## Iterative Macros

### Purpose
Repeat until a condition is met.

### Structure
```
[Macro Input]──[Process]──[Check Condition]──[Macro Output]
                    ↑              │
                    └──────────────┘ (loop back if not done)
```

### Key Components
1. **Macro Input**: Starting data
2. **Iteration Output**: Data to pass to next iteration
3. **Condition**: When to stop
4. **Macro Output**: Final results

### Example: Newton-Raphson
Calculate until difference < 0.001:
```
Iteration 1: Estimate = 5
Iteration 2: Estimate = 4.5
Iteration 3: Estimate = 4.472
(Stop when change < threshold)
```

---

## Macro Best Practices

### 1. Document Your Macro
Add comments and help text:
- What does it do?
- What inputs are expected?
- What outputs are produced?

### 2. Handle Edge Cases
- Empty inputs
- Null values
- Unexpected data types

### 3. Use Meaningful Names
For inputs, outputs, and interfaces:
```
Good: "Customer Data", "Filter Column", "Output Results"
Bad: "Input1", "Interface 1", "Output"
```

### 4. Test Thoroughly
Test with various inputs:
- Normal data
- Edge cases
- Error conditions

### 5. Version Control
Save versions as you develop:
```
MyMacro_v1.yxmc
MyMacro_v2.yxmc
MyMacro_v2.1.yxmc
```

---

## Macro Organization

### File Location
Store macros in:
- Project folder (for project-specific)
- Shared network location (for team)
- Custom macro folder (for personal library)

### Adding to Tool Palette
1. **Options** > **User Settings** > **Edit User Settings**
2. **Macros** tab
3. Add folder path
4. Macros appear in tool palette

---

## Debugging Macros

### Test as Workflow
Before saving as macro:
1. Use Input Data instead of Macro Input
2. Use Browse/Output instead of Macro Output
3. Test with real data
4. Replace with macro tools when working

### Open as Workflow
Right-click macro in workflow:
- **Open Macro** - Edit the macro
- View intermediate data

### Debug Mode
Insert Browse tools in macro to see intermediate results.

---

## Common Patterns

### Reusable Data Cleaning
```
[Macro Input]
     ↓
[Trim whitespace]
[Standardize case]
[Remove special chars]
     ↓
[Macro Output]
```

### Configurable Aggregation
```
[Macro Input]──[Summarize]──[Macro Output]
                    ↑
         [Drop Down: Group Field]
         [Drop Down: Aggregation Type]
```

### API Call Wrapper
```
[Macro Input: Parameters]
     ↓
[Build URL]
[Download]
[Parse Response]
     ↓
[Macro Output: Data]
```

---

## Quick Reference

### Macro File Types
| Extension | Type |
|-----------|------|
| .yxmc | Standard Macro |
| .yxwz | Analytic App |

### Interface → Action Connections
| Interface | Common Actions |
|-----------|----------------|
| Text Box | Update Value, Update String |
| Drop Down | Update Value, Update Select |
| Check Box | Update Value |
| File Browse | Update Value |

---

[← Advanced Topics](./README.md) | [Next: API Integration →](02-API-Integration.md)
