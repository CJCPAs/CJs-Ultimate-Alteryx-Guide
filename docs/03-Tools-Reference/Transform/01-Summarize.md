# Summarize Tool

[Home](../../../README.md) > [Tools Reference](../README.md) > [Transform](./README.md) > Summarize

---

## Overview

The Summarize tool groups data and calculates aggregations like sum, count, average, min, max, and more. Essential for reporting and analysis.

**Category:** Transform
**Icon:** Greek sigma (Σ)

---

## How It Works

```
Input:
| Region | Amount |
|--------|--------|
| West   | 100    |
| West   | 200    |
| East   | 150    |
| East   | 50     |

Summarize: Group by Region, Sum Amount

Output:
| Region | Sum_Amount |
|--------|------------|
| West   | 300        |
| East   | 200        |
```

---

## Configuration

### Adding Fields
1. In the Fields list, select a field
2. Choose an Action from the dropdown
3. Configure options as needed

### Action Types

#### Grouping
| Action | Description |
|--------|-------------|
| **Group By** | Create groups (like SQL GROUP BY) |

#### Numeric Aggregations
| Action | Description |
|--------|-------------|
| **Sum** | Total of all values |
| **Count** | Number of rows |
| **Count Distinct** | Unique values only |
| **Count Non-Null** | Non-null values |
| **Count Null** | Null values only |
| **Min** | Smallest value |
| **Max** | Largest value |
| **Avg** | Average (mean) |
| **Median** | Middle value |
| **Mode** | Most frequent value |
| **StdDev** | Standard deviation |
| **Variance** | Statistical variance |
| **First** | First value in group |
| **Last** | Last value in group |

#### String Actions
| Action | Description |
|--------|-------------|
| **Concat** | Combine values with separator |
| **First** | First string value |
| **Last** | Last string value |
| **Min (String)** | Alphabetically first |
| **Max (String)** | Alphabetically last |

#### Spatial
| Action | Description |
|--------|-------------|
| **Combine** | Merge spatial objects |
| **Create Intersection** | Common area |
| **Create Bounding Rect** | Enclosing rectangle |

---

## Common Use Cases

### Sum by Category
```
Group By: Category
Sum: Revenue

Result: Total revenue per category
```

### Count Records
```
Count: * (any field)

Result: Total number of rows
```

### Count by Group
```
Group By: Department
Count: EmployeeID

Result: Employees per department
```

### Multiple Aggregations
```
Group By: Region
Sum: Revenue
Count: Orders
Avg: OrderAmount
Max: OrderDate
```

### Distinct Count
```
Group By: Store
Count Distinct: CustomerID

Result: Unique customers per store
```

---

## Grouping

### No Group By
Summarize entire dataset:
```
Sum: Amount → Total of all amounts (1 row output)
```

### Single Group By
```
Group By: Region
Sum: Amount → One row per region
```

### Multiple Group By
```
Group By: Region, Category
Sum: Amount → One row per Region-Category combination
```

---

## Output Field Naming

By default, Alteryx names aggregated fields:
```
Sum_Amount
Count_Records
Avg_Price
```

### Custom Names
Click the output field name to rename:
```
Sum_Amount → TotalRevenue
Count_Records → OrderCount
```

---

## Advanced Options

### Concat Separator
For Concat action, specify separator:
```
Concat: ProductName
Separator: ", "
Output: "Widget, Gadget, Gizmo"
```

### Percentile
Calculate specific percentile:
```
Action: Percentile
Field: Score
Percentile: 0.25 (for 25th percentile)
```

### Weighted Average
```
Action: Weighted Average
Field: Price
Weight Field: Quantity
```

---

## Best Practices

### 1. Sort Before Summarize (Optional)
Summarize doesn't require sorted input, but results are easier to review if sorted.

### 2. Use Meaningful Field Names
Rename output fields:
```
Sum_Revenue → TotalRevenue
Count → NumberOfOrders
```

### 3. Choose Appropriate Aggregations
- Use SUM for quantities/amounts
- Use COUNT for record counting
- Use AVG for averages (watch for nulls)
- Use COUNT DISTINCT for unique counts

### 4. Handle Nulls
- COUNT excludes nulls
- COUNT NON NULL / COUNT NULL for explicit handling
- SUM, AVG ignore nulls

---

## Common Patterns

### Grand Total
Summarize without Group By:
```
[Data]───[Summarize: Sum Amount]───[Output]
Output: 1 row with total
```

### With Detail and Summary
```
[Data]─┬──[Summarize]──[Summary Output]
       │
       └──[Detail Output]
```

### Running Summary
Combine with Running Total for cumulative:
```
[Data]──[Sort]──[Summarize by Group]──[Running Total]
```

### Percentage of Total
```
1. Summarize for totals
2. Append total to detail
3. Calculate: Detail / Total * 100
```

---

## Troubleshooting

### Missing Groups
- Check Group By field for nulls
- Nulls are treated as a group
- Clean data before summarizing

### Unexpected Counts
- COUNT counts rows, not values
- Use COUNT DISTINCT for unique
- Use COUNT NON NULL to exclude nulls

### Wrong Aggregation Results
- Verify correct field selected
- Check for data type issues
- Preview data before summarizing

### Performance on Large Data
- Filter before summarizing
- Use In-DB Summarize for databases
- Check available memory

---

## Examples

### Example 1: Sales Summary
```
Input:
| Product | Region | Sales |
|---------|--------|-------|
| A       | North  | 100   |
| B       | North  | 150   |
| A       | South  | 200   |
| B       | South  | 100   |

Configuration:
- Group By: Region
- Sum: Sales
- Count: Product (for order count)

Output:
| Region | Sum_Sales | Count |
|--------|-----------|-------|
| North  | 250       | 2     |
| South  | 300       | 2     |
```

### Example 2: Product Stats
```
Configuration:
- Group By: Product
- Min: Price
- Max: Price
- Avg: Price
- Count Distinct: StoreID

Output:
| Product | Min | Max | Avg  | UniqueStores |
|---------|-----|-----|------|--------------|
| Widget  | 10  | 15  | 12.5 | 5            |
```

### Example 3: Concatenate Values
```
Input:
| Customer | Product |
|----------|---------|
| A        | Widget  |
| A        | Gadget  |
| B        | Gizmo   |

Configuration:
- Group By: Customer
- Concat: Product (separator: ", ")

Output:
| Customer | Products       |
|----------|----------------|
| A        | Widget, Gadget |
| B        | Gizmo          |
```

---

## Related Tools

- [Running Total](02-Running-Total.md) - Cumulative calculations
- [Cross Tab](04-Cross-Tab.md) - Pivot with aggregation
- [Unique](06-Unique.md) - De-duplication

---

[← Transform Tools](./README.md) | [Next: Running Total →](02-Running-Total.md)
