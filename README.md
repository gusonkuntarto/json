# E-Commerce Transactions Dataset

This repository contains a structured JSON dataset tracking retail orders, customer profiles, hardware device logs, and nested product purchase details.

## 📊 Dataset Overview

The dataset consists of transaction records containing flattened customer/device identifiers alongside a nested array of items. 

- **Format:** JSON Array
- **Total Records:** 18 order entries (ORD-9001 to ORD-9018)
- **Time Window:** June 15, 2026 – June 24, 2026

## 🧬 Data Schema & Field Definitions

### Root Level (Order Object)

| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `_id` | String | Unique order identifier | `"ORD-9001"` |
| `TRANSACTION_DATE` | String (ISO 8601) | Timestamp of purchase (UTC) | `"2026-06-15T10:20:00Z"` |
| `AMOUNT` | Float | Total value of the transaction | `1500.00` |
| `CUSTOMER__CUSTOMER_ID` | String | Unique customer identifier | `"CUST-101"` |
| `CUSTOMER__NAME` | String | Full name of the customer | `"Alice Wijaya"` |
| `CUSTOMER__EMAIL` | String | Email address of the customer | `"alice@email.com"` |
| `DEVICE__DEVICE_ID` | String | Hardware model or device log tag | `"DEV-IPHONE15"` |
| `DEVICE__IP_ADDRESS` | String | Client IPv4 address used during checkout | `"192.168.1.50"` |
| `items` | Array (Objects) | List of purchased individual products | `[...]` |

### Nested Array: `items`

| Field Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| `ITEMS__PRODUCT_ID` | String | Unique product stock identifier | `"PROD-A"` |
| `ITEMS__CATEGORY` | String | Classification category of the product | `"Electronics"` |
| `ITEMS__PRICE` | Float | Unit price of the individual item | `1200.00` |

## 📦 Sample Payload

```json
{
  "_id": "ORD-9001",
  "TRANSACTION_DATE": "2026-06-15T10:20:00Z",
  "AMOUNT": 1500.00,
  "CUSTOMER__CUSTOMER_ID": "CUST-101",
  "CUSTOMER__NAME": "Alice Wijaya",
  "CUSTOMER__EMAIL": "alice@email.com",
  "DEVICE__DEVICE_ID": "DEV-IPHONE15",
  "DEVICE__IP_ADDRESS": "192.168.1.50",
  "items": [
    { "ITEMS__PRODUCT_ID": "PROD-A", "ITEMS__CATEGORY": "Electronics", "ITEMS__PRICE": 1200.00 },
    { "ITEMS__PRODUCT_ID": "PROD-B", "ITEMS__CATEGORY": "Accessories", "ITEMS__PRICE": 300.00 }
  ]
}
```

## 🛠️ Usage Examples

### Parse Dataset (Python)
```python
import json

# Load dataset
with open('dataset.json', 'r') as file:
    data = json.load(file)

# Calculate total transaction revenue
total_revenue = sum(order['AMOUNT'] for order in data)
print(f"Total Revenue: ${total_revenue:,.2f}")
```

### Flatten Nested Arrays (Pandas)
```python
import pandas as pd

# Flatten the nested "items" array while preserving parent attributes
df = pd.json_normalize(
    data, 
    record_path=['items'], 
    meta=['_id', 'TRANSACTION_DATE', 'AMOUNT', 'CUSTOMER__CUSTOMER_ID']
)
print(df.head())
```
