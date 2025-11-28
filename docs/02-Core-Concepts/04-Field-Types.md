# Field Types & Conversions

[Home](../../README.md) > [Core Concepts](./README.md) > Field Types

---

## Understanding Field Types

Every field (column) in Alteryx has a type that controls storage, operations, and behavior. Choosing the right type improves performance and prevents errors.

---

## Type Categories

### Numeric Types

| Type | Size | Range | Precision | Use Case |
|------|------|-------|-----------|----------|
| **Byte** | 1 byte | 0 to 255 | Whole numbers | Flags, tiny counters |
| **Int16** | 2 bytes | ±32,767 | Whole numbers | Small integers |
| **Int32** | 4 bytes | ±2.1 billion | Whole numbers | IDs, counts |
| **Int64** | 8 bytes | ±9.2 × 10¹⁸ | Whole numbers | Large IDs |
| **Fixed Decimal** | Varies | Configurable | Exact | Currency, financial |
| **Float** | 4 bytes | ±3.4 × 10³⁸ | ~7 digits | Measurements |
| **Double** | 8 bytes | ±1.8 × 10³⁰⁸ | ~15 digits | Scientific data |

### String Types

| Type | Storage | Max Size | Use Case |
|------|---------|----------|----------|
| **String** | Fixed allocation | 2 GB | Standard text |
| **WString** | Unicode fixed | 2 GB | International text |
| **V_String** | Variable | Unlimited | Large text |
| **V_WString** | Variable Unicode | Unlimited | Large international text |

### Date/Time Types

| Type | Format | Example |
|------|--------|---------|
| **Date** | YYYY-MM-DD | 2024-01-15 |
| **Time** | HH:MM:SS | 14:30:00 |
| **DateTime** | YYYY-MM-DD HH:MM:SS | 2024-01-15 14:30:00 |

### Special Types

| Type | Description | Use Case |
|------|-------------|----------|
| **Bool** | True/False | Flags, binary conditions |
| **Blob** | Binary data | Images, documents |
| **SpatialObj** | Geographic data | Maps, locations |

---

## The Select Tool for Types

The Select tool is your primary way to manage field types.

### Changing Types
1. Add Select tool to workflow
2. Find the field
3. Click the Type dropdown
4. Choose new type

### Type Settings
When you select a type, configure its parameters:
- **String(50)**: Maximum 50 characters
- **Double(8.2)**: 8 bytes storage, display 2 decimals
- **Fixed Decimal(19.4)**: 19 total digits, 4 after decimal

### Common Type Changes

| From | To | Notes |
|------|-----|-------|
| String → Int32 | Numbers in text | Non-numeric values become null |
| String → Date | Date strings | Must match expected format |
| Double → Int32 | Decimal numbers | Truncates decimals |
| Int32 → String | IDs for joining | Preserves leading zeros |

---

## Auto Field Tool

Automatically optimizes field types based on actual data.

### What It Does
- Analyzes data values
- Determines minimum required type
- Converts to optimal type

### When to Use
- After reading from loose file formats
- To reduce memory usage
- Before processing large datasets

### Configuration
- **Convert Strings to WStrings**: Enable for international data
- **Default String Length**: Fallback if analysis can't determine

### Caution
- Run on representative data
- May choose too-small types if sample is limited
- Test with full dataset

---

## Type Conversion Strategies

### Explicit Conversion (Formula Tool)

#### Numbers
```
// String to Number
ToNumber([StringField])

// Number to String
ToString([NumericField])

// With formatting
ToString([Amount], 2)  // 2 decimal places

// Handle conversion errors
ToNumber([Field], 0)  // Returns 0 if conversion fails
```

#### Dates
```
// String to Date
DateTimeParse([DateString], "%Y-%m-%d")

// Date to String
DateTimeFormat([DateField], "%m/%d/%Y")

// Common format strings
"%Y-%m-%d"      // 2024-01-15
"%m/%d/%Y"      // 01/15/2024
"%d-%b-%Y"      // 15-Jan-2024
"%B %d, %Y"     // January 15, 2024
```

#### Booleans
```
// String to Bool
[Flag] = "Y" OR [Flag] = "Yes" OR [Flag] = "1"

// Number to Bool
[Value] != 0

// Bool to Number
IF [BoolField] THEN 1 ELSE 0 ENDIF
```

### Implicit Conversion
Alteryx sometimes converts automatically:
- Comparing string to number (risky)
- Numeric operations on strings (may fail)

**Best practice**: Always convert explicitly to avoid surprises.

---

## String Size Management

### Right-Sizing Strings

#### Problem: Oversized Strings
```
String(1000) for a 10-character field
→ Wastes memory
→ Slows processing
```

#### Solution: Use Auto Field
Or manually set appropriate sizes in Select tool.

### String vs V_String

| Aspect | String(n) | V_String |
|--------|-----------|----------|
| Size | Fixed allocation | Variable |
| Memory | Always uses n bytes | Only uses what's needed |
| Speed | Faster for known sizes | Better for unknown |
| Max | 2 GB | Unlimited |

**Recommendations:**
- Use String(n) when max size is known
- Use V_String for unpredictable text
- Convert V_String to String for final output

### Truncation Issues
When target size is smaller than data:
```
"Customer Service" in String(10)
Result: "Customer S" (truncated!)
```

**Prevention:**
- Check data before setting sizes
- Use data profiling
- Add buffer to expected max

---

## Numeric Precision

### Integer Types
For whole numbers, choose by range:
```
Age (0-120)           → Byte
Year (1900-2100)      → Int16
CustomerID            → Int32
TransactionID (large) → Int64
```

### Decimal Numbers

#### Float vs Double
```
Float:  7 significant digits
Double: 15 significant digits

For most purposes, Double is preferred.
```

#### Fixed Decimal for Money
```
Float:  0.1 + 0.2 = 0.30000000000000004 (!)
Fixed:  0.1 + 0.2 = 0.30 (exact)

Always use Fixed Decimal for financial calculations.
```

#### Fixed Decimal Format
`Fixed Decimal(19.4)` means:
- 19 total digits
- 4 digits after decimal
- 15 digits before decimal
- Common for currency

---

## Date Type Handling

### Recognizing Dates
Alteryx tries to detect date formats, but may fail with:
- Ambiguous formats (01/02/03)
- Non-standard separators
- Text mixed with dates

### Date Conversion Examples
```
// ISO format (recommended)
DateTimeParse([Date], "%Y-%m-%d")
// Input: "2024-01-15" → Date

// US format
DateTimeParse([Date], "%m/%d/%Y")
// Input: "01/15/2024" → Date

// European format
DateTimeParse([Date], "%d/%m/%Y")
// Input: "15/01/2024" → Date

// With time
DateTimeParse([DateTime], "%Y-%m-%d %H:%M:%S")
// Input: "2024-01-15 14:30:00" → DateTime
```

### Extracting Date Parts
```
DateTimeYear([Date])    // 2024
DateTimeMonth([Date])   // 1
DateTimeDay([Date])     // 15
DateTimeHour([Time])    // 14
DateTimeMinute([Time])  // 30
```

---

## NULL Handling

### What is NULL?
NULL means "no value" - different from empty string or zero.

### Checking for NULL
```
IsNull([Field])              // Returns true if null
IsEmpty([Field])             // True if null OR empty string
IsNull([Field]) OR [Field] = ""  // Explicit check
```

### Replacing NULLs
```
// With a default value
IIF(IsNull([Field]), "Default", [Field])

// With zero for numbers
IIF(IsNull([Amount]), 0, [Amount])

// With current date
IIF(IsNull([Date]), DateTimeToday(), [Date])
```

### NULL in Calculations
```
NULL + 5 = NULL
NULL = NULL → False (!)
NULL != NULL → False (!)

// Safe arithmetic
IIF(IsNull([A]) OR IsNull([B]), NULL(), [A] + [B])
```

---

## Best Practices

### 1. Set Types Early
Convert types right after input to catch issues early.

### 2. Use Consistent Types
Same data should have same type everywhere (e.g., all CustomerIDs as Int32).

### 3. Document Non-Obvious Choices
Add comments explaining why a particular type was chosen.

### 4. Profile Data First
Use Browse with Data Profile to understand your data before setting types.

### 5. Test Edge Cases
- Maximum values
- Minimum values
- NULL values
- Empty strings
- Special characters

### 6. Consider Output Requirements
Set types based on what downstream systems expect.

---

## Common Issues and Solutions

### Issue: "Data conversion error"
**Cause:** Incompatible data for target type
**Solution:** Check data, handle exceptions, clean before converting

### Issue: Unexpected NULLs
**Cause:** Failed conversion
**Solution:** Pre-validate data or use error-handling conversion

### Issue: Truncated strings
**Cause:** Target size too small
**Solution:** Increase string size in Select tool

### Issue: Precision loss
**Cause:** Wrong numeric type
**Solution:** Use Fixed Decimal for exact math, Double for scientific

### Issue: Dates not sorting correctly
**Cause:** Dates stored as strings
**Solution:** Convert to proper Date or DateTime type

---

## Quick Reference

### Type Selection Guide
| Data | Recommended Type |
|------|-----------------|
| IDs | Int32 or Int64 |
| Currency | Fixed Decimal(19.4) |
| Percentages | Double |
| Names | String(100) |
| Descriptions | V_String |
| Dates | DateTime |
| Yes/No | Bool |
| Large text | V_WString |

### Conversion Functions
| Function | Purpose |
|----------|---------|
| ToNumber() | String to number |
| ToString() | Any to string |
| DateTimeParse() | String to date |
| DateTimeFormat() | Date to string |

---

[← Expressions & Formulas](03-Expressions-Formulas.md) | [Next: Error Handling →](05-Error-Handling.md)
