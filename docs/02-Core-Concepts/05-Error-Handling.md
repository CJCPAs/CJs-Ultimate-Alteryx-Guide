# Error Handling Basics

[Home](../../README.md) > [Core Concepts](./README.md) > Error Handling

---

## Why Error Handling Matters

Data is messy. Files are missing. Connections fail. Good error handling:
- Prevents workflow crashes
- Provides meaningful feedback
- Enables automated recovery
- Maintains data quality

---

## Types of Errors

### Configuration Errors
Occur before running:
- Missing input files
- Invalid tool settings
- Connection problems

**Solution:** Fix configuration before running

### Runtime Errors
Occur during execution:
- Data type mismatches
- Division by zero
- Memory exhaustion

**Solution:** Build defensive logic

### Data Quality Errors
Invalid or unexpected data:
- Null values where not expected
- Format violations
- Out-of-range values

**Solution:** Validate and clean data

---

## The Message Tool

Output information during workflow execution.

### Message Types
| Type | Purpose | Color |
|------|---------|-------|
| **Message** | Informational | None |
| **Warning** | Potential issue | Yellow |
| **Error** | Critical problem | Red |
| **Field Conversion Error** | Type issues | Red |

### Configuration
```
Message Type: [Select type]
Message Expression: "Processing " + ToString(Count) + " records"
```

### Use Cases
```
// Progress information
"Starting data load..."

// Record counts
"Loaded " + ToString([RecordCount]) + " records"

// Conditional warnings
IF [NullCount] > 0 THEN
    "Warning: " + ToString([NullCount]) + " null values found"
ENDIF
```

---

## The Test Tool

Validate data and route based on conditions.

### How It Works
- Define validation expressions
- Records passing all tests → True output
- Records failing any test → False output

### Configuration
```
Test Expression: [Amount] > 0
Error Message: "Amount must be positive"

Test Expression: NOT IsNull([CustomerID])
Error Message: "CustomerID is required"
```

### Multiple Tests
Add multiple tests - all must pass for True output:
```
Test 1: [Amount] > 0
Test 2: NOT IsNull([Date])
Test 3: Length([Name]) > 0
```

### Handling Failed Records
Route failed records to:
- Log file for review
- Error handling workflow
- Notification system

---

## Defensive Formula Writing

### Handle NULLs
```
// Bad - crashes on null
[Quantity] * [Price]

// Good - handles null
IF IsNull([Quantity]) OR IsNull([Price])
THEN 0
ELSE [Quantity] * [Price]
ENDIF
```

### Prevent Division by Zero
```
// Bad - crashes when Denominator is 0
[Numerator] / [Denominator]

// Good - safe division
IF [Denominator] = 0 OR IsNull([Denominator])
THEN NULL()
ELSE [Numerator] / [Denominator]
ENDIF
```

### Safe Type Conversion
```
// Bad - crashes on non-numeric
ToNumber([TextField])

// Good - handles errors
IF REGEX_Match([TextField], "^-?[0-9]+\.?[0-9]*$")
THEN ToNumber([TextField])
ELSE 0
ENDIF
```

### Safe Date Parsing
```
// Validate before parsing
IF REGEX_Match([DateField], "^\d{4}-\d{2}-\d{2}$")
THEN DateTimeParse([DateField], "%Y-%m-%d")
ELSE NULL()
ENDIF
```

---

## Input Validation Patterns

### Required Fields
```
// Using Test Tool
Test: NOT IsNull([CustomerID]) AND [CustomerID] != ""
Error: "CustomerID is required"
```

### Format Validation
```
// Email format
Test: REGEX_Match([Email], ".+@.+\..+")
Error: "Invalid email format"

// Phone format
Test: REGEX_Match([Phone], "^\d{10}$")
Error: "Phone must be 10 digits"
```

### Range Validation
```
// Numeric range
Test: [Age] >= 0 AND [Age] <= 120
Error: "Age must be between 0 and 120"

// Date range
Test: [OrderDate] >= "2020-01-01" AND [OrderDate] <= DateTimeToday()
Error: "Invalid order date"
```

### Referential Validation
Use Join to check if values exist in reference table:
- Inner Join → Only valid records
- Left Join + Filter → Identify invalid references

---

## Handling Missing Files

### Check File Existence
Use Directory tool to list files, then check if expected file exists.

### Dynamic File Paths
Build file paths dynamically:
```
[BasePath] + [FileName] + ".csv"
```

### Default Values
When file might be missing:
1. Use Block Until Done to sequence
2. Check file existence
3. Proceed or use default data

---

## Connection Error Handling

### Database Connections
- Set appropriate timeout values
- Implement retry logic (using iterative macros)
- Have fallback data sources

### API Connections
- Handle HTTP error codes
- Implement exponential backoff
- Log failed requests

### Network Issues
- Cache data locally when possible
- Build workflows to handle partial data
- Include checksums for data integrity

---

## Logging Best Practices

### What to Log
- Workflow start/end times
- Record counts at key points
- Errors and warnings
- Input file names
- Parameter values

### Log Format
Create a standardized log format:
```
[Timestamp] | [Workflow] | [Level] | [Message]
2024-01-15 14:30:00 | SalesReport | INFO | Processing started
2024-01-15 14:30:05 | SalesReport | INFO | Loaded 1000 records
2024-01-15 14:30:10 | SalesReport | WARN | 5 null values in Amount
2024-01-15 14:30:15 | SalesReport | INFO | Processing completed
```

### Logging Implementation
1. Create log records with Formula tool
2. Append to log file with Output tool
3. Use Union to combine log streams

---

## Error Recovery Strategies

### Skip Bad Records
Route invalid records to error output, continue processing valid ones:
```
[Input]───[Test]─T──[Process]───[Output]
               └F──[Error Log]
```

### Default Values
Replace invalid data with defaults:
```
IIF(IsNull([Amount]), 0, [Amount])
```

### Retry Logic
For transient errors (network, API limits):
1. Capture failed records
2. Wait/delay
3. Retry processing
4. After max retries, log failure

### Graceful Degradation
When secondary data unavailable:
1. Check if source exists
2. If not, proceed without it
3. Note limitation in output

---

## Workflow-Level Error Handling

### Events Tab
Configure workflow-level responses:
1. **Workflow** > **Events** tab
2. Add events for errors
3. Configure notifications

### Email Notifications
Send email on error:
- Configure SMTP settings
- Build error message
- Include relevant details

### Run Command
Execute external programs on error:
- Call PowerShell scripts
- Trigger monitoring alerts
- Update status dashboards

---

## Debugging Techniques

### Browse Tool
Insert Browse tools to inspect data:
```
[Input]───[Transform]───[Browse]───[Output]
                           ↑
                    Inspect data here
```

### Disable Containers
Isolate problem areas:
1. Put sections in containers
2. Disable containers to bypass
3. Identify where error occurs

### Run Partial Workflow
- Select subset of tools
- Right-click → Run
- Test sections independently

### Cache and Run
Cache data at problem points:
1. Output to temporary file
2. Input from file to continue
3. Iterate faster on problem section

### Workflow Meta Info
Use **Workflow** > **View** > **XML View** for deep debugging

---

## Error Handling Checklist

### Input Validation
- [ ] Check for required fields
- [ ] Validate data formats
- [ ] Verify value ranges
- [ ] Handle null values

### Processing
- [ ] Protect against division by zero
- [ ] Handle type conversion errors
- [ ] Validate lookup results
- [ ] Check for empty datasets

### Output
- [ ] Verify output file paths
- [ ] Check write permissions
- [ ] Confirm record counts
- [ ] Log completion status

### Recovery
- [ ] Route errors to logs
- [ ] Provide meaningful error messages
- [ ] Consider retry logic
- [ ] Enable notifications

---

## Common Error Patterns

### Pattern: Validate-Process-Report
```
[Input]───[Validate]─Pass──[Process]───[Output]
                    └Fail──[Log Errors]───[Error Report]
```

### Pattern: Try-Catch with Macros
Create a macro that attempts operation and catches failures.

### Pattern: Checkpoint and Recovery
Save progress at checkpoints, resume from last checkpoint on failure.

---

## Quick Reference

### Test Tool Expressions
```
NOT IsNull([Field])           // Required field
[Amount] > 0                  // Positive number
REGEX_Match([Email], ".+@.+") // Email format
[Date] <= DateTimeToday()     // Date not future
Length([Code]) = 5            // Exact length
```

### Safe Formula Patterns
```
// Safe division
IIF([D]=0, 0, [N]/[D])

// Null replacement
IIF(IsNull([F]), "default", [F])

// Safe conversion
IIF(REGEX_Match([S],"^\d+$"), ToNumber([S]), 0)
```

---

[← Field Types](04-Field-Types.md) | [Next: Tools Reference →](../03-Tools-Reference/)
