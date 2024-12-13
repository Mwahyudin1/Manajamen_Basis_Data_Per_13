# Manajamen Basis Data Pertemuan 13 TugasPraktikum

### Nama: M Wahyudin Ardiansyah
### Nim: 312210148
### Kelas: TI.22.A1
### Dosen Pengampu: Wahyu Hadikristanto, S.Kom., M.Kom.
----
# `Praktikum Toko`
- Berikut script database praktikum Toko
```
-- Tabel Suppliers
CREATE TABLE IF NOT EXISTS Suppliers (
    supplier_id INT AUTO_INCREMENT PRIMARY KEY,
    supplier_name VARCHAR(100) NOT NULL,
    supplier_contact VARCHAR(100),
    supplier_phone VARCHAR(15),
    supplier_address TEXT
);

-- Tabel Categories
CREATE TABLE IF NOT EXISTS Categories (
    category_id INT AUTO_INCREMENT PRIMARY KEY,
    category_name VARCHAR(50) NOT NULL
);

-- Tabel Products
CREATE TABLE IF NOT EXISTS Products (
    product_id INT AUTO_INCREMENT PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    product_description TEXT,
    product_price DECIMAL(10, 2) NOT NULL,
    product_stock INT NOT NULL,
    product_category VARCHAR(50),
    category_id INT,
    product_supplier_id INT,
    FOREIGN KEY (category_id) REFERENCES Categories(category_id),
    FOREIGN KEY (product_supplier_id) REFERENCES Suppliers(supplier_id)
);

-- Tabel Customers
CREATE TABLE IF NOT EXISTS Customers (
    customer_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    customer_email VARCHAR(100) UNIQUE,
    customer_phone VARCHAR(15),
    customer_address TEXT
);

-- Tabel Employees
CREATE TABLE IF NOT EXISTS Employees (
    employee_id INT AUTO_INCREMENT PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL,
    employee_position VARCHAR(50),
    employee_salary DECIMAL(10, 2),
    hire_date DATE,
    employee_phone VARCHAR(15),
    employee_address TEXT
);

-- Tabel Sales
CREATE TABLE IF NOT EXISTS Sales (
    sale_id INT AUTO_INCREMENT PRIMARY KEY,
    sale_date DATE NOT NULL,
    sale_total DECIMAL(10, 2) NOT NULL,
    customer_id INT,
    employee_id INT,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id),
    FOREIGN KEY (employee_id) REFERENCES Employees(employee_id)
);

-- Tabel Sale_Items
CREATE TABLE IF NOT EXISTS Sale_Items (
    sale_item_id INT AUTO_INCREMENT PRIMARY KEY,
    sale_id INT,
    product_id INT,
    quantity_sold INT NOT NULL,
    product_price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (sale_id) REFERENCES Sales(sale_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

-- Tabel Inventory
CREATE TABLE IF NOT EXISTS Inventory (
    inventory_id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT,
    quantity_in_stock INT NOT NULL,
    last_update DATE,
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

-- Tabel Purchase_Orders
CREATE TABLE IF NOT EXISTS Purchase_Orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    order_date DATE NOT NULL,
    supplier_id INT,
    total_amount DECIMAL(10, 2),
    status VARCHAR(20),
    FOREIGN KEY (supplier_id) REFERENCES Suppliers(supplier_id)
);

-- Tabel Order_Items
CREATE TABLE IF NOT EXISTS Order_Items (
    order_item_id INT AUTO_INCREMENT PRIMARY KEY,
    order_id INT,
    product_id INT,
    quantity_ordered INT NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES Purchase_Orders(order_id),
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

-- Tabel Payments
CREATE TABLE IF NOT EXISTS Payments (
    payment_id INT AUTO_INCREMENT PRIMARY KEY,
    sale_id INT,
    payment_date DATE NOT NULL,
    payment_amount DECIMAL(10, 2),
    payment_method VARCHAR(20),
    FOREIGN KEY (sale_id) REFERENCES Sales(sale_id)
);

-- Tabel Expenses
CREATE TABLE IF NOT EXISTS Expenses (
    expense_id INT AUTO_INCREMENT PRIMARY KEY,
    expense_description TEXT,
    expense_amount DECIMAL(10, 2),
    expense_date DATE NOT NULL,
    employee_id INT,
    FOREIGN KEY (employee_id) REFERENCES Employees(employee_id)
);

-- Tabel Discounts
CREATE TABLE IF NOT EXISTS Discounts (
    discount_id INT AUTO_INCREMENT PRIMARY KEY,
    discount_description TEXT,
    discount_percentage DECIMAL(5, 2),
    start_date DATE,
    end_date DATE,
    product_id INT,
    FOREIGN KEY (product_id) REFERENCES Products(product_id)
);

-- Tabel Product_Returns
CREATE TABLE IF NOT EXISTS Product_Returns (
    return_id INT AUTO_INCREMENT PRIMARY KEY,
    sale_item_id INT,
    return_date DATE,
    return_reason TEXT,
    FOREIGN KEY (sale_item_id) REFERENCES Sale_Items(sale_item_id)
);

-- Tabel Promotions
CREATE TABLE IF NOT EXISTS Promotions (
    promotion_id INT AUTO_INCREMENT PRIMARY KEY,
    promotion_name VARCHAR(100),
    promotion_description TEXT,
    start_date DATE,
    end_date DATE
);

-- Tabel User_Accounts
CREATE TABLE IF NOT EXISTS User_Accounts (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE,
    password VARCHAR(255) NOT NULL,
    role VARCHAR(20)
);

-- Tabel Product_Reviews
CREATE TABLE IF NOT EXISTS Product_Reviews (
    review_id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT,
    customer_id INT,
    review_date DATE,
    review_text TEXT,
    rating INT,
    FOREIGN KEY (product_id) REFERENCES Products(product_id),
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);

-- Tabel Employee_Shifts
CREATE TABLE IF NOT EXISTS Employee_Shifts (
    shift_id INT AUTO_INCREMENT PRIMARY KEY,
    employee_id INT,
    shift_start_time DATETIME,
    shift_end_time DATETIME,
    FOREIGN KEY (employee_id) REFERENCES Employees(employee_id)
);

-- Tabel Supplier_Transactions
CREATE TABLE IF NOT EXISTS Supplier_Transactions (
    transaction_id INT AUTO_INCREMENT PRIMARY KEY,
    supplier_id INT,
    transaction_date DATE,
    total_amount DECIMAL(10, 2),
    payment_method VARCHAR(20),
    FOREIGN KEY (supplier_id) REFERENCES Suppliers(supplier_id)
);

-- Tabel Store_Locations
CREATE TABLE IF NOT EXISTS Store_Locations (
    store_id INT AUTO_INCREMENT PRIMARY KEY,
    store_name VARCHAR(100),
    store_address TEXT,
    store_phone VARCHAR(15)
);

-- Tabel Inventory_Logs
CREATE TABLE IF NOT EXISTS Inventory_Logs (
    log_id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT,
    change_quantity INT,
    change_date DATETIME,
    reason VARCHAR(100),
    employee_id INT,
    FOREIGN KEY (product_id) REFERENCES Products(product_id),
    FOREIGN KEY (employee_id) REFERENCES Employees(employee_id)
);

-- Tabel Customer_Loyalty
CREATE TABLE IF NOT EXISTS Customer_Loyalty (
    loyalty_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    points INT,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id)
);

-- Tabel Delivery_Orders
CREATE TABLE IF NOT EXISTS Delivery_Orders (
    delivery_id INT AUTO_INCREMENT PRIMARY KEY,
    sale_id INT,
    delivery_address TEXT,
    delivery_status VARCHAR(20),
    delivery_date DATE,
    FOREIGN KEY (sale_id) REFERENCES Sales(sale_id)
);

-- Tabel Audit_Logs
CREATE TABLE IF NOT EXISTS Audit_Logs (
    audit_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    action VARCHAR(100),
    action_date DATETIME,
    FOREIGN KEY (user_id) REFERENCES User_Accounts(user_id)
);

-- Tabel Inventory_Reorders
CREATE TABLE IF NOT EXISTS Inventory_Reorders (
    reorder_id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT,
    reorder_quantity INT,
    reorder_date DATE,
    supplier_id INT,
    FOREIGN KEY (product_id) REFERENCES Products(product_id),
    FOREIGN KEY (supplier_id) REFERENCES Suppliers(supplier_id)
);

```

- Melakukan insert data ke dalam tabel<br>Berikut script insert data

```
INSERT INTO Suppliers (supplier_name, supplier_contact, supplier_phone, supplier_address)
VALUES
    ('Supplier A', 'John Doe', '081234567890', 'Jl. Supplier A No.1'),
    ('Supplier B', 'Jane Smith', '081234567891', 'Jl. Supplier B No.2'),
    ('Supplier C', 'Michael Johnson', '081234567892', 'Jl. Supplier C No.3'),
    ('Supplier D', 'Emily Davis', '081234567893', 'Jl. Supplier D No.4'),
    ('Supplier E', 'David Wilson', '081234567894', 'Jl. Supplier E No.5');

INSERT INTO Categories (category_name)
VALUES
    ('Electronics'),
    ('Furniture'),
    ('Clothing'),
    ('Books'),
    ('Groceries');

INSERT INTO Products (product_name, product_description, product_price, product_stock, category_id, product_supplier_id)
VALUES
    ('Smartphone', 'Latest model with 5G support', 5000000.00, 50, 1, 1),
    ('Table', 'Wooden dining table', 2000000.00, 20, 2, 2),
    ('T-Shirt', 'Cotton t-shirt with cool design', 150000.00, 100, 3, 3),
    ('Novel', 'A mystery novel by famous author', 100000.00, 200, 4, 4),
    ('Rice', 'Premium rice, 5kg pack', 80000.00, 300, 5, 5);

INSERT INTO Customers (customer_name, customer_email, customer_phone, customer_address)
VALUES
    ('Alice Green', 'alice.green@example.com', '082345678901', 'Jl. Customer A No.1'),
    ('Bob Brown', 'bob.brown@example.com', '082345678902', 'Jl. Customer B No.2'),
    ('Charlie White', 'charlie.white@example.com', '082345678903', 'Jl. Customer C No.3'),
    ('David Black', 'david.black@example.com', '082345678904', 'Jl. Customer D No.4'),
    ('Eva Blue', 'eva.blue@example.com', '082345678905', 'Jl. Customer E No.5');

INSERT INTO Employees (employee_name, employee_position, employee_salary, hire_date, employee_phone, employee_address)
VALUES
    ('John Carter', 'Manager', 5000000.00, '2023-01-01', '083456789012', 'Jl. Employee A No.1'),
    ('Sarah Adams', 'Salesperson', 3000000.00, '2023-02-15', '083456789013', 'Jl. Employee B No.2'),
    ('James Lee', 'Cashier', 2500000.00, '2023-03-20', '083456789014', 'Jl. Employee C No.3'),
    ('Emily Clark', 'Technician', 4000000.00, '2023-04-10', '083456789015', 'Jl. Employee D No.4'),
    ('Michael Scott', 'Supervisor', 4500000.00, '2023-05-25', '083456789016', 'Jl. Employee E No.5');

INSERT INTO Sales (sale_date, sale_total, customer_id, employee_id)
VALUES
    ('2024-12-01', 6000000.00, 1, 1),
    ('2024-12-02', 2500000.00, 2, 2),
    ('2024-12-03', 1200000.00, 3, 3),
    ('2024-12-04', 3000000.00, 4, 4),
    ('2024-12-05', 8000000.00, 5, 5);

INSERT INTO Sale_Items (sale_id, product_id, quantity_sold, product_price)
VALUES
    (1, 1, 1, 5000000.00),
    (2, 2, 1, 2000000.00),
    (3, 3, 2, 150000.00),
    (4, 4, 1, 100000.00),
    (5, 5, 3, 80000.00);
```

- Melakukan report (laporan)<br>
`Melihat total penjualan berdasarkan tanggal`
![Imgur](https://i.imgur.com/DDQ0cUl.png)
