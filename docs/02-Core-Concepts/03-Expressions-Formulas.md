# Expressions & Formulas

[Home](../../README.md) > [Core Concepts](./README.md) > Expressions & Formulas

---

## Introduction

The Formula tool is one of the most powerful and frequently used tools in Alteryx. Master expressions and you can transform any data.

---

## Formula Tool Basics

### Adding a Formula
1. Drag **Formula** tool from Preparation
2. Connect to your data stream
3. In Configuration:
   - Select or create output column
   - Write expression
   - Set data type

### Expression Syntax
```
// Reference columns with square brackets
[ColumnName]

// Basic arithmetic
[Price] * [Quantity]

// String concatenation
[FirstName] + " " + [LastName]

// Conditional logic
IF [Amount] > 100 THEN "Large" ELSE "Small" ENDIF
```

---

## Data Types in Formulas

### Numeric Operations
```
// Addition, subtraction, multiplication, division
[A] + [B]
[A] - [B]
[A] * [B]
[A] / [B]

// Modulo (remainder)
Mod([Number], 10)

// Power
Pow([Base], 2)

// Absolute value
Abs([Number])

// Rounding
Round([Number], 2)      // 2 decimal places
Ceil([Number])          // Round up
Floor([Number])         // Round down
```

### String Operations
```
// Concatenation
[First] + " " + [Last]

// Length
Length([Text])

// Case conversion
Uppercase([Text])
Lowercase([Text])
TitleCase([Text])

// Trimming
Trim([Text])           // Both ends
TrimLeft([Text])       // Left only
TrimRight([Text])      // Right only

// Substring
Substring([Text], 0, 5)  // First 5 characters
Left([Text], 5)          // First 5 characters
Right([Text], 5)         // Last 5 characters

// Find and replace
Replace([Text], "old", "new")
FindString([Text], "search")  // Returns position or -1
```

### Date Operations
```
// Current date/time
DateTimeNow()
DateTimeToday()

// Extract components
DateTimeYear([Date])
DateTimeMonth([Date])
DateTimeDay([Date])

// Date arithmetic
DateTimeAdd([Date], 7, "days")
DateTimeDiff([Date1], [Date2], "days")

// Parsing strings to dates
DateTimeParse([DateString], "%Y-%m-%d")

// Formatting dates to strings
DateTimeFormat([Date], "%m/%d/%Y")
```

---

## Conditional Logic

### IF-THEN-ELSE
```
// Simple condition
IF [Status] = "Active" THEN 1 ELSE 0 ENDIF

// Multiple conditions (nested)
IF [Score] >= 90 THEN "A"
ELSEIF [Score] >= 80 THEN "B"
ELSEIF [Score] >= 70 THEN "C"
ELSEIF [Score] >= 60 THEN "D"
ELSE "F"
ENDIF
```

### SWITCH-CASE Alternative
Use nested IFs or look up in a join instead:
```
IF [Code] = "A" THEN "Apple"
ELSEIF [Code] = "B" THEN "Banana"
ELSEIF [Code] = "C" THEN "Cherry"
ELSE "Unknown"
ENDIF
```

### Null Handling
```
// Check for null
IsNull([Field])

// Replace null
IIF(IsNull([Field]), "Default", [Field])

// Null function
NULL()  // Returns null value
```

---

## Comparison Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `=` | Equals | `[Status] = "Active"` |
| `!=` | Not equals | `[Status] != "Inactive"` |
| `>` | Greater than | `[Amount] > 100` |
| `<` | Less than | `[Amount] < 100` |
| `>=` | Greater or equal | `[Amount] >= 100` |
| `<=` | Less or equal | `[Amount] <= 100` |

### Logical Operators
```
// AND - both must be true
[Status] = "Active" AND [Amount] > 100

// OR - either can be true
[Status] = "Active" OR [Priority] = "High"

// NOT - inverts condition
NOT [IsDeleted]

// Combined
([Status] = "Active" AND [Amount] > 100) OR [Priority] = "High"
```

---

## String Functions Reference

### Manipulation
| Function | Description | Example |
|----------|-------------|---------|
| `Trim(s)` | Remove whitespace | `Trim("  hello  ")` → `"hello"` |
| `Left(s, n)` | First n chars | `Left("hello", 2)` → `"he"` |
| `Right(s, n)` | Last n chars | `Right("hello", 2)` → `"lo"` |
| `Substring(s, start, len)` | Extract portion | `Substring("hello", 1, 3)` → `"ell"` |
| `Replace(s, old, new)` | Replace text | `Replace("hello", "l", "x")` → `"hexxo"` |
| `PadLeft(s, n, char)` | Pad left side | `PadLeft("5", 3, "0")` → `"005"` |
| `PadRight(s, n, char)` | Pad right side | `PadRight("5", 3, "0")` → `"500"` |

### Information
| Function | Description | Example |
|----------|-------------|---------|
| `Length(s)` | String length | `Length("hello")` → `5` |
| `FindString(s, search)` | Find position | `FindString("hello", "l")` → `2` |
| `Contains(s, search)` | Check contains | `Contains("hello", "ell")` → `True` |
| `StartsWith(s, prefix)` | Check start | `StartsWith("hello", "he")` → `True` |
| `EndsWith(s, suffix)` | Check end | `EndsWith("hello", "lo")` → `True` |

### Case Conversion
| Function | Description | Example |
|----------|-------------|---------|
| `Uppercase(s)` | All uppercase | `Uppercase("Hello")` → `"HELLO"` |
| `Lowercase(s)` | All lowercase | `Lowercase("Hello")` → `"hello"` |
| `TitleCase(s)` | Title case | `TitleCase("hello world")` → `"Hello World"` |

---

## Numeric Functions Reference

### Basic Math
| Function | Description | Example |
|----------|-------------|---------|
| `Abs(n)` | Absolute value | `Abs(-5)` → `5` |
| `Round(n, d)` | Round to d decimals | `Round(3.456, 2)` → `3.46` |
| `Ceil(n)` | Round up | `Ceil(3.2)` → `4` |
| `Floor(n)` | Round down | `Floor(3.8)` → `3` |
| `Mod(n, d)` | Remainder | `Mod(10, 3)` → `1` |
| `Pow(base, exp)` | Power | `Pow(2, 3)` → `8` |
| `Sqrt(n)` | Square root | `Sqrt(16)` → `4` |

### Statistics
| Function | Description |
|----------|-------------|
| `RandInt(max)` | Random integer 0 to max-1 |
| `Rand()` | Random decimal 0 to 1 |
| `Log(n)` | Natural logarithm |
| `Log10(n)` | Base-10 logarithm |

---

## Date Functions Reference

### Current Date/Time
| Function | Returns |
|----------|---------|
| `DateTimeNow()` | Current date and time |
| `DateTimeToday()` | Current date (no time) |

### Extract Components
| Function | Returns | Example |
|----------|---------|---------|
| `DateTimeYear(dt)` | Year | `2024` |
| `DateTimeMonth(dt)` | Month (1-12) | `1` |
| `DateTimeDay(dt)` | Day (1-31) | `15` |
| `DateTimeHour(dt)` | Hour (0-23) | `14` |
| `DateTimeMinute(dt)` | Minute (0-59) | `30` |
| `DateTimeSecond(dt)` | Second (0-59) | `45` |
| `DateTimeDayOfWeek(dt)` | Day of week (1=Sun) | `3` |

### Date Arithmetic
```
// Add time
DateTimeAdd([Date], 1, "years")
DateTimeAdd([Date], 2, "months")
DateTimeAdd([Date], 7, "days")
DateTimeAdd([Date], 24, "hours")

// Difference between dates
DateTimeDiff([EndDate], [StartDate], "days")
DateTimeDiff([EndDate], [StartDate], "months")
```

### Parsing and Formatting
```
// String to date
DateTimeParse("2024-01-15", "%Y-%m-%d")
DateTimeParse("01/15/2024", "%m/%d/%Y")
DateTimeParse("Jan 15, 2024", "%b %d, %Y")

// Date to string
DateTimeFormat([Date], "%Y-%m-%d")      // 2024-01-15
DateTimeFormat([Date], "%m/%d/%Y")      // 01/15/2024
DateTimeFormat([Date], "%B %d, %Y")     // January 15, 2024
```

### Format Codes
| Code | Meaning | Example |
|------|---------|---------|
| `%Y` | 4-digit year | 2024 |
| `%y` | 2-digit year | 24 |
| `%m` | Month (01-12) | 01 |
| `%d` | Day (01-31) | 15 |
| `%H` | Hour 24h (00-23) | 14 |
| `%I` | Hour 12h (01-12) | 02 |
| `%M` | Minute (00-59) | 30 |
| `%S` | Second (00-59) | 45 |
| `%p` | AM/PM | PM |
| `%b` | Month abbrev | Jan |
| `%B` | Month name | January |
| `%a` | Day abbrev | Mon |
| `%A` | Day name | Monday |

---

## Type Conversion Functions

| Function | Purpose | Example |
|----------|---------|---------|
| `ToNumber(s)` | String to number | `ToNumber("123")` → `123` |
| `ToString(n)` | Number to string | `ToString(123)` → `"123"` |
| `ToNumber(s, decimal, thousands)` | Locale-aware | `ToNumber("1.234,56", ",", ".")` |

---

## Regular Expressions

### REGEX_Match
Check if pattern matches:
```
REGEX_Match([Email], ".+@.+\..+")  // Basic email check
REGEX_Match([Phone], "^\d{3}-\d{3}-\d{4}$")  // Phone format
```

### REGEX_Replace
Replace matching patterns:
```
// Remove non-digits
REGEX_Replace([Phone], "[^0-9]", "")

// Replace multiple spaces with single
REGEX_Replace([Text], "\s+", " ")
```

### REGEX_CountMatches
Count pattern occurrences:
```
REGEX_CountMatches([Text], "[aeiou]")  // Count vowels
```

### Common Regex Patterns
| Pattern | Matches |
|---------|---------|
| `\d` | Any digit |
| `\w` | Word character (letter, digit, underscore) |
| `\s` | Whitespace |
| `.` | Any character |
| `+` | One or more |
| `*` | Zero or more |
| `?` | Zero or one |
| `^` | Start of string |
| `$` | End of string |
| `[abc]` | Any of a, b, or c |
| `[^abc]` | Not a, b, or c |

---

## Practical Examples

### Clean Phone Numbers
```
// Remove all non-digits
REGEX_Replace([Phone], "[^0-9]", "")

// Format as (XXX) XXX-XXXX
"(" + Left([CleanPhone], 3) + ") " +
Substring([CleanPhone], 3, 3) + "-" +
Right([CleanPhone], 4)
```

### Calculate Age
```
DateTimeDiff(DateTimeToday(), [BirthDate], "years")
```

### Fiscal Year
```
// Fiscal year starts in July
IF DateTimeMonth([Date]) >= 7
THEN DateTimeYear([Date]) + 1
ELSE DateTimeYear([Date])
ENDIF
```

### Full Name from Parts
```
Trim([FirstName]) + " " +
IIF(IsNull([MiddleName]), "", Trim([MiddleName]) + " ") +
Trim([LastName])
```

### Grade Classification
```
IF [Score] >= 90 THEN "A"
ELSEIF [Score] >= 80 THEN "B"
ELSEIF [Score] >= 70 THEN "C"
ELSEIF [Score] >= 60 THEN "D"
ELSE "F"
ENDIF
```

### Percent Change
```
([CurrentValue] - [PreviousValue]) / [PreviousValue] * 100
```

### Handle Division by Zero
```
IF [Denominator] = 0 OR IsNull([Denominator])
THEN 0
ELSE [Numerator] / [Denominator]
ENDIF
```

---

## Formula Tips

### Multiple Expressions
Add multiple columns in one Formula tool:
1. Click `+` to add new expression
2. Each expression creates/modifies one column

### Expression Order
Expressions execute in order listed. Reference earlier expressions:
```
Expression 1: [TaxRate] = 0.08
Expression 2: [Tax] = [Subtotal] * [TaxRate]  // Can use TaxRate
Expression 3: [Total] = [Subtotal] + [Tax]     // Can use Tax
```

### Performance Tips
- Simple expressions are faster
- Avoid repeated calculations (calculate once, reference result)
- Use appropriate data types

### Debugging
- Test expressions with simple data first
- Break complex expressions into steps
- Use Browse tools to check intermediate results

---

## Common Errors

### "Field not found"
- Check spelling of column name
- Ensure column exists at that point in workflow
- Column names are case-sensitive

### "Type mismatch"
- Converting incompatible types
- Use ToString() or ToNumber() explicitly
- Check for null values

### "Syntax error"
- Missing ENDIF or parentheses
- Incorrect function name
- Wrong number of arguments

---

[← Data Connections](02-Data-Connections.md) | [Next: Field Types →](04-Field-Types.md)
