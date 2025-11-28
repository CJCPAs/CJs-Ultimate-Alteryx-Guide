# Formula Tool

[Home](../../../README.md) > [Tools Reference](../README.md) > [Preparation](./README.md) > Formula

---

## Overview

The Formula tool creates new fields and modifies existing fields using expressions. It's one of the most used tools in Alteryx.

**Category:** Preparation
**Icon:** Blue "Fx" symbol

---

## Basic Configuration

### Creating a New Field
1. In Output Column, type new field name or select existing
2. Select Data Type (String, Int32, Double, etc.)
3. Write expression
4. Run to test

### Expression Syntax
```
// Reference fields with square brackets
[FieldName]

// Arithmetic
[Price] * [Quantity]

// String operations
[FirstName] + " " + [LastName]

// Conditions
IF [Amount] > 100 THEN "Large" ELSE "Small" ENDIF
```

---

## Multiple Expressions

Add multiple expressions in one Formula tool:
1. Click the "+" button
2. Add another output column and expression
3. Expressions execute in order (top to bottom)

**Tip:** Later expressions can reference earlier ones:
```
Expression 1: [Total] = [Price] * [Quantity]
Expression 2: [Tax] = [Total] * 0.08
Expression 3: [GrandTotal] = [Total] + [Tax]
```

---

## Data Types

### Setting Output Type
Choose appropriate type for your calculation:

| Output | Recommended Type |
|--------|------------------|
| Whole numbers | Int32, Int64 |
| Decimals | Double, Fixed Decimal |
| Currency | Fixed Decimal(19.4) |
| Text | String(n), V_String |
| Dates | DateTime |
| True/False | Bool |

### Type Size
For strings, set appropriate length:
- String(50) - Short text
- String(255) - Medium text
- V_String - Variable/unknown length

---

## Common Formulas

### Arithmetic
```
// Basic math
[A] + [B]           // Addition
[A] - [B]           // Subtraction
[A] * [B]           // Multiplication
[A] / [B]           // Division

// Rounding
Round([Amount], 2)  // 2 decimal places
Ceil([Value])       // Round up
Floor([Value])      // Round down

// Other
Abs([Number])       // Absolute value
Mod([A], [B])       // Remainder
Pow([Base], 2)      // Power
Sqrt([Number])      // Square root
```

### Strings
```
// Combine
[First] + " " + [Last]

// Case
Uppercase([Text])
Lowercase([Text])
TitleCase([Text])

// Extract
Left([Text], 5)         // First 5 chars
Right([Text], 5)        // Last 5 chars
Substring([Text], 0, 5) // Position 0, length 5

// Clean
Trim([Text])            // Remove whitespace
TrimLeft([Text])
TrimRight([Text])

// Replace
Replace([Text], "old", "new")

// Length
Length([Text])
```

### Dates
```
// Current
DateTimeNow()           // Date and time
DateTimeToday()         // Date only

// Extract parts
DateTimeYear([Date])
DateTimeMonth([Date])
DateTimeDay([Date])

// Calculate
DateTimeAdd([Date], 30, "days")
DateTimeDiff([End], [Start], "days")

// Format
DateTimeFormat([Date], "%Y-%m-%d")

// Parse
DateTimeParse([String], "%m/%d/%Y")
```

### Conditions
```
// Simple IF
IF [Status] = "Active" THEN 1 ELSE 0 ENDIF

// Multiple conditions
IF [Score] >= 90 THEN "A"
ELSEIF [Score] >= 80 THEN "B"
ELSEIF [Score] >= 70 THEN "C"
ELSE "F"
ENDIF

// IIF (inline if)
IIF([Amount] > 100, "Large", "Small")

// Null check
IIF(IsNull([Field]), "Default", [Field])
```

### Null Handling
```
// Check null
IsNull([Field])

// Replace null
IIF(IsNull([Amount]), 0, [Amount])

// Create null
NULL()
```

---

## Advanced Formulas

### Regular Expressions
```
// Match pattern
REGEX_Match([Email], ".+@.+\..+")

// Replace with regex
REGEX_Replace([Phone], "[^0-9]", "")

// Extract matches
REGEX_Replace([Text], ".*(\d{5}).*", "$1")
```

### Type Conversion
```
// To number
ToNumber([String])

// To string
ToString([Number])
ToString([Number], 2)    // 2 decimals

// To date
DateTimeParse([String], "%Y-%m-%d")
```

### String Padding
```
// Pad left
PadLeft([ID], 10, "0")    // "0000000123"

// Pad right
PadRight([Name], 20, " ")
```

### Complex Calculations
```
// Percentage of total
[Value] / [Total] * 100

// Running calculation (use Multi-Row Formula)

// Conditional sum
IIF([Category] = "A", [Amount], 0)
```

---

## Best Practices

### 1. One Calculation Per Expression
Break complex formulas into steps:
```
Expression 1: [Subtotal] = [Price] * [Qty]
Expression 2: [Discount] = [Subtotal] * [DiscountRate]
Expression 3: [Total] = [Subtotal] - [Discount]
```
Easier to debug than one complex expression.

### 2. Name Fields Clearly
```
Good: [TotalSalesAmount]
Bad:  [Calc1]
```

### 3. Handle Edge Cases
```
// Avoid division by zero
IIF([Denom] = 0, 0, [Num] / [Denom])

// Handle nulls
IIF(IsNull([Field]), DefaultValue, [Field])
```

### 4. Document Complex Logic
Add annotations explaining business rules.

### 5. Use Appropriate Types
- Double for calculations
- Fixed Decimal for money
- String for IDs that might have leading zeros

---

## Common Patterns

### Full Name from Parts
```
Trim([FirstName]) + " " +
IIF(IsNull([MiddleName]), "", Trim([MiddleName]) + " ") +
Trim([LastName])
```

### Age from Birth Date
```
DateTimeDiff(DateTimeToday(), [BirthDate], "years")
```

### Fiscal Year (July start)
```
IF DateTimeMonth([Date]) >= 7
THEN "FY" + ToString(DateTimeYear([Date]) + 1)
ELSE "FY" + ToString(DateTimeYear([Date]))
ENDIF
```

### Clean Phone Number
```
REGEX_Replace([Phone], "[^0-9]", "")
```

### Percent of Total
```
[Value] / [GrandTotal] * 100
```

### Categorize Values
```
IF [Amount] >= 1000 THEN "Large"
ELSEIF [Amount] >= 100 THEN "Medium"
ELSE "Small"
ENDIF
```

---

## Troubleshooting

### "Unknown field" Error
- Check spelling (case-sensitive)
- Verify field exists at this point
- Check for spaces in name

### "Type mismatch" Error
- Comparing different types
- Use conversion functions
- Check for null values

### "Syntax error"
- Missing ENDIF
- Unbalanced parentheses
- Wrong function name
- Missing commas

### Unexpected NULL Results
- Input has nulls
- Type conversion failed
- Regex didn't match

### Wrong Results
- Check operator precedence
- Verify data types
- Test with known values

---

## Performance Tips

### Keep It Simple
Complex expressions are slower:
```
// Slower
IIF(Condition1, IIF(Condition2, IIF(Condition3, A, B), C), D)

// Faster - use separate formulas or Filter
```

### Pre-Calculate Constants
Calculate once, use many times:
```
Expression 1: [TaxRate] = 0.08
Expression 2: [Tax] = [Amount] * [TaxRate]
```

### Avoid Redundant Calculations
If same value needed in multiple formulas, calculate once.

---

## Related Tools

- [Multi-Field Formula](09-Multi-Field-Formula.md) - Same formula on multiple fields
- [Multi-Row Formula](10-Multi-Row-Formula.md) - Reference other rows
- [Select](01-Select.md) - Change types without formulas
- [Filter](02-Filter.md) - Use conditions to route data

---

## Quick Reference

### Operators
| Operator | Meaning |
|----------|---------|
| + | Add/Concatenate |
| - | Subtract |
| * | Multiply |
| / | Divide |
| = | Equals |
| != | Not equals |
| < > <= >= | Comparisons |
| AND OR NOT | Logic |

### Key Functions
| Function | Purpose |
|----------|---------|
| IF/ELSEIF/ENDIF | Conditions |
| IIF() | Inline condition |
| IsNull() | Check null |
| ToNumber() | Convert to number |
| ToString() | Convert to string |
| DateTimeNow() | Current timestamp |
| REGEX_Match() | Pattern match |

---

[← Filter](02-Filter.md) | [Next: Sort →](04-Sort.md)
