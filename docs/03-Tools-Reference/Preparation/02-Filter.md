# Filter Tool

[Home](../../../README.md) > [Tools Reference](../README.md) > [Preparation](./README.md) > Filter

---

## Overview

The Filter tool splits data into two streams based on a condition. Rows matching the condition go to the True (T) output; non-matching rows go to the False (F) output.

**Category:** Preparation
**Icon:** Blue funnel

---

## Outputs

The Filter tool has two output anchors:
- **T (True)** - Rows that match the filter condition
- **F (False)** - Rows that don't match

```
                    ┌─ T ─→ Matching rows
[Input]───[Filter]──┤
                    └─ F ─→ Non-matching rows
```

---

## Filter Modes

### Basic Filter
Simple conditions using dropdowns:

```
Field: [Category]
Operator: Equals
Value: Electronics
```

**Available Operators:**
| Operator | Meaning | Example |
|----------|---------|---------|
| Equals | Exact match | Status = "Active" |
| Does Not Equal | Not matching | Status != "Deleted" |
| Less Than | Numeric comparison | Amount < 100 |
| Less Than Or Equal | ≤ comparison | Age <= 65 |
| Greater Than | Numeric comparison | Quantity > 0 |
| Greater Than Or Equal | ≥ comparison | Score >= 90 |
| Is Null | No value | MiddleName Is Null |
| Is Not Null | Has value | Email Is Not Null |
| Contains | Substring match | Name Contains "Corp" |
| Does Not Contain | Excludes substring | Notes Does Not Contain "Error" |
| Is Empty | Null or empty string | Comments Is Empty |
| Is Not Empty | Has content | Phone Is Not Empty |

### Custom Filter
Write expressions for complex conditions:

```
[Amount] > 100 AND [Category] = "Electronics"
```

Custom filter uses the same expression syntax as Formula tool.

---

## Basic Filter Examples

### Equals
```
Field: Status
Operator: Equals
Value: Active
```

### Numeric Comparison
```
Field: Quantity
Operator: Greater Than
Value: 0
```

### Date Comparison
```
Field: OrderDate
Operator: Greater Than Or Equal
Value: 2024-01-01
```

### Text Contains
```
Field: Description
Operator: Contains
Value: urgent
```

### Null Check
```
Field: Email
Operator: Is Not Null
```

---

## Custom Filter Examples

### Multiple Conditions (AND)
```
[Status] = "Active" AND [Amount] > 100
```
Both conditions must be true.

### Multiple Conditions (OR)
```
[Category] = "Electronics" OR [Category] = "Appliances"
```
Either condition can be true.

### Complex Logic
```
([Status] = "Active" AND [Amount] > 100) OR [Priority] = "High"
```
Use parentheses to group conditions.

### Using IN equivalent
```
[Category] IN ("Electronics", "Appliances", "Furniture")
```

### Date Ranges
```
[OrderDate] >= "2024-01-01" AND [OrderDate] < "2024-02-01"
```

### Null Handling
```
NOT IsNull([Email]) AND [Email] != ""
```

### Pattern Matching
```
REGEX_Match([SKU], "^[A-Z]{3}-\d{4}$")
```

---

## Common Patterns

### Keep Only Valid Records
```
[Filter: Status = "Active"]─T──[Continue Processing]
                           └F──[Log Rejected]
```

### Split by Category
```
[Filter: Category = "Electronics"]─T──[Electronics Process]
                                  └F──[Other Process]
```

### Remove Bad Data
```
[Filter: NOT IsNull(RequiredField)]─T──[Valid Data]
                                    └F──[Error Log]
```

### Date-Based Filtering
```
[Filter: OrderDate >= DateTimeToday() - 30]─T──[Recent Orders]
                                           └F──[Historical]
```

---

## Using Both Outputs

You can process both T and F outputs:

```
                        ┌─ T ─→ [Process Valid]─────┐
[Input]───[Filter]──────┤                           ├──[Union]──[Output]
                        └─ F ─→ [Handle Invalid]────┘
```

### Why Use False Output?
- Log rejected records
- Send to exception handling
- Alternative processing path
- Audit trail

---

## Best Practices

### 1. Filter Early
Apply filters close to input:
```
Good:
[Input]───[Filter]───[Heavy Processing]

Bad:
[Input]───[Heavy Processing]───[Filter]
```

### 2. Use Meaningful Names
Rename Filter tools:
```
"Filter Active Customers"
"Filter Last 30 Days"
"Remove Nulls"
```

### 3. Combine Simple Conditions
Use Custom Filter for multiple AND conditions:
```
[Status] = "Active"
AND [Amount] > 0
AND NOT IsNull([CustomerID])
```

### 4. Test Both Outputs
Verify what's being filtered out:
- Connect Browse to F output
- Check record counts
- Validate business logic

### 5. Document Complex Filters
Add comments explaining business rules.

---

## Performance Tips

### Push Filters Down
If reading from database, filter in SQL instead:
```sql
SELECT * FROM Orders WHERE Status = 'Active'
```

### Simpler Conditions First
For OR conditions, put likely-true conditions first.

### Avoid Complex Regex in Filters
If possible, create a flag field first, then filter on the flag.

---

## Troubleshooting

### "All records filtered out"
- Check condition logic
- Verify data values (case sensitivity)
- Look for NULL issues
- Test with simpler condition

### "No records filtered out"
- Condition may be too broad
- Check for type mismatches
- Verify the field contains expected values

### Case Sensitivity
String comparisons are case-sensitive:
```
"Active" != "active" != "ACTIVE"

Fix:
Uppercase([Status]) = "ACTIVE"
```

### Date Comparisons
Ensure dates are Date type, not String:
```
[DateField] >= "2024-01-01"
// Works if DateField is Date type
// May fail if DateField is String
```

---

## Filter vs. Test Tool

| Filter | Test |
|--------|------|
| Single condition | Multiple validations |
| Binary output | Aggregate error reporting |
| Simple use | Complex validation |
| Use for data routing | Use for data quality |

Choose Filter for:
- Simple inclusion/exclusion
- Routing data flows
- Performance-critical filtering

Choose Test for:
- Multiple validation rules
- Error messaging
- Data quality checks

---

## Examples

### Example 1: Active Customers Only
```
Mode: Basic
Field: IsActive
Operator: Equals
Value: True

T Output: Active customers
F Output: Inactive customers
```

### Example 2: Date Range
```
Mode: Custom
Expression:
[TransactionDate] >= "2024-01-01"
AND [TransactionDate] <= "2024-12-31"

T Output: 2024 transactions
F Output: Other years
```

### Example 3: Exclude Problem Records
```
Mode: Custom
Expression:
NOT IsNull([CustomerID])
AND [Amount] > 0
AND [Status] != "Cancelled"

T Output: Valid records
F Output: Problem records for review
```

---

## Related Tools

- [Test](../Developer/Test.md) - Multiple condition validation
- [Formula](03-Formula.md) - Create filter flags
- [Unique](06-Unique.md) - Remove duplicates

---

[← Select](01-Select.md) | [Next: Formula →](03-Formula.md)
