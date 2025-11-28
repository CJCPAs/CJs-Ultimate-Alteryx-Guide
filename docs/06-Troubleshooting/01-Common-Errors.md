# Common Errors & Solutions

[Home](../../README.md) > [Troubleshooting](./README.md) > Common Errors

---

## Error Message Index

Jump to specific errors:
- [File/Path Errors](#file-and-path-errors)
- [Data Type Errors](#data-type-errors)
- [Formula Errors](#formula-errors)
- [Join Errors](#join-errors)
- [Memory Errors](#memory-errors)
- [Connection Errors](#connection-errors)

---

## File and Path Errors

### "The file or directory does not exist"

**Cause:** File path is invalid or file was moved/deleted.

**Solutions:**
1. Verify the file exists at the specified path
2. Check for typos in the path
3. Ensure network drives are connected
4. Use absolute paths instead of relative
5. Check file permissions

### "Access denied" / "Permission denied"

**Cause:** Insufficient permissions to read/write.

**Solutions:**
1. Check file isn't open in another application
2. Verify folder permissions
3. Run Alteryx as Administrator (if needed)
4. Check if file is read-only

### "The process cannot access the file because it is being used"

**Cause:** File is locked by another process.

**Solutions:**
1. Close the file in Excel or other applications
2. Wait and retry
3. Check for other Alteryx workflows using the file
4. Use a different output filename

---

## Data Type Errors

### "Type mismatch" / "Cannot convert"

**Cause:** Trying to compare or combine incompatible types.

**Solutions:**
1. Use ToNumber() for string to number:
   ```
   ToNumber([StringField])
   ```
2. Use ToString() for number to string:
   ```
   ToString([NumberField])
   ```
3. Match types in Join keys
4. Check for unexpected null values

### "Field not found"

**Cause:** Referenced field doesn't exist.

**Solutions:**
1. Check spelling (case-sensitive)
2. Verify field exists at that point in workflow
3. Check if field was removed by Select tool
4. Use square brackets: `[FieldName]`

### "Cannot convert to date"

**Cause:** Date string doesn't match expected format.

**Solutions:**
1. Check input format and use DateTimeParse:
   ```
   DateTimeParse([Date], "%m/%d/%Y")
   ```
2. Verify data is actually a date
3. Handle non-date values with IF statement

---

## Formula Errors

### "Syntax error"

**Cause:** Invalid formula syntax.

**Common Fixes:**
1. Check for missing ENDIF:
   ```
   IF condition THEN x ELSE y ENDIF
   ```
2. Balance parentheses
3. Use proper operators (AND, OR, not &&, ||)
4. Check function names and arguments

### "Unknown function"

**Cause:** Function name is misspelled or doesn't exist.

**Solutions:**
1. Check spelling (functions are case-insensitive)
2. Verify the function exists in Alteryx
3. Check for extra spaces
4. Refer to function documentation

### "Expression must return [type]"

**Cause:** Formula returns wrong data type.

**Solutions:**
1. Match return type to output column type
2. Use conversion functions
3. Ensure all branches of IF return same type

---

## Join Errors

### "No records in join output"

**Cause:** No matching records between inputs.

**Solutions:**
1. Check join fields have matching values
2. Verify data types match (String vs Int)
3. Check for whitespace differences
4. Look for case sensitivity issues
5. Test with sample data

### "Cartesian join warning"

**Cause:** Many-to-many join creating row explosion.

**Solutions:**
1. Check for duplicate keys in both inputs
2. De-duplicate before joining
3. Verify correct join fields selected
4. Consider if join is appropriate

### "Fields with same name"

**Cause:** Both inputs have fields with identical names.

**Solutions:**
1. Rename fields before join using Select
2. Use Select options in Join configuration
3. Remove duplicates after join

---

## Memory Errors

### "Out of memory"

**Cause:** Data exceeds available RAM.

**Solutions:**
1. Filter data early in workflow
2. Remove unnecessary columns with Select
3. Use Auto Field to optimize types
4. Increase virtual memory
5. Use In-Database processing
6. Process data in chunks

### "Sort failed - insufficient memory"

**Cause:** Sorting requires more memory than available.

**Solutions:**
1. Filter before sorting
2. Sort on fewer fields
3. Increase temp file space
4. Use SSD for temp directory

---

## Connection Errors

### "Cannot connect to database"

**Cause:** Database connection failed.

**Solutions:**
1. Verify server name/address
2. Check credentials
3. Test connection outside Alteryx
4. Verify firewall allows connection
5. Install required drivers
6. Check VPN connection

### "ODBC driver not found"

**Cause:** Required driver not installed.

**Solutions:**
1. Install appropriate ODBC driver
2. Verify 32-bit vs 64-bit match
3. Configure ODBC in Windows
4. Restart Alteryx after installing

### "Connection timeout"

**Cause:** Server didn't respond in time.

**Solutions:**
1. Check network connectivity
2. Increase timeout settings
3. Verify server is running
4. Simplify query if too complex

---

## Workflow Errors

### "Tool has configuration errors"

**Cause:** Tool not properly configured.

**Solutions:**
1. Click the tool - red highlights show issues
2. Complete required fields
3. Check for broken connections
4. Verify input data exists

### "Empty stream" / "No records"

**Cause:** No data flowing to tool.

**Solutions:**
1. Check upstream filters
2. Verify input file has data
3. Check join conditions
4. Look for data quality issues

### "Workflow runtime exceeded"

**Cause:** Workflow taking too long.

**Solutions:**
1. Optimize performance (filter early)
2. Increase timeout setting
3. Check for infinite loops in iterative macros
4. Break into smaller workflows

---

## Error Resolution Process

1. **Read the error message** - Often contains the solution
2. **Identify the tool** - Click the error to highlight
3. **Check configuration** - Verify settings are correct
4. **Inspect upstream data** - Use Browse to see input
5. **Simplify** - Isolate the problem
6. **Test** - Try with known good data
7. **Search** - Check community for similar issues

---

## Prevention Tips

### Validate Early
Add validation at the start:
```
[Input]──[Test: Validate]──[Continue]
```

### Use Defensive Formulas
Handle edge cases:
```
IIF([Denominator] = 0, 0, [Numerator]/[Denominator])
IIF(IsNull([Field]), "Default", [Field])
```

### Add Error Handling
Route errors for review:
```
[Process]──[Filter: Errors]──[Error Log]
```

### Log Progress
Track what's happening:
```
[Message: "Loaded " + ToString([Count]) + " records"]
```

---

[← Troubleshooting](./README.md) | [Next: Performance Issues →](02-Performance-Issues.md)
