# Intro

## Docker Desktop Installation
- download and install [docker desktop](https://www.docker.com/products/docker-desktop/)
- create an account in [docker hub](https://hub.docker.com/signup)
- verify installation by opening terminal and running
```
docker run --rm hello-world
```
- remove the testing image
```
docker rmi hello-world:latest
```

## Postgres

### Container Interaction

#### Show Running Containers
```
docker ps
```

#### Show All Containers
```
docker ps -a
```

#### Create a container
```
docker create --name my-postgres -e POSTGRES_USER=root -e POSTGRES_PASSWORD=pass -p 5432:5432 postgres:16
```

#### Start a container
```
docker start my-postgres
```

#### Stop a container
```
docker stop my-postgres
```

#### Connect to the container
```
docker exec -it my-postgres bash
```

#### Exit from the container
```
exit
```

#### Delete a container
```
docker rm my-postgres
```
_By deleting a container all its data will get lost._

### Database Interaction

#### Connect to the database engine
```
psql -U root
```

#### Basic Commands
- Show databases `\l`
- Create a database `create database demo;`
- Connect to a database `\c demo`
- Drop a database `drop database demo;` _not in prodcuction if possible_
- Exit from database `exit` or `\q`

### Connect with Beekeeper
- Make sure that the container is started
- Connect with the URL `postgres://root:pass@localhost:5432/demo`

### Tech Company example

#### Create a new database
```
create database tech_company;
```

#### Create customers table
```
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255) UNIQUE,
    phone_number VARCHAR(20)
);
```

#### Add customers data
```
INSERT INTO customers (name, email, phone_number) VALUES
('John Doe', 'john.doe@example.com', '123-456-7890'),
('Jane Smith', 'jane.smith@example.com', '234-567-8901'),
('Alice Johnson', 'alice.johnson@example.com', '345-678-9012'),
('Bob Brown', 'bob.brown@example.com', '456-789-0123'),
('Charlie White', 'charlie.white@example.com', '567-890-1234'),
('David Green', 'david.green@example.com', '678-901-2345'),
('Emily Black', 'emily.black@example.com', '789-012-3456'),
('Frank Red', 'frank.red@example.com', '890-123-4567'),
('Grace Blue', 'grace.blue@example.com', '901-234-5678'),
('Helen Yellow', 'helen.yellow@example.com', '012-345-6789');
```

#### Create orders table
```
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    product_name VARCHAR(255),
    quantity INT,
    price DECIMAL(10, 2),
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

#### Add orders data
```
INSERT INTO orders (product_name, quantity, price, customer_id) VALUES
('iPhone 13 Pro Max', 1, 999.00, 1),
('Samsung Galaxy S21 Ultra', 2, 1199.99, 2),
('MacBook Air M1', 1, 1299.00, 3),
('Dell XPS 15', 2, 1499.99, 4),
('Google Pixel 6 Pro', 1, 799.00, 5),
('Microsoft Surface Laptop 4', 2, 1099.99, 6),
('iPad Pro 12.9 inch', 1, 799.00, 7),
('Amazon Fire HD 10 Tablet', 2, 99.99, 8),
('PlayStation 5', 1, 499.99, 9),
('Xbox Series X', 2, 499.99, 10),
('Nikon Z6 Mirrorless Camera', 1, 1399.95, 1),
('Canon EOS R5 DSLR Camera', 2, 2499.99, 2),
('GoPro Hero 10', 1, 399.99, 3),
('Sony WH-1000XM4 Headphones', 2, 349.99, 4),
('Apple Watch Series 7', 1, 399.00, 5),
('Fitbit Versa 3 Health Tracker', 2, 199.99, 6),
('Logitech MX Master 3 Mouse', 1, 99.99, 7),
('JBL Bar 5.1 Soundbar', 2, 299.99, 8),
('Bose QuietComfort Earbuds', 1, 279.99, 9),
('Sonos One Smart Speaker', 2, 199.99, 10),
('Nintendo Switch OLED Model', 1, 349.99, 1),
('Steam Deck Gaming Handheld', 2, 519.99, 2),
('ASUS ROG Zephyrus G14', 1, 1599.99, 3),
('Lenovo ThinkPad X1 Carbon Gen 9', 2, 1699.99, 4),
('HP Spectre x360', 1, 1299.99, 5),
('Acer Predator Helios 500', 2, 899.99, 6),
('Alienware m15 R4', 1, 1799.99, 7),
('MSI GE Raider RGB', 2, 1499.99, 8),
('Corsair iCUE Elite Capellix RGB Keyboard', 1, 149.99, 9),
('SteelSeries Arctis Pro Wireless Gaming Headset', 2, 199.99, 10);
```

#### Create dump file - backup

##### From the host machine
```
docker exec -it database pg_dumpall -c -U root > dump.sql
```
_A common naming convention is to include a timestamp in the dump file to make sure that it is unique dump_$(date +%Y-%m-%d"_"%H_%M_%S).sql_

#### Restore data

##### Create a new database container
```
docker create --name temp-database -e POSTGRES_USER=root -e POSTGRES_PASSWORD=pass -p 5431:5432 postgres:16
```
_remember to use a different port, in this case I used 5431_

##### Start the database container
```
docker start temp-database
```

##### Copy the dump file inside the new container
```
docker cp dump.sql temp-database:/backup.sql
```

##### Connect to the container
```
docker exec -it temp-database bash
```

##### Restore the database
```
psql -U root -f /backup.sql
```
