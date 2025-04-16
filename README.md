# DYNAMIC PRICING ENGINE

The goal is to build a Pricing Engine that dynamically updates product prices based on current inventory and recent sales. The idea is to optimize prices in a way that reflects product demand and stock levels while ensuring a minimum profit margin.

The notebook processes two input CSV files—products.csv and sales.csv—applies a series of rules to adjust prices, and generates an output file updated_prices.csv that contains both the old and new prices (with units).

## Project Files:

- pricing_engine.ipynb: Main Jupyter Notebook that processes the input data and applies pricing logic.

- products.csv: Input file containing product information (SKU, name, current price, cost price, stock).

- sales.csv: Input file containing sales data (SKU, quantity sold in the last 30 days).

- updated_prices.csv: Output file containing updated product prices with units.

- README.md: This document that explains the entire solution.


## Input Description

1. *products.csv*
Contains information about each product including:

- SKU (product ID)

- Product name

- Current selling price

- Cost price (what it costs the company)

- Available stock

2. *sales.csv*
Contains data about recent sales for each product:

- SKU

- Quantity sold in the last 30 days


## How It Works (Module by Module):

 1. **Data Loading**
The notebook starts by importing both CSV files using a data analysis library (like pandas) and stores them in memory as dataframes.

 2. **Data Merging**
To analyze pricing conditions for each product, the two datasets (products.csv and sales.csv) are merged using the SKU as a common identifier. This results in a single table with all relevant details for each product in one row.

     --> If a product has no sales data (i.e., it's not in sales.csv), the sales quantity is considered zero.

 3. **Rule-Based Price Adjustment**
This is the core logic of the engine. Each product is analyzed using a series of pricing rules. Only the first matching rule from the list below is applied (they are in priority order), followed by a final rule that ensures a minimum margin.

    - Rule 1: Low Stock, High Demand (Highest Priority)
      Triggered when stock is very low (less than 20 units) but demand is high (more than 30 units sold).

      The idea is to take advantage of demand and increase price by 15%.

    - Rule 2: Dead Stock
      Triggered when stock is too high (more than 200 units) but no sales in the last 30 days.

      This suggests the product isn’t moving at all, so price is decreased by 30% to attract buyers.

    - Rule 3: Overstocked Inventory
      Triggered when stock is high (over 100 units) and sales are slow (less than 20 units).

      Price is dropped by 10% to encourage movement.

    - Rule 4: Minimum Profit Constraint (Always Applied)
      Regardless of the rule applied above, the engine ensures that the final price is at least 20% more than the cost price.

      If it falls below that threshold, it’s adjusted upward to meet the minimum profit margin.

    --> All prices are finally rounded to two decimal places for formatting and standardization.

4. **Price Formatting**
The old and new prices are formatted to include the unit (INR)—which is a requirement in the challenge. This helps make the pricing clearer in the output file.

   Example:

   - Old Price: 649.99 INR

   - New Price: 600.00 INR

5. **Output Generation**
The notebook creates a new output file named updated_prices.csv which includes:

- SKU

- Old price (with unit)

- New price (with unit)

- This file is ready for submission or further use.



## Output Example


sku     | old_price | new_price
--------|-----------|----------
A123    | 649.99 INR| 600.00 INR
B456    | 699.00 INR| 803.85 INR
C789    | 999.00 INR| 699.30 INR



