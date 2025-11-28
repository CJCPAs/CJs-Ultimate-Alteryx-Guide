# Performance Optimization

[Home](../../README.md) > [Advanced Topics](./README.md) > Performance

---

## Why Performance Matters

Slow workflows cost time and resources. Optimizing performance:
- Reduces processing time
- Lowers memory usage
- Improves reliability
- Enables larger data processing

---

## Quick Wins

### 1. Filter Early
Remove unneeded rows as soon as possible:
```
Good:
[Input]──[Filter]──[Heavy Processing]

Bad:
[Input]──[Heavy Processing]──[Filter]
```

### 2. Select Early
Remove unneeded columns:
```
Good:
[Input]──[Select: 5 columns]──[Process]

Bad:
[Input: 100 columns]──[Process]──[Select: 5 columns]
```

### 3. Avoid Unnecessary Sorts
Sorting is expensive. Only sort when:
- Required for specific operations
- Needed for output presentation

### 4. Use Browse Sparingly
Browse tools load data into memory:
- Disable during production runs
- Remove from final workflows
- Use only for debugging

---

## Memory Management

### Understanding Memory Usage
Alteryx loads data into memory. More data = more memory.

### Reduce Data Size
| Action | Impact |
|--------|--------|
| Remove columns | Reduces width |
| Filter rows | Reduces height |
| Use smaller data types | Reduces per-cell size |
| Convert V_String to String | Fixed allocation |

### Auto Field Tool
Automatically optimizes data types:
```
[Input]──[Auto Field]──[Continue...]
```

### String Size Optimization
```
Before: String(1000) × 1M rows = 1 GB
After:  String(50) × 1M rows = 50 MB
```

---

## Workflow Design for Performance

### Parallel Processing
Alteryx can process multiple branches simultaneously:
```
       ┌──[Process A]──┐
[Data]─┤               ├──[Join]──[Output]
       └──[Process B]──┘
```
Both branches process in parallel.

### Avoid Bottlenecks
Single-threaded operations slow everything:
- Large sorts
- Complex regex on every row
- Spatial operations

### Container Disabling
Disable containers you don't need during development:
```
[Input]──[Container: Disabled]──[Output]
         (Skipped during run)
```

---

## Tool-Specific Optimization

### Join Tool
- Filter inputs before joining
- Use same data type on join keys
- Be aware of many-to-many joins

### Summarize Tool
- Pre-filter data
- Limit group-by fields
- Use appropriate aggregations

### Formula Tool
- Keep expressions simple
- Pre-calculate constants
- Avoid nested IIF statements

### Sort Tool
- Only sort when necessary
- Limit sort keys
- Consider if truly needed

### Regex
- Simple patterns faster
- Pre-compile complex patterns
- Consider string functions first

---

## In-Database Processing

### When to Use In-DB
- Data lives in database
- Large datasets
- Database has more resources

### In-DB Advantages
- Processing happens in database
- Only results transfer to Alteryx
- Leverages database indexes

### In-DB Tools
```
[Connect In-DB]──[Filter In-DB]──[Summarize In-DB]──[Data Stream Out]
```

### Push Processing Down
Write optimized SQL:
```sql
SELECT Region, SUM(Amount)
FROM Orders
WHERE OrderDate >= '2024-01-01'
GROUP BY Region
```

---

## Large File Handling

### File Formats
| Format | Speed | Size | Use Case |
|--------|-------|------|----------|
| YXDB | Fast | Small | Alteryx-to-Alteryx |
| Parquet | Fast | Small | Big data |
| CSV | Slow | Large | Exchange |
| Excel | Slowest | Large | End users |

### Use YXDB for Intermediate Files
```
[Heavy Process]──[Output: temp.yxdb]

[Input: temp.yxdb]──[Continue...]
```

### Chunking Large Files
Process in batches:
1. Read portion of file
2. Process
3. Append to output
4. Repeat

---

## Monitoring Performance

### Performance Profiling
Enable profiling to see time per tool:
1. **Options** > **User Settings**
2. **Canvas** tab
3. Enable **Record Performance Profiling**

### Results Window Timing
After running, each tool shows:
- Record count
- Processing time
- % of total time

### Identify Bottlenecks
Look for:
- Tools taking longest time
- Tools processing most records
- Memory warnings

---

## Runtime Settings

### Workflow Settings
**Workflow** > **Runtime** tab:

| Setting | Purpose |
|---------|---------|
| Memory Limit | Cap memory usage |
| Temp Files | Location for temp data |
| Conveyors | Thread settings |

### Temp File Location
Use SSD for temp files:
```
Options > User Settings > Edit User Settings > Defaults
Set Temp Directory to SSD path
```

### 64-bit Mode
Use 64-bit Alteryx for:
- Datasets > 2GB
- Complex workflows
- Memory-intensive operations

---

## Common Performance Issues

### "Out of Memory"
Causes:
- Data too large for RAM
- Memory leak
- Too many Browse tools

Solutions:
- Filter earlier
- Use In-DB processing
- Process in chunks
- Increase virtual memory

### Slow Joins
Causes:
- Large datasets
- Many-to-many relationship
- Unsorted data

Solutions:
- Filter before joining
- Verify join type is correct
- Check for duplicate keys

### Slow Sorts
Causes:
- Sorting large datasets
- Multiple sort keys
- Sorting on strings

Solutions:
- Reduce data first
- Minimize sort keys
- Sort once, not multiple times

---

## Performance Checklist

### Before Building
- [ ] Understand data volumes
- [ ] Identify required columns
- [ ] Plan filtering strategy

### During Development
- [ ] Filter early
- [ ] Select only needed columns
- [ ] Use appropriate data types
- [ ] Minimize Browse tools

### Before Production
- [ ] Remove debug tools
- [ ] Disable unused containers
- [ ] Test with full dataset
- [ ] Profile performance

### For Large Data
- [ ] Consider In-DB processing
- [ ] Use YXDB format
- [ ] Check memory settings
- [ ] Implement chunking if needed

---

## Performance Tips Summary

| Tip | Impact |
|-----|--------|
| Filter early | High |
| Remove columns early | High |
| Use Auto Field | Medium |
| Minimize sorts | Medium |
| Use YXDB format | Medium |
| In-DB for large data | High |
| Remove Browse tools | Medium |
| Simplify formulas | Low-Medium |

---

[← API Integration](02-API-Integration.md) | [Next: Python & R →](04-Python-R-Integration.md)
