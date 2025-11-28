# Join Tool

[Home](../../../README.md) > [Tools Reference](../README.md) > [Join](./README.md) > Join

---

## Overview

The Join tool combines two data streams based on matching values in one or more key fields. Think of it like VLOOKUP on steroids.

**Category:** Join
**Icon:** Two arrows merging

---

## Inputs and Outputs

### Inputs
- **L (Left)** - Primary data stream
- **R (Right)** - Lookup/secondary data stream

### Outputs
- **J (Join)** - Rows that matched
- **L (Left)** - Left rows with no match
- **R (Right)** - Right rows with no match

```
[Left Input]─L─┐          ┌─J─→ Matched rows
               ├─[Join]───┼─L─→ Left unmatched
[Right Input]─R─┘          └─R─→ Right unmatched
```

---

## Configuration

### Join Fields
Specify which fields to match on:

| Left Field | Right Field |
|------------|-------------|
| CustomerID | CustID |
| OrderDate | Date |

Fields don't need the same name, but must have compatible types.

### Join Type
Choose what to output:

| Type | Output | SQL Equivalent |
|------|--------|----------------|
| Inner | Only matching | INNER JOIN |
| Left Outer | All left + matching right | LEFT JOIN |
| Right Outer | All right + matching left | RIGHT JOIN |
| Full Outer | All from both | FULL OUTER JOIN |

### Select Output Fields
Choose which fields to include from each input:
- Check/uncheck fields
- Rename to avoid conflicts
- Set field order

---

## Join Types Explained

### Inner Join
Only rows that match in both inputs:
```
Left:  A, B, C
Right: B, C, D
Result: B, C (only matching)
```

### Left Outer Join
All left rows, matching right where available:
```
Left:  A, B, C
Right: B, C, D
Result: A (null), B (matched), C (matched)
```

### Right Outer Join
All right rows, matching left where available:
```
Left:  A, B, C
Right: B, C, D
Result: B (matched), C (matched), D (null)
```

### Full Outer Join
All rows from both, matching where possible:
```
Left:  A, B, C
Right: B, C, D
Result: A (left only), B, C (matched), D (right only)
```

---

## Common Use Cases

### Add Customer Info to Orders
```
Left: Orders (OrderID, CustomerID, Amount)
Right: Customers (CustomerID, CustomerName, Region)
Join on: CustomerID
Result: Orders with customer details
```

### Find Missing Records
```
Left: Expected Records
Right: Actual Records
Join on: ID
Use Left Unmatched output → Missing records
```

### Validate References
```
Left: Transactions with CategoryCode
Right: Valid Categories
Join on: CategoryCode
Left unmatched → Invalid categories
```

---

## Multiple Join Keys

Join on multiple fields for precise matching:

| Left Field | Right Field |
|------------|-------------|
| CustomerID | CustomerID |
| ProductID | ProductID |
| OrderDate | TransactionDate |

All fields must match for a row to join.

---

## Handling Duplicates

### One-to-One
Each key appears once in each input:
```
Left:  ID=1, ID=2
Right: ID=1, ID=2
Result: 2 rows
```

### One-to-Many
Key appears once in left, multiple times in right:
```
Left:  CustomerID=1
Right: CustomerID=1, CustomerID=1, CustomerID=1
Result: 3 rows (left duplicated)
```

### Many-to-Many (Cartesian)
Key appears multiple times in both:
```
Left:  ID=1, ID=1 (2 rows)
Right: ID=1, ID=1, ID=1 (3 rows)
Result: 6 rows (2 × 3)
```

**Warning:** Many-to-many joins can explode row counts!

---

## Best Practices

### 1. Understand Your Data
Before joining:
- Know if keys are unique
- Check for duplicates
- Understand expected result

### 2. Use Appropriate Join Type
- Inner when you only want matches
- Left when you need all primary records
- Full Outer for reconciliation

### 3. Handle Unmatched Records
Don't ignore the L and R outputs:
- Log for investigation
- Handle with default values
- Route to error handling

### 4. Rename Conflicting Fields
When both inputs have same field names:
```
CustomerName (from Left)
CustomerName_Right (from Right) ← Rename or remove
```

### 5. Filter Before Joining
Reduce data before join for performance:
```
[Large Table]───[Filter]───[Join]  ✓ Good
[Large Table]───[Join]───[Filter]  ✗ Less efficient
```

---

## Common Patterns

### Left Join with Default Values
```
[Orders]─L─┐
           ├─[Join]─J──[Formula: Fill nulls]──[Output]
[Customers]─R─┘
```

### Find Missing/Extra Records
```
[Expected]─L─┐         ┌─J─→ (discard)
             ├─[Join]──┼─L─→ [Missing Report]
[Actual]─R───┘         └─R─→ [Extra Report]
```

### Lookup and Append
```
[Data]─────────L─┐
                 ├─[Join]─J──[Output]
[Lookup Table]─R─┘
```

---

## Troubleshooting

### "No rows in output"
- Check join fields match
- Verify data types are compatible
- Look for leading/trailing spaces
- Check case sensitivity

### "Too many rows in output"
- Likely many-to-many join
- Check for duplicate keys
- De-duplicate before joining

### "Null values in joined fields"
- Right side had no match
- Using Left/Full Outer join
- Add Formula to handle nulls

### "Type mismatch on join"
- Convert types to match
- String "123" won't match Int 123
- Use Formula/Select to convert

### Performance Issues
- Large datasets take time
- Filter before joining
- Consider In-Database joins
- Check for many-to-many

---

## Examples

### Example 1: Simple Lookup
```
Left (Orders):
| OrderID | CustomerID | Amount |
|---------|------------|--------|
| 1       | 100        | 500    |
| 2       | 101        | 300    |

Right (Customers):
| CustomerID | Name    |
|------------|---------|
| 100        | Alice   |
| 101        | Bob     |

Join on CustomerID → Inner Join

Result (J output):
| OrderID | CustomerID | Amount | Name  |
|---------|------------|--------|-------|
| 1       | 100        | 500    | Alice |
| 2       | 101        | 300    | Bob   |
```

### Example 2: Left Join with Unmatched
```
Left (Orders):
| OrderID | CustomerID |
|---------|------------|
| 1       | 100        |
| 2       | 999        | ← No customer 999

Right (Customers):
| CustomerID | Name |
|------------|------|
| 100        | Alice|

Left Outer Join:

J output:
| OrderID | CustomerID | Name  |
|---------|------------|-------|
| 1       | 100        | Alice |
| 2       | 999        | NULL  |

L output: Empty (all left matched or included)
R output: Empty (no unmatched right)
```

---

## Related Tools

- [Union](02-Union.md) - Stack data vertically
- [Find Replace](04-Find-Replace.md) - Simple lookups
- [Join Multiple](05-Join-Multiple.md) - Join 3+ sources

---

[← Join Tools](./README.md) | [Next: Union →](02-Union.md)
