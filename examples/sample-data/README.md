# Sample Data Files

This folder contains sample data files for practicing Alteryx skills.

## Available Files

| File | Description | Use Case |
|------|-------------|----------|
| `customers.csv` | Customer master data | Joins, filtering |
| `orders.csv` | Sales order data | Calculations, summarization |
| `products.csv` | Product catalog | Lookups, joins |
| `messy_data.csv` | Intentionally messy data | Data cleaning practice |

## File Details

### customers.csv
10 customer records with:
- CustomerID (primary key)
- Name, contact information
- Location data
- Status (Active/Inactive)

### orders.csv
15 order records with:
- OrderID (primary key)
- CustomerID (foreign key)
- Order details
- Discount and status

### products.csv
10 product records with:
- ProductID (primary key)
- Product details
- Pricing and cost
- Stock status

### messy_data.csv
Intentionally problematic data for practicing:
- Inconsistent formatting
- Missing values
- Case variations
- Various date formats
- Whitespace issues

## Practice Exercises

1. **Join Practice**: Join orders with customers and products
2. **Summarization**: Calculate total sales by customer, region, or category
3. **Data Cleaning**: Clean up messy_data.csv
4. **Filtering**: Find active customers with orders over $100

## How to Use

1. Download the files
2. Use Input Data tool in Alteryx
3. Build workflows using these datasets
4. Practice techniques from the guides
