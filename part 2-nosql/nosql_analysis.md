Section A: Limitations of RDBMS

1. products having different attributes is a problem because RDBMS requires the same columns for all rows. For example, laptops need RAM and processor details, while shoes need size and color. To handle this, many extra columns are created, which stay empty for most products and waste space.

2. frequent schema changes are difficult because adding new product types requires altering tables again and again. These changes can be risky, time-consuming, and may affect existing data or applications.

3. storing customer reviews as nested data is not easy in RDBMS. Reviews often include ratings, comments, replies, and likes. Representing this requires multiple tables and complex joins, which makes queries slow and harder to manage.

Because of these limitations, relational databases are less suitable for highly flexible and nested data.


Section B: NoSQL Benefits

1. MongoDB uses a flexible schema, which means each product can have different fields. For example, a laptop document can store RAM and processor details, while a shoe document can store size and color. There is no need to create extra empty columns, making data storage efficient and simple.

2. MongoDB supports embedded documents, which allows customer reviews to be stored directly inside the product document. Ratings, comments, and replies can be stored together in one place. This makes data easier to read and faster to retrieve without using complex joins.

3. MongoDB provides horizontal scalability, meaning data can be distributed across multiple servers. As the number of products, users, or reviews grows, MongoDB can easily handle large data by adding more servers instead of changing the database structure.

Because of these features, MongoDB is well suited for flexible and growing applications.


Section C: Trade-offs

1. MongoDB does not enforce strict relationships like foreign keys. This makes it harder to maintain strong data consistency between products, orders, and customers. Developers must handle these checks manually in the application.

2. MongoDB is not ideal for complex transactions and joins. In a product catalog linked with payments and orders, MySQL handles multi-table transactions more safely and reliably. MongoDB supports transactions, but they are more complex and less efficient compared to relational databases.

Because of these trade-offs, MongoDB is better for flexibility, while MySQL is better for strong consistency and structured data.