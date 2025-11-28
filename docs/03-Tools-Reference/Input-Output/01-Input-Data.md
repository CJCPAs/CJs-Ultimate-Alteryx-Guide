# Input Data Tool

[Home](../../../README.md) > [Tools Reference](../README.md) > [Input/Output](./README.md) > Input Data

---

## Overview

The Input Data tool reads data from files, databases, and other sources. It's typically the starting point of any workflow.

**Category:** In/Out
**Icon:** Blue arrow pointing right

---

## Supported Data Sources

### Files
| Format | Extensions | Notes |
|--------|------------|-------|
| Delimited | .csv, .txt, .tsv | Comma, tab, pipe, etc. |
| Excel | .xlsx, .xls, .xlsm | Sheets, named ranges |
| Database | .accdb, .mdb | Microsoft Access |
| Alteryx | .yxdb | Native format |
| JSON | .json | Use JSON Parse after |
| XML | .xml | Use XML Parse after |
| Parquet | .parquet | Columnar format |
| Avro | .avro | Apache Avro |
| SAS | .sas7bdat | SAS datasets |

### Databases
- SQL Server
- Oracle
- PostgreSQL
- MySQL
- Snowflake
- Amazon Redshift
- Google BigQuery
- And more via ODBC

### Cloud
- Amazon S3
- Azure Blob Storage
- Google Cloud Storage
- SharePoint
- Salesforce

---

## Basic Configuration

### Reading a File

1. Drag Input Data to canvas
2. In Configuration panel:
   - Click dropdown arrow
   - Select **File** or **Database**
   - Browse to file location
3. Configure format options
4. Run to preview data

### Configuration Options

#### For Delimited Files (CSV/TXT)

| Option | Description |
|--------|-------------|
| **Delimiter** | Character separating columns (comma, tab, etc.) |
| **First Row Contains Field Names** | Header row present |
| **Start Data Import on Line** | Skip initial rows |
| **Field Length** | Auto or set maximum |
| **Quote Character** | Usually double quote (") |
| **File Format** | Encoding (UTF-8, ANSI, etc.) |

#### For Excel Files

| Option | Description |
|--------|-------------|
| **Sheet** | Select specific sheet |
| **Named Range** | Use Excel named range |
| **First Row Contains Data** | Header handling |
| **Import Only Range** | Specify cell range |

---

## Common Use Cases

### Reading CSV with Headers
```
File: sales_data.csv
Delimiter: Comma
First Row Contains Field Names: Yes
```

### Reading Tab-Delimited File
```
File: data.txt
Delimiter: Tab (\t)
First Row Contains Field Names: Yes
```

### Reading Excel Sheet
```
File: report.xlsx
Sheet: Sheet1
First Row Contains Data: Yes
```

### Reading Specific Excel Range
```
File: report.xlsx
Import Only Range: A1:F100
```

### Reading from Database
```
Connection: ODBC or native
Table: Select from list
OR
SQL: Write custom query
```

---

## Advanced Options

### Record Limit
Limit rows for testing:
- Set **Record Limit** value
- Useful for large files during development
- Remove limit for production

### Skip Rows
Start reading after certain rows:
```
Start Data Import on Line: 3
(Skips first 2 rows)
```

### Field Type Options
Configure how types are detected:
- **Auto-detect** - Let Alteryx guess
- **Set manually** - Specify exact types

### Multiple Files
Read multiple files with same structure:
- Use wildcard: `folder\*.csv`
- All matching files combined
- Add `FileName` field option

---

## Database Queries

### Simple Table Select
1. Choose connection
2. Select table from list
3. Alteryx generates: `SELECT * FROM table`

### Custom SQL Query
Write optimized queries:
```sql
SELECT
    CustomerID,
    CustomerName,
    Region
FROM Customers
WHERE Region = 'West'
    AND Active = 1
ORDER BY CustomerName
```

### Query Builder
Use visual query builder:
1. Select tables
2. Choose fields
3. Add filters
4. Preview results

---

## Tips and Best Practices

### 1. Preview Before Processing
Always run Input Data alone first to verify data looks correct.

### 2. Limit During Development
Use Record Limit to work with subset while building workflow.

### 3. Right-Size Field Lengths
Auto-detect may create oversized strings. Use Select or Auto Field after.

### 4. Handle Encoding
For international characters, use UTF-8 or appropriate encoding.

### 5. Use Connection Aliases
For databases, create named connections for easy maintenance.

### 6. Relative Paths
Consider relative paths for portable workflows:
```
.\data\input.csv
```

---

## Troubleshooting

### "File not found"
- Check file path spelling
- Verify file exists
- Check file permissions
- Ensure path is accessible

### Garbled Characters
- Wrong encoding selected
- Try UTF-8 or other encodings
- Check source file encoding

### Missing Columns
- Wrong delimiter selected
- Header row misconfigured
- Try different delimiter

### Wrong Data Types
- Add Select tool after
- Manually set correct types
- Use Auto Field for optimization

### Database Connection Fails
- Verify server accessibility
- Check credentials
- Confirm driver installed
- Test connection outside Alteryx

---

## Examples

### Example 1: Basic CSV
```
Input: customers.csv
Output: CustomerID, Name, Email, Phone, City
```

### Example 2: Excel with Headers
```
Input: sales_report.xlsx, Sheet: January
Output: Date, Product, Quantity, Revenue
```

### Example 3: Database Query
```sql
SELECT o.OrderID, c.CustomerName, o.OrderDate, o.Total
FROM Orders o
INNER JOIN Customers c ON o.CustomerID = c.CustomerID
WHERE o.OrderDate >= '2024-01-01'
```

### Example 4: Multiple Files
```
Input: data\sales_*.csv (wildcard)
Additional field: FileName
Output: All matching files combined with source filename
```

---

## Related Tools

- [Output Data](02-Output-Data.md) - Write data
- [Browse](03-Browse.md) - View data
- [Text Input](04-Text-Input.md) - Manual data entry
- [Directory](05-Directory.md) - List files

---

[← Input/Output Tools](./README.md) | [Next: Output Data →](02-Output-Data.md)
