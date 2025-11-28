# Union Tool

[Home](../../../README.md) > [Tools Reference](../README.md) > [Join](./README.md) > Union

---

## Overview

The Union tool stacks multiple data streams vertically - combining rows from different sources into one output.

**Category:** Join
**Icon:** Two rectangles stacking

---

## How It Works

```
Input 1: [A, B, C]     (3 rows)
Input 2: [D, E]        (2 rows)
Input 3: [F, G, H, I]  (4 rows)
         ↓
Output:  [A, B, C, D, E, F, G, H, I]  (9 rows)
```

All inputs are stacked on top of each other.

---

## Configuration Modes

### Auto Config by Name
Match fields by name across inputs:
```
Input 1: ID, Name, Value
Input 2: ID, Name, Amount
Result:  ID, Name, Value, Amount
         (Value null for Input 2, Amount null for Input 1)
```

### Auto Config by Position
Match fields by column order:
```
Input 1: CustomerID, CustomerName
Input 2: CustID, CustName
Result:  Uses first input's names: CustomerID, CustomerName
```

### Manual Configuration
Explicitly map fields from each input to output.

---

## Configuration Options

### Set Field Order
Choose which input determines output field order:
- **Auto** - First connected input
- **Specific input** - Choose input number

### Output Union Field Indicator
Add a field showing which input each row came from:
```
[x] Output Union Field Indicator
Field Name: SourceInput
```

Result adds column:
| Data | SourceInput |
|------|-------------|
| A    | 1           |
| B    | 1           |
| C    | 2           |

---

## Handling Different Structures

### Same Columns
All inputs have identical columns - straightforward stacking.

### Different Columns
Columns unique to one input:
- Other inputs get NULL for that column
- Or use Manual mode to drop

### Different Data Types
Same column name, different types:
- Union tries to reconcile
- May convert to String
- Use Select before Union for consistency

---

## Common Use Cases

### Combine Monthly Files
```
[January]────┐
[February]───┼──[Union]──[Output: Full Year]
[March]──────┘
```

### Stack Multiple Sources
```
[CRM Data]───┐
[Web Data]───┼──[Union]──[Unified Customer List]
[Email Data]─┘
```

### Append Error Records
```
[Valid Data]───┐
               ├──[Union]──[All Records]
[Fixed Errors]─┘
```

---

## Best Practices

### 1. Standardize Before Union
Make column names and types consistent:
```
[Input 1]──[Select: Rename columns]──┐
                                     ├──[Union]
[Input 2]──[Select: Rename columns]──┘
```

### 2. Use Source Indicator
Track which input each row came from:
```
[x] Output Union Field Indicator
```

### 3. Check Column Alignment
Verify columns match as expected:
- Run and inspect results
- Check for unexpected NULLs

### 4. Order Matters for Position Mode
In position-based matching:
- First input determines column names
- Ensure consistent column order

---

## Common Patterns

### Pattern: Dynamic File Union
Read files from folder, union all:
```
[Directory]──[Dynamic Input]──[Union]
```

### Pattern: Add Source Identifier
Before union, add source field:
```
[File A]──[Formula: Source="A"]──┐
                                  ├──[Union]
[File B]──[Formula: Source="B"]──┘
```

### Pattern: Combine Branches
After Filter or other split:
```
[Data]──[Filter]─T──[Process A]──┐
                └F──[Process B]──┴──[Union]──[Output]
```

---

## Troubleshooting

### Missing Data
- Check column names match (case-sensitive)
- Verify inputs are connected
- Check for empty inputs

### Unexpected NULLs
- Columns not matching by name
- Switch to Manual mode
- Standardize column names before Union

### Type Conflicts
- Same column name, different types
- All converted to String
- Use Select to set consistent types

### Row Order
- Output order: Input 1, then Input 2, etc.
- Use Sort after Union if specific order needed

---

## Union vs. Join

| Union | Join |
|-------|------|
| Stacks rows vertically | Combines columns horizontally |
| Adds more rows | Adds more columns |
| Inputs need same structure | Inputs need matching key |
| Like SQL UNION | Like SQL JOIN |

```
Union:          Join:
A               A + 1
B               B + 2
C         vs    C + 3
+
1
2
3
=
A B C 1 2 3    A1 B2 C3
```

---

## Examples

### Example 1: Simple Stack
```
Input 1:
| ID | Name  |
|----|-------|
| 1  | Alice |

Input 2:
| ID | Name |
|----|------|
| 2  | Bob  |

Output:
| ID | Name  |
|----|-------|
| 1  | Alice |
| 2  | Bob   |
```

### Example 2: Different Columns
```
Input 1:
| ID | Name  | Dept |
|----|-------|------|
| 1  | Alice | IT   |

Input 2:
| ID | Name | Region |
|----|------|--------|
| 2  | Bob  | West   |

Output (by Name):
| ID | Name  | Dept | Region |
|----|-------|------|--------|
| 1  | Alice | IT   | NULL   |
| 2  | Bob   | NULL | West   |
```

### Example 3: With Source Indicator
```
Configuration: Output Union Field = "Source"

Output:
| ID | Name  | Source |
|----|-------|--------|
| 1  | Alice | 1      |
| 2  | Bob   | 2      |
```

---

## Related Tools

- [Join](01-Join.md) - Combine on matching keys
- [Append Fields](03-Append-Fields.md) - Add columns from another source
- Dynamic Input - Read multiple files

---

[← Join](01-Join.md) | [Next: Append Fields →](03-Append-Fields.md)
