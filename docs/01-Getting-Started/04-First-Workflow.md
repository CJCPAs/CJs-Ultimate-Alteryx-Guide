# Your First Workflow

[Home](../../README.md) > [Getting Started](./README.md) > First Workflow

---

## What We'll Build

In this tutorial, you'll build a complete workflow that:
1. Reads a CSV file
2. Filters the data
3. Adds a calculated column
4. Outputs the results

This covers the fundamentals you'll use in every Alteryx workflow.

---

## Prerequisites

- Alteryx Designer installed and activated
- Sample data file (we'll create one, or use your own CSV)

---

## Step 0: Create Sample Data

If you don't have a data file, create a simple CSV:

**Save this as `sales_data.csv`:**
```csv
OrderID,Product,Category,Quantity,Price,OrderDate
1001,Widget A,Electronics,5,29.99,2024-01-15
1002,Gadget B,Electronics,3,49.99,2024-01-16
1003,Widget A,Electronics,10,29.99,2024-01-16
1004,Supply C,Office,25,4.99,2024-01-17
1005,Gadget B,Electronics,2,49.99,2024-01-18
1006,Paper D,Office,100,9.99,2024-01-18
1007,Widget A,Electronics,7,29.99,2024-01-19
1008,Supply C,Office,50,4.99,2024-01-20
```

---

## Step 1: Start a New Workflow

1. Open Alteryx Designer
2. Click **File** > **New Workflow** (or Ctrl+N)
3. You now have a blank canvas

**Save your workflow:**
1. Press Ctrl+S
2. Name it `MyFirstWorkflow.yxmd`
3. Choose a location you'll remember

---

## Step 2: Add Input Data Tool

The Input Data tool reads data from files or databases.

### Add the Tool
1. In the Tool Palette, find **In/Out** category
2. Drag **Input Data** tool onto the canvas
3. The tool appears with a blue icon

### Configure Input Data
1. Click the Input Data tool to select it
2. In the Configuration window, click the **File** dropdown
3. Click **Select File...**
4. Browse to your `sales_data.csv` file
5. Click Open

### Verify the Data
1. Click **Run** (Ctrl+R) to execute the workflow
2. Click the Input Data tool
3. Look at the **Results** window at the bottom
4. You should see your 8 rows of data

```
Your workflow now looks like:
[Input Data]●
```

---

## Step 3: Add Select Tool

The Select tool lets you choose which columns to keep and rename them.

### Add the Tool
1. Find **Select** in the Preparation category
2. Drag it to the right of your Input Data tool

### Connect the Tools
1. Click and drag from the **output anchor** (●) of Input Data
2. Drag to the **input anchor** of Select
3. A line connects them

```
[Input Data]───●───[Select]●
```

### Configure Select
1. Click the Select tool
2. You see a list of all columns
3. For this tutorial, keep all columns checked
4. Notice you could:
   - Uncheck columns to remove them
   - Change the column order by dragging
   - Rename columns by editing the name
   - Change data types using the dropdown

### Run and Verify
1. Press Ctrl+R to run
2. Click the Select tool
3. Verify data flows through unchanged

---

## Step 4: Add Filter Tool

The Filter tool keeps or removes rows based on conditions.

### Add and Connect
1. Drag **Filter** tool from Preparation
2. Connect it after the Select tool

```
[Input Data]───[Select]───[Filter]●
                                  ●
```

Notice: Filter has **two output anchors** (T and F for True/False)

### Configure Filter
We'll keep only Electronics products:

1. Click the Filter tool
2. In Configuration, select **Basic Filter**
3. Set up the condition:
   - Column: `Category`
   - Operator: `Equals`
   - Value: `Electronics`

### Understand the Outputs
- **T (True) anchor**: Rows that match the condition
- **F (False) anchor**: Rows that don't match

### Run and Verify
1. Run the workflow (Ctrl+R)
2. Click the Filter tool
3. In Results, notice two tabs at the bottom: **T** and **F**
4. **T** should show 5 rows (Electronics only)
5. **F** should show 3 rows (Office products)

---

## Step 5: Add Formula Tool

The Formula tool creates new columns or modifies existing ones.

### Add and Connect
1. Drag **Formula** tool from Preparation
2. Connect it to the **T (True)** output of Filter

```
[Input Data]───[Select]───[Filter]─T──[Formula]●
                                  └F──●
```

### Configure Formula
We'll calculate Total Sales (Quantity × Price):

1. Click the Formula tool
2. In **Output Column**, select `-- Create New Column --`
3. Name it: `TotalSales`
4. In the expression box, type:
   ```
   [Quantity] * [Price]
   ```
5. Set **Data Type** to: `Double` (8.2)

### Understanding the Expression
- `[Quantity]` references the Quantity column
- `*` is multiplication
- `[Price]` references the Price column
- Square brackets are used for column names

### Run and Verify
1. Run the workflow
2. Click the Formula tool
3. You should see a new `TotalSales` column
4. Row 1: 5 × 29.99 = 149.95

---

## Step 6: Add Output Data Tool

The Output Data tool saves your results.

### Add and Connect
1. Drag **Output Data** from In/Out
2. Connect it after the Formula tool

```
[Input Data]───[Select]───[Filter]─T──[Formula]───[Output Data]
                                  └F──●
```

### Configure Output Data
1. Click the Output Data tool
2. Click the **File** dropdown
3. Select the format (e.g., `.csv`)
4. Choose a location and filename: `electronics_sales.csv`
5. Click Save

### Output Options
In the Configuration, you can:
- Choose file format (CSV, Excel, database, etc.)
- Set delimiters and encoding
- Choose to append or overwrite
- Configure headers

### Run Final Workflow
1. Run the workflow (Ctrl+R)
2. Check the Messages tab - should say "0 errors, 0 warnings"
3. Open your output file to verify the results

---

## Your Complete Workflow

```
[Input Data]───[Select]───[Filter]─T──[Formula]───[Output Data]
     ↓              ↓          ↓           ↓             ↓
  Read CSV     Choose      Keep only   Calculate    Save
   file        columns    Electronics  TotalSales  results
```

**What it does:**
1. Reads 8 rows from CSV
2. Passes all 6 columns
3. Filters to 5 Electronics rows
4. Adds TotalSales column (now 7 columns)
5. Saves 5 rows to output file

---

## Enhancing Your Workflow

### Add Annotations
1. Right-click any tool
2. Select **Add Annotation**
3. Type a description of what the tool does

### Add a Comment Tool
1. Drag **Comment** from Documentation
2. Position it above your workflow
3. Add title: "Electronics Sales Analysis"

### Clean Up Layout
1. Select all tools (Ctrl+A)
2. Right-click > **Align** > **Horizontal Center**
3. Space tools evenly for readability

---

## Common Issues & Solutions

### "File not found" Error
- Check the file path in Input Data
- Ensure the file isn't open in another program
- Verify you have read permissions

### No Data After Filter
- Check your filter condition
- Click the **F** output to see filtered-out rows
- Try removing the filter temporarily to verify upstream data

### Formula Error
- Check column names are spelled correctly
- Use square brackets: `[ColumnName]`
- Verify data types are compatible

### Output File Won't Save
- Check you have write permissions
- Ensure the folder exists
- Close the file if it's open in Excel

---

## Challenge Exercises

### Exercise 1: Different Filter
Modify the workflow to filter for:
- Orders with Quantity > 10
- Orders from January 17, 2024 or later

### Exercise 2: Additional Calculations
Add more Formula tools to calculate:
- `DiscountedPrice`: Price with 10% discount
- `Revenue`: TotalSales rounded to 2 decimal places

### Exercise 3: Multiple Outputs
- Send Electronics to one file
- Send Office products to another file (connect the F output)

---

## Key Takeaways

1. **Workflows flow left to right** - data moves through connected tools
2. **Tools have anchors** - outputs connect to inputs
3. **Configuration is per-tool** - click a tool to see its settings
4. **Results show data** - click any tool to see its output
5. **Filter has two outputs** - True and False paths

---

## What's Next?

Now that you've built your first workflow, continue learning:

- [Understanding Data Types →](05-Data-Types.md)
- [Core Concepts →](../02-Core-Concepts/01-Workflow-Design.md)
- [Tools Reference →](../03-Tools-Reference/)

---

[← Previous: Interface Overview](03-Interface-Overview.md) | [Next: Data Types →](05-Data-Types.md)
