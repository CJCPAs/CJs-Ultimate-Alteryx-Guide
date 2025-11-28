# Understanding Data Types

[Home](../../README.md) > [Getting Started](./README.md) > Data Types

---

## Why Data Types Matter

Every column in Alteryx has a data type that determines:
- What values can be stored
- How much memory is used
- What operations are allowed
- How data sorts and compares

Using correct data types prevents errors and improves performance.

---

## Alteryx Data Types Overview

### Numeric Types

| Type | Description | Range | Use Case |
|------|-------------|-------|----------|
| **Byte** | Tiny integer | 0 to 255 | Flags, small counters |
| **Int16** | Small integer | -32,768 to 32,767 | Small whole numbers |
| **Int32** | Standard integer | ±2.1 billion | IDs, counts, years |
| **Int64** | Large integer | ±9.2 quintillion | Large IDs, big counts |
| **Fixed Decimal** | Precise decimal | Varies by precision | Currency, exact math |
| **Float** | Floating point (4 bytes) | ±3.4 × 10³⁸ | Measurements |
| **Double** | Double precision (8 bytes) | ±1.8 × 10³⁰⁸ | Scientific data |

### String Types

| Type | Description | Max Length | Use Case |
|------|-------------|------------|----------|
| **String** | Variable length text | 2 GB | Names, descriptions |
| **WString** | Unicode variable length | 2 GB | International characters |
| **V_String** | Unlimited length | No limit | Large text blocks |
| **V_WString** | Unlimited Unicode | No limit | Large international text |

### Date/Time Types

| Type | Format | Example |
|------|--------|---------|
| **Date** | YYYY-MM-DD | 2024-01-15 |
| **Time** | HH:MM:SS | 14:30:00 |
| **DateTime** | YYYY-MM-DD HH:MM:SS | 2024-01-15 14:30:00 |

### Other Types

| Type | Description | Use Case |
|------|-------------|----------|
| **Bool** | True/False | Flags, binary states |
| **Blob** | Binary data | Images, files |
| **Spatial** | Geographic data | Maps, coordinates |

---

## Type Notation in Alteryx

When you see types in Alteryx, they include size:

```
String(50)    → String with max 50 characters
Double(8.2)   → Double, 8 bytes, 2 decimal display
Int32         → 32-bit integer
V_WString     → Variable-length wide string
```

The notation `Double(8.2)` means:
- 8 = number of bytes for storage
- 2 = decimal places to display (doesn't affect precision)

---

## Choosing the Right Type

### For Whole Numbers
```
Small values (0-255)        → Byte
Typical integers            → Int32
Very large numbers          → Int64
```

### For Decimal Numbers
```
Currency/financial          → Fixed Decimal (19.4)
General calculations        → Double
Scientific precision        → Double
```

### For Text
```
Short text (< 1000 chars)   → String with appropriate size
International text          → WString
Large/unknown text          → V_String or V_WString
```

### For Dates
```
Date only                   → Date
Time only                   → Time
Date and time together      → DateTime
```

---

## Common Type Issues

### Issue 1: Truncated Text
**Problem:** Text is cut off
**Cause:** String size too small
**Fix:** Increase size in Select tool

```
"Customer Service Rep" in String(10)
Result: "Customer S" (truncated!)

Fix: Change to String(50)
```

### Issue 2: Number as Text
**Problem:** Numbers sort incorrectly (1, 10, 2 instead of 1, 2, 10)
**Cause:** Numbers stored as String
**Fix:** Convert to numeric type

### Issue 3: Decimal Precision Loss
**Problem:** 10.50 becomes 10.5 or calculations slightly off
**Cause:** Float used instead of Fixed Decimal
**Fix:** Use Fixed Decimal for financial data

### Issue 4: Date Not Recognized
**Problem:** Dates treated as text, can't sort or calculate
**Cause:** Dates in unexpected format
**Fix:** Use DateTime Parse tool or DateTimeParse function

---

## Converting Data Types

### Using the Select Tool
1. Add a Select tool
2. Click the dropdown in the Type column
3. Choose the new type
4. Alteryx automatically converts

### Using the Formula Tool
```
// String to Number
ToNumber([StringField])

// Number to String
ToString([NumericField])

// String to Date
DateTimeParse([DateString], "%Y-%m-%d")

// Date to String
DateTimeFormat([DateField], "%m/%d/%Y")
```

### Using Auto Field Tool
Automatically optimizes all field types:
1. Add Auto Field tool after your input
2. It analyzes data and sets optimal types
3. Great for reducing memory usage

---

## Type Conversion Reference

### To Number
```
ToNumber("123")          → 123
ToNumber("123.45")       → 123.45
ToNumber("abc")          → Null (warning)
ToNumber("123abc")       → 123 (partial)
```

### To String
```
ToString(123)            → "123"
ToString(123.45)         → "123.45"
ToString([Date])         → "2024-01-15"
```

### To Date
```
DateTimeParse("2024-01-15", "%Y-%m-%d")      → 2024-01-15
DateTimeParse("01/15/2024", "%m/%d/%Y")      → 2024-01-15
DateTimeParse("Jan 15, 2024", "%b %d, %Y")   → 2024-01-15
```

### Date Format Codes
| Code | Meaning | Example |
|------|---------|---------|
| %Y | 4-digit year | 2024 |
| %y | 2-digit year | 24 |
| %m | Month (01-12) | 01 |
| %d | Day (01-31) | 15 |
| %H | Hour (00-23) | 14 |
| %M | Minute (00-59) | 30 |
| %S | Second (00-59) | 45 |
| %b | Month abbreviation | Jan |
| %B | Month full name | January |

---

## Data Type Best Practices

### 1. Right-Size Strings
Don't use String(1000) when String(50) will do:
- Wastes memory
- Slows processing
- Use Auto Field to optimize

### 2. Use Appropriate Numeric Types
```
Good: Int32 for CustomerID
Bad:  Double for CustomerID (wastes space, risks precision)

Good: Fixed Decimal for Price
Bad:  Float for Price (precision issues)
```

### 3. Standardize Dates Early
Convert all dates to DateTime type at input:
- Enables date functions
- Ensures proper sorting
- Prevents comparison issues

### 4. Handle Nulls Intentionally
```
// Check for null before conversion
IF IsNull([Field]) THEN 0 ELSE ToNumber([Field]) ENDIF
```

### 5. Document Non-Obvious Types
Add comments when type choices aren't obvious:
```
// Using Int64 because OrderID exceeds Int32 max
```

---

## Viewing Data Types

### In Results Window
1. Run your workflow
2. Click a tool
3. Look at the column headers
4. Hover over columns to see full type info

### In Select Tool
1. Add a Select tool
2. See all fields listed with their types
3. Types shown in the dropdown

### In Field Info Window
1. Click a tool
2. Go to **View** > **Field Info**
3. See complete type information

---

## Memory Usage by Type

Understanding memory helps optimize large workflows:

| Type | Bytes per Value |
|------|-----------------|
| Bool | 1 |
| Byte | 1 |
| Int16 | 2 |
| Int32 | 4 |
| Int64 | 8 |
| Float | 4 |
| Double | 8 |
| Date | 10 |
| DateTime | 19 |
| String(n) | n bytes |

**Example:**
1 million rows × String(100) = 100 MB
1 million rows × String(10) = 10 MB

---

## Practice Exercise

Create a workflow that demonstrates type conversion:

1. **Input Data** with this CSV:
```csv
ID,Name,Amount,Date,Active
1,John,100.50,01/15/2024,Y
2,Jane,200.75,02/20/2024,N
3,Bob,50.25,03/10/2024,Y
```

2. **Select Tool** - Convert:
   - ID → Int32
   - Name → String(50)
   - Amount → Fixed Decimal(19.2)
   - Date → Keep as String (for now)
   - Active → String(1)

3. **Formula Tool** - Convert Date and Active:
```
// New column: OrderDate
DateTimeParse([Date], "%m/%d/%Y")

// New column: IsActive
[Active] = "Y"
```

4. **Output** and verify types are correct

---

## Key Takeaways

1. **Every field has a type** - understand what type your data is
2. **Types affect behavior** - sorting, calculations, storage
3. **Convert early** - fix types at the start of your workflow
4. **Right-size strings** - don't waste memory on oversized fields
5. **Use Auto Field** - automatically optimize types
6. **Fixed Decimal for money** - avoid floating-point precision issues

---

## Quick Reference

### Conversion Functions
| Function | Purpose |
|----------|---------|
| `ToNumber()` | Convert to number |
| `ToString()` | Convert to string |
| `DateTimeParse()` | String to date |
| `DateTimeFormat()` | Date to string |
| `ToDate()` | Direct date conversion |

### Common Type Settings
| Data | Recommended Type |
|------|-----------------|
| IDs | Int32 or Int64 |
| Currency | Fixed Decimal(19.4) |
| Names | String(100) |
| Descriptions | V_String |
| Dates | DateTime |
| Yes/No flags | Bool |

---

[← Previous: First Workflow](04-First-Workflow.md) | [Next: Core Concepts →](../02-Core-Concepts/01-Workflow-Design.md)
