-- Create the database\CREATE DATABASE ecommerce;
CREATE DATABASE ecommerce;


-- Create the customers table
CREATE TABLE customers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    address TEXT NOT NULL
);

-- Create the orders table
CREATE TABLE orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL,
    total_amount DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);

-- Create the products table
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    description TEXT NOT NULL
);

-- Insert sample data into customers table
INSERT INTO customers (name, email, address) VALUES
('Kavin', 'kavin@gmail.com', '123 Elm St'),
('Balaji', 'jane@gmail.com', '456 Oak St'),
('Sakthi', 'alice@gmail.com', '789 Pine St');

-- Insert sample data into products table
INSERT INTO products (name, price, description) VALUES
('Product A', 30.00, 'Description of Product A'),
('Product B', 50.00, 'Description of Product B'),
('Product C', 40.00, 'Description of Product C');

-- Insert sample data into orders table
INSERT INTO orders (customer_id, order_date, total_amount) VALUES
(1, '2025-01-01', 80.00),
(2, '2025-01-05', 50.00),
(3, '2025-01-10', 100.00);

-- Query 1: Retrieve all customers who have placed an order in the last 30 days
SELECT DISTINCT c.*
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.order_date >= CURDATE() - INTERVAL 30 DAY;

-- Query 2: Get the total amount of all orders placed by each customer
SELECT c.name, SUM(o.total_amount) AS total_spent
FROM customers c
JOIN orders o ON c.id = o.customer_id
GROUP BY c.id;

-- Query 3: Update the price of Product C to 45.00
UPDATE products
SET price = 45.00
WHERE name = 'Product C';

-- Query 4: Add a new column discount to the products table
ALTER TABLE products
ADD COLUMN discount DECIMAL(5, 2) DEFAULT 0.00;

-- Query 5: Retrieve the top 3 products with the highest price
SELECT *
FROM products
ORDER BY price DESC
LIMIT 3;

-- Query 6: Get the names of customers who have ordered Product A
SELECT DISTINCT c.name
FROM customers c
JOIN orders o ON c.id = o.customer_id
JOIN order_items oi ON o.id = oi.order_id
JOIN products p ON oi.product_id = p.id
WHERE p.name = 'Product A';

-- Query 7: Join the orders and customers tables to retrieve the customer's name and order date for each order
SELECT c.name, o.order_date
FROM customers c
JOIN orders o ON c.id = o.customer_id;

-- Query 8: Retrieve the orders with a total amount greater than 150.00
SELECT *
FROM orders
WHERE total_amount > 150.00;

-- Normalization Step: Create a separate table for order items
CREATE TABLE order_items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

-- Update the orders table to remove product-related fields (if any already exist)
-- No specific changes in this case as the orders table remains intact.

-- Query 9: Retrieve the average total of all orders
SELECT AVG(total_amount) AS average_order_total
FROM orders;
