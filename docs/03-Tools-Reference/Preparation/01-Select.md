# Select Tool

[Home](../../../README.md) > [Tools Reference](../README.md) > [Preparation](./README.md) > Select

---

## Overview

The Select tool manages your fields (columns). Use it to keep, remove, rename, reorder, and change the data types of fields.

**Category:** Preparation
**Icon:** Blue checkmark

---

## What It Does

- Choose which fields to include
- Remove unwanted fields
- Rename fields
- Reorder fields
- Change data types
- Set field sizes

---

## Basic Configuration

### Keeping/Removing Fields
1. Connect data to Select tool
2. Check boxes next to fields to keep
3. Uncheck boxes to remove fields
4. Run workflow

### Field List Columns
| Column | Purpose |
|--------|---------|
| **Checkbox** | Include (checked) or exclude (unchecked) |
| **Field** | Original field name |
| **Rename** | New name (leave blank to keep original) |
| **Type** | Data type dropdown |
| **Size** | Field size for strings |
| **Description** | Optional field description |

---

## Common Operations

### Remove Unwanted Columns
Simply uncheck the fields you don't need:
```
[x] CustomerID
[x] CustomerName
[ ] InternalCode    ← Removed
[x] Email
[ ] TempField       ← Removed
```

### Rename Columns
Click in the Rename column and type new name:
```
Field: "Cust_ID" → Rename: "CustomerID"
Field: "Cust_Nm" → Rename: "CustomerName"
```

### Reorder Columns
Drag and drop fields to change order:
1. Click and hold on the move handle (left side)
2. Drag to new position
3. Release

Or use the sort buttons:
- **Sort** - Alphabetical order
- **Move Up/Down** - Adjust position

### Change Data Type
Click the Type dropdown:
```
String → Int32      (for numeric IDs)
String → DateTime   (for date fields)
Double → FixedDecimal   (for currency)
```

---

## Advanced Features

### Select All / Deselect All
Use the checkbox in the header row:
- Check to select all
- Uncheck to deselect all
- Then adjust individual fields

### Unknown Fields
Handle new fields in input data:
- **Show Unknown Fields** option at top
- Choose to include or exclude new fields
- Useful for changing input sources

### Dynamic/Unknown Field Handling Options
| Option | Behavior |
|--------|----------|
| **Passthrough** | Allow all unknown fields |
| **Error** | Fail if unknown fields appear |
| **Warn** | Warn but continue |
| **Ignore** | Silently exclude unknown |

### Setting Field Descriptions
Add descriptions for documentation:
```
Field: CustomerID
Description: "Primary key from CRM system"
```

---

## Type Conversion in Select

### String to Number
When changing String to numeric type:
- Non-numeric values become NULL
- Leading/trailing spaces may cause issues
- Consider cleaning first

### String Size
For String fields, set appropriate size:
```
String(50)   - First/Last names
String(100)  - Full names, emails
String(255)  - Addresses
String(1000) - Descriptions
V_String     - Unknown length
```

### Date Formats
Changing String to Date:
- Alteryx tries to auto-detect format
- May fail on ambiguous dates (01/02/03)
- Use DateTime Parse for complex formats

---

## Best Practices

### 1. Place Early in Workflow
Add Select right after Input Data:
- Remove unneeded columns early
- Reduces memory usage
- Simplifies downstream logic

### 2. Use Meaningful Names
Rename cryptic fields:
```
"Cust_Cd" → "CustomerCode"
"dt_crt" → "CreatedDate"
"amt_01" → "SalesAmount"
```

### 3. Standardize Types
Set consistent types:
- All IDs as Int32 or String (not mixed)
- All dates as DateTime
- All currency as FixedDecimal

### 4. Right-Size Strings
Don't use String(1000) for 10-character data:
- Wastes memory
- Slows processing
- Use Auto Field or check max lengths

### 5. Document Fields
Add descriptions for:
- Business meaning
- Source system
- Transformation notes

---

## Common Patterns

### Pattern: Clean Input Fields
Immediately after Input Data:
```
[Input Data]───[Select]───[Continue...]
                  │
   - Remove temp columns
   - Fix names
   - Set correct types
```

### Pattern: Prepare for Join
Before Join tool:
```
[Table A]───[Select: Keep KeyField, Rename to JoinKey]───┐
                                                         ├─[Join]
[Table B]───[Select: Keep KeyField, Rename to JoinKey]───┘
```

### Pattern: Output Preparation
Before Output Data:
```
[Process]───[Select: Final column order, names]───[Output]
```

---

## Troubleshooting

### Field Name Conflicts
After Join, you might have:
```
CustomerID (from A)
CustomerID_1 (from B)
```
Use Select to rename and remove duplicates.

### Type Conversion Warnings
When changing types:
- Check for unexpected NULL values
- Review the data first
- Consider Formula for complex conversions

### Missing Fields
If expected field doesn't appear:
- Check input source
- Verify upstream processing
- Use "Show Unknown Fields"

---

## Examples

### Example 1: Basic Field Selection
```
Input: 20 fields
Keep: CustomerID, Name, Email, Phone
Result: 4 fields
```

### Example 2: Rename and Reorder
```
Before: id, nm, email, phone
After:  CustomerID, CustomerName, Email, Phone
(Standardized names, logical order)
```

### Example 3: Type Cleanup
```
CustomerID: String → Int32
OrderDate: String → DateTime
Amount: Double → FixedDecimal(19.2)
IsActive: String → Bool
```

---

## Related Tools

- [Formula](03-Formula.md) - Create new fields
- [Auto Field](08-Auto-Field.md) - Automatic type optimization
- [Data Cleansing](07-Data-Cleansing.md) - Clean text values

---

[← Preparation Tools](./README.md) | [Next: Filter →](02-Filter.md)
