ENTITY: customers

Purpose: Stores customer information

Attributes:

customer_id (INT, Primary Key): Unique identifier for each customer

first_name (VARCHAR): Customer’s first name

last_name (VARCHAR): Customer’s last name

email (VARCHAR, UNIQUE): Customer’s email address

phone (VARCHAR): Contact number

created_at (DATE): Account creation date

Relationships:

One customer can place many orders (1:M with orders)

ENTITY: products

Purpose: Stores product details

Attributes:

product_id (INT, Primary Key): Unique product identifier

product_name (VARCHAR): Name of the product

category (VARCHAR): Product category

price (DECIMAL): Price per unit

stock_quantity (INT): Available stock

Relationships:

One product can appear in many order_items (1:M)

ENTITY: orders

Purpose: Stores order information

Attributes:

order_id (INT, Primary Key): Unique order identifier

customer_id (INT, Foreign Key): References customers

order_date (DATE): Date of order

total_amount (DECIMAL): Total order value

Relationships:

One order belongs to one customer

One order can have many order_items

ENTITY: order_items

Purpose: Stores products within each order

Attributes:

order_item_id (INT, Primary Key): Unique identifier

order_id (INT, Foreign Key): References orders

product_id (INT, Foreign Key): References products

quantity (INT): Quantity ordered

item_price (DECIMAL): Price at time of order

Relationships:

Many order_items belong to one order

Many order_items reference one product


3NF->
This database design follows Third Normal Form (3NF) to ensure data integrity, eliminate redundancy, and avoid anomalies.
Each table contains attributes that are fully dependent on the primary key and only the primary key.

Functional Dependencies:

customer_id → first_name, last_name, email, phone

product_id → product_name, category, price

order_id → customer_id, order_date, total_amount

order_item_id → order_id, product_id, quantity, item_price

There are no partial dependencies because all tables use single-column primary keys.
There are no transitive dependencies since non-key attributes do not depend on other non-key attributes.

Update Anomalies:
Product price updates occur only in the products table, preventing inconsistent pricing across orders.

Insert Anomalies:
New customers or products can be added independently without requiring an order.

Delete Anomalies:
Deleting an order does not remove customer or product data, preserving historical records.

By separating customers, products, orders, and order_items into distinct tables, the design maintains data consistency, improves scalability, and supports efficient querying. Hence, the schema fully complies with 3NF principles.