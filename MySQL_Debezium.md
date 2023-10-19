# MySQL and Debezium Setup

## Overview
Before deploying Apache NiFi, set up a MySQL database with an extra layer called Debezium for Change Data Capture (CDC).

## Steps
1. **Run MySQL Container**: Run a MySQL container with Debezium.
    ```
    docker run -dit --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=debezium -e MYSQL_USER=mysqluser -e MYSQL_PASSWORD=mysqlpw debezium/example-mysql:1.6
    ```
    
2. **Exec into MySQL Container**: Log into the MySQL container to create a new database and table.
    ```
    docker exec -it '<container_id>' bash
    mysql -u root -p
    ```
    When prompted, enter the root password for MySQL (`debezium`).

3. **Create Database and Table**: Execute the following SQL commands to create a new database and table.
    ```
    CREATE DATABASE demo;
    use demo;
    CREATE TABLE bus_status (
        record_id INT NOT NULL AUTO_INCREMENT,
        id INT NOT NULL,
        routeId INT NOT NULL,
        directionId VARCHAR(40),
        predictable BOOLEAN,
        secsSinceReport INT NOT NULL,
        kph INT NOT NULL,
        heading INT,
        lat REAL NOT NULL, 
        lon REAL NOT NULL,
        leadingVehicleId INT,
        event_time DATETIME DEFAULT NOW(),
        PRIMARY KEY (record_id)
    );
    ```
