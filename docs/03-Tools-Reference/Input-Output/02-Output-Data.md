# Output Data Tool

[Home](../../../README.md) > [Tools Reference](../README.md) > [Input/Output](./README.md) > Output Data

---

## Overview

The Output Data tool writes workflow results to files, databases, or other destinations.

**Category:** In/Out
**Icon:** Blue arrow pointing left

---

## Supported Destinations

### Files
| Format | Extensions | Notes |
|--------|------------|-------|
| Delimited | .csv, .txt | Configurable delimiter |
| Excel | .xlsx | Multiple sheets supported |
| Alteryx | .yxdb | Efficient native format |
| JSON | .json | Structured output |
| Parquet | .parquet | Columnar, compressed |
| PDF | .pdf | Report output |
| HTML | .html | Web-ready |

### Databases
- SQL Server
- Oracle
- PostgreSQL
- MySQL
- Snowflake
- And more via ODBC

### Cloud
- Amazon S3
- Azure Blob Storage
- Google Cloud Storage
- SharePoint

---

## Basic Configuration

### Writing to File

1. Drag Output Data tool and connect
2. Click dropdown in Configuration
3. Select file type and location
4. Configure options
5. Run workflow

### Configuration Options

#### For Delimited Files

| Option | Description |
|--------|-------------|
| **Delimiter** | Comma, Tab, Pipe, etc. |
| **Line Ending** | CRLF (Windows) or LF (Unix) |
| **Write BOM** | Include Byte Order Mark |
| **Quote Fields** | Always, When Needed, Never |
| **First Row Contains Field Names** | Include headers |
| **Code Page** | Encoding (UTF-8, etc.) |

#### For Excel Files

| Option | Description |
|--------|-------------|
| **Sheet Name** | Name of worksheet |
| **Preserve Formatting** | Keep existing formatting |
| **Append to Existing Sheet** | Add rows vs replace |

#### Output Options

| Option | Description |
|--------|-------------|
| **Overwrite** | Replace existing file |
| **Append** | Add to existing file |
| **Create New** | Create new file (error if exists) |

---

## Writing to Different Formats

### CSV File
```
Format: Comma Separated Value (*.csv)
Delimiter: Comma
First Row Contains Field Names: Yes
Code Page: UTF-8
```

### Excel Workbook
```
Format: Microsoft Excel (*.xlsx)
Sheet Name: DataExport
Overwrite Existing Sheet: Yes
```

### Database Table
```
Connection: [Your Database]
Table: OutputTable
Output Options: Append or Overwrite
```

### Multiple Excel Sheets
Use field value as sheet name:
```
Sheet Name: [SheetNameField]
```

---

## Advanced Options

### Conditional File Names
Create dynamic file paths:
```
C:\Output\Report_[Date].csv
```

Use Formula tool before Output to create path:
```
"C:\Output\Report_" + DateTimeFormat(DateTimeToday(), "%Y%m%d") + ".csv"
```

### Pre-Create SQL
Execute SQL before insert:
```sql
TRUNCATE TABLE OutputTable;
-- or
DELETE FROM OutputTable WHERE Date = '2024-01-15';
```

### Post-Create SQL
Execute SQL after insert:
```sql
UPDATE OutputTable SET ProcessedDate = GETDATE();
-- or
EXEC UpdateSummaryTable;
```

### Output Field Map
For databases, map fields to columns:
- Rename fields for database
- Exclude fields from insert
- Handle special characters

---

## Database Output Options

### Append
Add records to existing table:
- Table must exist
- Field types must match
- Fastest for incremental loads

### Overwrite Table (Drop)
Replace entire table:
- Drops and recreates table
- Loses indexes and permissions
- Simple but destructive

### Overwrite Table (Delete Data)
Clear and refill:
- Keeps table structure
- Preserves indexes
- DELETE + INSERT

### Create New Table
Create table if not exists:
- Good for initial deployment
- Define types carefully

### Update/Insert (Upsert)
Match on key, update or insert:
```
Key Field: CustomerID
Update existing, insert new
```

---

## Best Practices

### 1. Use Appropriate Format
- CSV for data exchange
- Excel for business users
- YXDB for Alteryx workflows
- Parquet for big data

### 2. Consider File Size
- Large Excel files slow to open
- Split into multiple files if needed
- Use Parquet for compression

### 3. Handle Existing Files
Decide explicitly:
- Overwrite for regular updates
- Append for logs
- Unique names for archives

### 4. Database Transactions
For critical data:
- Use transaction support
- Implement error handling
- Verify row counts

### 5. Test with Subset
During development:
- Output to temp location
- Verify format is correct
- Check with actual consumers

---

## Common Patterns

### Append with Timestamp
Add records with load date:
```
1. Formula: LoadDate = DateTimeNow()
2. Output: Append to table
```

### Archive Old Data
Before overwriting:
```
1. Read existing data
2. Output to archive folder with date
3. Output new data (overwrite)
```

### Multiple Outputs
Same data to different destinations:
```
[Data]─┬─[Output: CSV]
       ├─[Output: Database]
       └─[Output: Excel]
```

### Dynamic File Names
Based on data content:
```
1. Summarize to get distinct values
2. Formula: Build file path
3. Output with [FilePath] field
```

---

## Troubleshooting

### "Access denied"
- Check write permissions
- Close file if open elsewhere
- Verify folder exists

### "File in use"
- Close file in Excel/other apps
- Check no other process using file
- Try different filename

### "Field type mismatch"
- Compare field types
- Use Select to match expected types
- Check for nulls in non-null columns

### "Connection timeout"
- Check network connectivity
- Increase timeout settings
- Verify database is available

### Large file takes too long
- Use YXDB for Alteryx-to-Alteryx
- Consider Parquet for compression
- Split into multiple files

---

## Examples

### Example 1: Daily Report
```
Path: C:\Reports\Sales_20240115.xlsx
Sheet: DailyData
Overwrite: Yes
```

### Example 2: Append to Log
```
Path: C:\Logs\ProcessLog.csv
Output Option: Append to Existing
Include Field Names: No (after first run)
```

### Example 3: Database Upsert
```
Connection: ProductionDB
Table: CustomerMaster
Mode: Update/Insert
Key: CustomerID
```

### Example 4: Archive Pattern
```
Archive Path: C:\Archive\Data_[YYYYMMDD].csv
Production Path: C:\Current\Data.csv
```

---

## Related Tools

- [Input Data](01-Input-Data.md) - Read data
- [Browse](03-Browse.md) - View without saving
- Render - Create formatted reports

---

[← Input Data](01-Input-Data.md) | [Next: Browse →](03-Browse.md)
