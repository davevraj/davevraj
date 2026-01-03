Section 1:
FACT TABLE: fact_sales
Grain: One row per product per order line item.
Business process: Sales transactions.


Measures (numeric facts):
-quantity_sold: Number of units sold.
-unit_price: Price per unit at time of sale.
-discount_amount: Discount applied.
-total_amount: Final amount (quantity_sold × unit_price − discount_amount).


Foreign keys:
-date_key → dim_date.
-product_key → dim_product.
-customer_key → dim_customer.


Dimension: dim_date
DIMENSION TABLE: dim_date
Purpose: Date dimension for time‑based analysis.
Type: Conformed dimension.
Attributes:
-date_key (PK): Surrogate key (integer, format: YYYYMMDD).
-full_date: Actual calendar date.
-day_of_week: Monday, Tuesday, etc.
-month: 1–12.
-month_name: January, February, etc.
-quarter: Q1, Q2, Q3, Q4.
-year: 2023, 2024, etc.
-is_weekend: Boolean flag (Yes/No).


Dimension: dim_product
DIMENSION TABLE: dim_product
Purpose: Product details for each sold item.
Type: Conformed dimension.
Attributes:
-product_key (PK): Surrogate key for each product.
-product_id: Business/product code from source system.
-product_name: Name of the product.
-category: Product category (for example, Electronics, Clothing).
-subcategory: More detailed product group if needed.
-brand: Brand or manufacturer name.
-unit_of_measure: Piece, kg, liter, etc.
-is_active: Boolean to mark active/inactive products.


Dimension: dim_customer
DIMENSION TABLE: dim_customer
Purpose: Customer information for sales analysis.
Type: Conformed dimension.
Attributes:
-customer_key (PK): Surrogate key for each customer.
-customer_id: Business/customer code from source system.
-first_name: Customer first name.
-last_name: Customer last name.
-gender: Gender of customer.
-date_of_birth: Birth date.
-city: City of residence.
-state: State/region.
-country: Country.
-customer_segment: Retail, wholesale, corporate, etc.


Section 2:
1.The granularity is at transaction line‑item level so each row shows one product in one order, with its own quantity, price and discount. This makes sales totals correct and allows analysis by product, order, customer or date without double counting.

2.Surrogate keys are used instead of natural keys because they are small, numeric, never change and are always unique. Natural keys like product codes or emails can change when business rules change, and they can be long or multi‑column, which slows joins and makes maintenance harder.

3.This design supports drill‑down and roll‑up because the fact table links to simple dimension tables. Users can roll up by grouping facts through dimensions (for example, day → month → year, or product → category), and drill down by moving from higher‑level attributes in a dimension to more detailed ones while still using the same fact rows.


Section 3:
Source Transaction:
Order #101, Customer "John Doe", Product "Laptop", Qty: 2, Price: 50000, Date: 2024‑01‑15.

ETL Flow:
1.Extract: Read raw sales data from source system (ERP/CRM).
Raw: Order 101 | John Doe | Laptop | 2 units | ₹50,000 | 15 Jan 2024.

2.Transform Dimensions:
dim_date: Lookup/create date_key=20240115 (full_date='2024-01-15', month=1, quarter='Q1').
dim_product: Lookup/create product_key=5 (product_name='Laptop', category='Electronics').
dim_customer: Lookup/create customer_key=12 (customer_name='John Doe', city='Mumbai').

3.Transform Fact: Calculate total_amount = 2 × 50000 = 100000.
Data Warehouse Result:
fact_sales: {date_key: 20240115, product_key: 5, customer_key: 12, quantity_sold: 2, unit_price: 50000, total_amount: 100000}

dim_date: {date_key: 20240115, full_date: '2024-01-15', month: 1, quarter: 'Q1'...}
dim_product: {product_key: 5, product_name: 'Laptop', category: 'Electronics'...}
dim_customer: {customer_key: 12, customer_name: 'John Doe', city: 'Mumbai'...}