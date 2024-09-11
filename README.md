# hack-SQL-HiramMasso
repositorio para enviar el codigo SQL del hack 
------------------------------------------------------------------------------------------------------------------------------
H-1 y H-2:
create table countries(
  id_country serial primary key,
  name varchar(50) not null  
);

create table users(
 id_users serial primary key,
 id_country integer not null,
 email varchar(100) not null,
 name varchar (50) not null,
 foreign key (id_country) references countries (id_country)   
);

insert into countries (name) values ('argentina') , ('colombia'),('chile');
select * from countries;

insert into users (id_country, email, name) 
  values (2, 'foo@foo.com', 'fooziman'), (3, 'bar@bar.com', 'barziman'); 
  select * from users;
  
delete from users where email = 'bar@bar.com';
update users set email = 'foo@foo.foo', name = 'fooz' where id_users = 1;
select * from users;
select * from users inner join  countries on users.id_country = countries.id_country;

select u.id_users as id, u.email, u.name as fullname, c.name 
  from users u inner join  countries c on u.id_country = c.id_country;
------------------------------------------------------------------------------------------------------------------------------
H-3 y H-4:
create table countries(
  id_country serial primary key,
  name varchar(50) not null  
);

create table priorities(
  id_priority serial primary key,
  type_name varchar(50) not null  
);

create table contact_request(
  id_email serial primary key,
  id_country integer not null,
  id_priority integer not null,
  name varchar(50) not null,
  detail varchar(250) not null,
  physical_address varchar(250) not null,
  foreign key (id_country) references countries (id_country),
  foreign key (id_priority) references priorities (id_priority)
);

insert into countries (name) values ('Argentina'),('Colombia'),('Chile'),('Venezuela'),('España');
select * from countries;

INSERT INTO priorities (type_name) VALUES ('prioridad alta'),('prioridad media'),('prioridad baja');
select * from priorities;

INSERT INTO contact_request (id_country, id_priority, name, detail, physical_address) VALUES 
(1, 1, 'Hiram Masso', 'deposito de un millon de dolares xD', 'madrid'),
(2, 2, 'Adrian caraballo', 'cosas de profesor de programacion', 'caracas'),
(3, 3, 'raykel', 'ansiedad en los ojos', 'bogota');
select * from contact_request;

DELETE FROM contact_request WHERE id_email = (SELECT MAX(id_email) FROM contact_request);

UPDATE contact_request SET name = 'Hiram Masso Actualizado', detail = 'detalles actualizados (dos millones de dolares xD)'
WHERE id_email = (SELECT MIN(id_email) FROM contact_request);
------------------------------------------------------------------------------------------------------------------------------
H-5 y H-6:
create table countries(
  id_country serial primary key,
  name varchar(50) not null  
);

create table roles(
  id_role serial primary key,
  name varchar(50) not null  
);

create table taxes(
  id_tax serial primary key,
  percentage DECIMAL(5, 2) not null  
);

create table offers(
  id_offer serial primary key,
  status varchar(50) not null  
);

create table discounts(
  id_discount serial primary key,
  status varchar(50) not null,
  percentage DECIMAL(5, 2)
);

create table payments(
  id_payment serial primary key,
  type VARCHAR(50) not null  
);

CREATE TABLE customers(
  email VARCHAR(50) PRIMARY KEY,
  id_country INTEGER not null,
  id_role INTEGER not null,
  name VARCHAR(50) not null,
  age int CHECK (age >=0),
  password VARCHAR(255) not null,
  physical_address VARCHAR(255) not null,
  foreign key (id_country) references countries (id_country),
  foreign key (id_role) references roles (id_role)  
);

CREATE TABLE invoice_status(
  id_invoice_status serial PRIMARY KEY,
  status VARCHAR(50) not null
);

CREATE TABLE products(
  id_product serial PRIMARY KEY,
  id_discount INTEGER not null,
  id_offer INTEGER not null,
  id_tax INTEGER not null,
  name VARCHAR (50) not null,
  details VARCHAR (100) not null,
  minimum_stock INTEGER NOT NULL,
  maximum_stock INTEGER NOT NULL,
  price DECIMAL (10, 2) NOT NULL,
  price_with_tax DECIMAL (10, 2) NOT NULL CHECK (price_with_tax >= 0),
  foreign key (id_discount) references discounts (id_discount),
  foreign key (id_offer) references offers (id_offer),
  foreign key (id_tax) references taxes (id_tax)
);

CREATE TABLE products_customers(
  id_product integer not null,
  id_customer VARCHAR(50) not null,
  FOREIGN KEY (id_product) REFERENCES products (id_product),
  FOREIGN KEY (id_customer) REFERENCES customers (email)
);

CREATE TABLE invoices(
  id_invoice serial PRIMARY KEY,
  id_customer VARCHAR(50) not null,
  id_payment INTEGER not null,
  id_invoice_status INTEGER not null,
  date DATE not null,
  total_to_pay DECIMAL(10, 2) not null,
  FOREIGN KEY (id_customer) REFERENCES customers (email),
  FOREIGN KEY (id_payment) REFERENCES payments (id_payment),
  FOREIGN KEY (id_invoice_status) REFERENCES invoice_status (id_invoice_status)
);

CREATE TABLE orders(
  id_order serial PRIMARY KEY,
  id_invoice INTEGER not null,
  id_product INTEGER not null,
  detail VARCHAR(50) not null,
  amount INTEGER not null,
  price DECIMAL (10, 2) not null,
  FOREIGN KEY (id_invoice) REFERENCES invoices (id_invoice),
  FOREIGN KEY (id_product) REFERENCES products (id_product)
);

INSERT INTO countries (name) VALUES ('venezuela'), ('argentina'), ('chile');

INSERT INTO roles (name) values ('admin'), ('user'), ('manager');

insert into taxes (percentage) VALUES (10.0), (20.0), (30.0);

INSERT into offers (status) VALUES ('active'), ('expired'), ('pending');

INSERT INTO discounts (status, percentage) VALUES ('Active', 10.00), ('Expired', 5.50), ('Pending', 15.75);

INSERT into payments (type) VALUES ('Credit Card'), ('PayPal'), ('Bank Transfer');

INSERT INTO customers (email, id_country, id_role, name, age, password, physical_address) VALUES
('alfa@gmail.com', 1, 1, 'alfa', 30, 'password123', 'Maracay, calle 7'),
('beta@outlook.com', 2, 2, 'beta', 25, 'securepassword', 'Buenos Aires, avenida 2'),
('delta@hotmail.com', 1, 1, 'delta', 28, 'mypassword', 'Caracas, calle 1');

insert into invoice_status (status) VALUES ('pending'), ('paid'), ('canceled');

INSERT INTO products (id_discount, id_offer, id_tax, name, details, minimum_stock, maximum_stock, price, price_with_tax) VALUES
(1, 1, 1, 'Product A', 'Description of Product A', 10, 100, 50.00, 55.00),
(2, 2, 2, 'Product B', 'Description of Product B', 5, 50, 30.00, 32.50),
(3, 1, 1, 'Product C', 'Description of Product C', 8, 80, 20.00, 22.00);

INSERT INTO products_customers (id_product, id_customer) VALUES
(1, 'alfa@gmail.com'),
(2, 'beta@outlook.com'),
(3, 'delta@hotmail.com');

INSERT INTO invoices (id_customer, id_payment, id_invoice_status, date, total_to_pay) VALUES
('alfa@gmail.com', 1, 1, '2024-09-01', 150.00),
('beta@outlook.com', 2, 2, '2024-09-02', 200.50),
('delta@hotmail.com', 1, 1, '2024-09-03', 75.25);

INSERT INTO orders (id_invoice, id_product, detail, amount, price) VALUES
(1, 1, 'Product A - Description of Product A', 2, 50.00),
(2, 2, 'Product B - Description of Product B', 1, 30.00),
(3, 3, 'Product A - Description of Product C', 3, 50.00);

DELETE FROM orders WHERE id_invoice IN (
    SELECT id_invoice FROM invoices WHERE id_customer IN ('alfa@gmail.com', 'delta@hotmail.com')
);
DELETE FROM invoices WHERE id_customer IN ('alfa@gmail.com', 'delta@hotmail.com');
DELETE FROM products_customers WHERE id_customer IN ('alfa@gmail.com', 'delta@hotmail.com');
DELETE FROM customers WHERE email IN ('alfa@gmail.com', 'delta@hotmail.com');

UPDATE customers SET name = 'Beta Updated', physical_address = 'Nueva dirección, Buenos Aires'
WHERE email = 'beta@outlook.com';

UPDATE taxes SET percentage = percentage + 5.00;

UPDATE products SET price = price * 1.10, price_with_tax = price_with_tax * 1.10;
