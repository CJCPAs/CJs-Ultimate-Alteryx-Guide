# Data Cleaning Masterclass

[Home](../../README.md) > [Use Cases](./README.md) > Data Cleaning

---

## The Reality of Data

Real-world data is messy. Before analysis, you need to clean it. This guide covers the most common data quality issues and how to fix them.

---

## Common Data Problems

1. **Inconsistent formatting** - Dates, phones, names
2. **Missing values** - Nulls, blanks, "N/A"
3. **Duplicates** - Same record multiple times
4. **Invalid values** - Out of range, wrong type
5. **Whitespace issues** - Leading/trailing spaces
6. **Case inconsistencies** - "ACTIVE", "Active", "active"

---

## Problem 1: Whitespace

### The Issue
```
"  John Smith  " vs "John Smith"
```
Leading/trailing spaces cause join failures and inconsistencies.

### The Solution
```
// Formula tool
Trim([Name])

// For all whitespace (including multiple internal spaces)
REGEX_Replace(Trim([Name]), "\s+", " ")
```

### Using Data Cleansing Tool
1. Add Data Cleansing tool
2. Select fields to clean
3. Check "Remove Leading/Trailing Whitespace"

---

## Problem 2: Case Inconsistencies

### The Issue
```
"ELECTRONICS", "Electronics", "electronics"
```
Same value, different cases.

### The Solution
```
// Standardize to uppercase
Uppercase([Category])

// Or title case
TitleCase([Category])

// Or lowercase
Lowercase([Category])
```

### When to Use Each
| Style | Use Case |
|-------|----------|
| UPPERCASE | Codes, IDs |
| Title Case | Names, titles |
| lowercase | Comparisons, URLs |

---

## Problem 3: Inconsistent Dates

### The Issue
```
"01/15/2024"
"2024-01-15"
"15-Jan-2024"
"January 15, 2024"
```
Same date, different formats.

### The Solution
Parse all to DateTime, then format consistently:

```
// Parse various formats
IF REGEX_Match([Date], "^\d{2}/\d{2}/\d{4}$")
THEN DateTimeParse([Date], "%m/%d/%Y")
ELSEIF REGEX_Match([Date], "^\d{4}-\d{2}-\d{2}$")
THEN DateTimeParse([Date], "%Y-%m-%d")
ELSE NULL()
ENDIF

// Then format output consistently
DateTimeFormat([CleanDate], "%Y-%m-%d")
```

---

## Problem 4: Phone Number Cleanup

### The Issue
```
"(555) 123-4567"
"555-123-4567"
"5551234567"
"+1 555 123 4567"
```

### The Solution
```
// Remove all non-digits
[CleanPhone] = REGEX_Replace([Phone], "[^0-9]", "")

// Format consistently
"(" + Left([CleanPhone], 3) + ") " +
Substring([CleanPhone], 3, 3) + "-" +
Right([CleanPhone], 4)
```

Result: "(555) 123-4567"

---

## Problem 5: Handling Missing Values

### The Issue
- NULL
- Empty string ""
- "N/A", "NA", "n/a"
- "-"
- "Unknown"

### Identifying Missing Values
```
IsNull([Field]) OR
[Field] = "" OR
Uppercase([Field]) IN ("N/A", "NA", "UNKNOWN", "-", "NULL")
```

### Handling Options
```
// Replace with default
IIF(IsNull([Field]) OR [Field] = "", "Unknown", [Field])

// Replace with calculated value (e.g., average)
IIF(IsNull([Amount]), [AvgAmount], [Amount])

// Keep as NULL (for later filtering)
IF [Field] IN ("N/A", "", "-") THEN NULL() ELSE [Field] ENDIF
```

---

## Problem 6: Removing Duplicates

### Simple Duplicates
Use **Unique** tool:
1. Select key fields
2. Check "Unique" for deduplication

### Keeping Specific Duplicate
Use **Summarize** tool:
```
Group By: CustomerID
First: CustomerName (keeps first occurrence)
Max: LastOrderDate (keeps most recent)
```

### Complex Deduplication
```
// Add row number per group
[Input]──[Multi-Row Formula: Row number by group]

// Keep only row 1 per group
[Filter: RowNum = 1]
```

---

## Problem 7: Standardizing Categories

### The Issue
```
"Electronics", "ELECTRONICS", "electronic", "Electr."
```

### The Solution: Lookup Table
Create a lookup table:
| Original | Standardized |
|----------|--------------|
| Electronics | Electronics |
| ELECTRONICS | Electronics |
| electronic | Electronics |
| Electr. | Electronics |

Then use **Find Replace** tool:
```
[Data]──[Find Replace]──[Clean Data]
           ↑
    [Lookup Table]
```

### The Solution: Formula
```
IF Uppercase(Trim([Category])) IN ("ELECTRONICS", "ELECTRONIC", "ELECTR.")
THEN "Electronics"
ELSEIF ...
ENDIF
```

---

## Problem 8: Splitting Combined Fields

### The Issue
```
"John Smith" → First: "John", Last: "Smith"
"123 Main St, Suite 100" → Address1, Address2
```

### The Solution: Text To Columns
1. Add **Text To Columns** tool
2. Set delimiter (space, comma, etc.)
3. Specify number of columns

### The Solution: Regex
```
// First name (before first space)
REGEX_Replace([FullName], "^(\S+).*", "$1")

// Last name (after last space)
REGEX_Replace([FullName], ".*\s(\S+)$", "$1")
```

---

## Problem 9: Combining Fields

### The Issue
```
First: "John", Last: "Smith" → "John Smith"
```

### The Solution
```
// Simple concatenation
[FirstName] + " " + [LastName]

// With null handling
Trim(
    IIF(IsNull([FirstName]), "", [FirstName]) + " " +
    IIF(IsNull([MiddleName]), "", [MiddleName] + " ") +
    IIF(IsNull([LastName]), "", [LastName])
)
```

---

## Problem 10: Validating Data

### Email Validation
```
REGEX_Match([Email], "^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$")
```

### Phone Validation
```
// After cleaning to digits only
Length([Phone]) = 10
```

### Range Validation
```
[Age] >= 0 AND [Age] <= 120
[Date] >= "1900-01-01" AND [Date] <= DateTimeToday()
```

---

## Complete Cleaning Workflow

```
[Raw Data]
     ↓
[Data Cleansing]──────── Trim whitespace, nulls
     ↓
[Formula: Case]──────── Standardize case
     ↓
[Formula: Parse Dates]── Fix date formats
     ↓
[Find Replace]────────── Standardize categories
     ↓
[Test: Validation]────── Validate values
     │
     ├─T─→ [Valid Data]──[Unique]──[Output]
     │
     └─F─→ [Invalid Data]──[Error Log]
```

---

## Data Cleansing Tool Reference

The Data Cleansing tool provides multiple options:

| Option | Purpose |
|--------|---------|
| Null Values | Replace with blank/0/custom |
| Whitespace | Trim, remove, normalize |
| Case | Upper, lower, title |
| Punctuation | Remove unwanted characters |
| Tab/Line Breaks | Replace or remove |

---

## Best Practices

### 1. Profile First
Use Browse with Data Profile to understand your data:
- What values exist?
- How many nulls?
- What formats are present?

### 2. Clean at Source
If possible, fix data at the source system.

### 3. Document Decisions
Record how you handled each issue.

### 4. Validate After Cleaning
Verify cleaning worked:
- Check record counts
- Spot-check samples
- Test edge cases

### 5. Create Reusable Macros
Package cleaning logic into macros for reuse.

---

## Quick Reference

### Common Cleaning Functions
| Task | Function |
|------|----------|
| Trim spaces | `Trim([Field])` |
| Uppercase | `Uppercase([Field])` |
| Remove non-digits | `REGEX_Replace([F], "[^0-9]", "")` |
| Replace null | `IIF(IsNull([F]), "default", [F])` |
| Parse date | `DateTimeParse([F], "%m/%d/%Y")` |

---

[← Use Cases](./README.md) | [Next: ETL Pipelines →](02-ETL-Pipelines.md)
