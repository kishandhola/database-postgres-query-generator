# PostgreSQL Database Query Generator

This Python script connects to a PostgreSQL database, retrieves all tables, and generates SQL queries (INSERT, UPDATE, DELETE, and SELECT) for each table. The queries are dynamically constructed based on the table schema and written to individual text files.

## Features

- Connect to a PostgreSQL database using user-provided credentials.
- Retrieve all tables in the public schema.
- Generate SQL queries for each table.
  - INSERT query
  - UPDATE query
  - DELETE query
  - SELECT query (all fields)
  - SELECT query (specific fields)
- Save the generated queries to text files.

## Requirements

- Python 3.x
- `psycopg2` library

## Installation

1. Install Python 3.x from [python.org](https://www.python.org/).
2. Install `psycopg2` library using pip:
   ```sh
   pip install psycopg2
   ```

## Usage

1. Clone the repository or download the script.
2. Run the script:
   ```sh
   python generate_pg_sql_queries.py
   ```
3. Enter the database connection details when prompted.
4. The script will create a folder with the database name and generate text files with the queries for each table.

## Detailed Description

### Database Connection

The script starts by prompting the user for the database connection details (host, user, password, database). It then connects to the PostgreSQL database using the `psycopg2` library.

### Folder Creation

The `create_folder_if_not_exists` function ensures that a folder named after the database exists in the current directory. If the folder does not exist, it creates it.

### Data Type Handling

The `requires_quotes` function determines whether a given data type requires quotes in SQL queries. It returns `True` for types like `character varying`, `text`, `timestamp`, and `date`, and `False` for numeric types like `integer`, `bigint`, `numeric`, and `double precision`.

### Primary Key Retrieval

The `get_primary_key` function retrieves the primary key column name of a given table using a query on the `information_schema` tables.

### Query Generation

For each table in the database:
- The script retrieves the column names and data types.
- Constructs field names and value strings for the INSERT query.
- Constructs the SET part and the condition for the UPDATE query.
- Constructs the condition for the DELETE and SELECT queries.
- Prints the generated queries.
- Writes the queries to a text file named after the table in the database folder.

## Examples

### Example Database

Consider a database named `example_db` with two tables: `users` and `orders`.

#### `users` Table

| id  | name   | email          |
|-----|--------|----------------|
| 1   | Alice  | alice@mail.com |
| 2   | Bob    | bob@mail.com   |

#### `orders` Table

| order_id | user_id | amount |
|----------|---------|--------|
| 1        | 1       | 100.50 |
| 2        | 2       | 200.00 |

### Generated Queries

For the `users` table, the generated queries would look like this:

**`example_db/users.txt`**:
```
Insert Query
INSERT INTO users (id, name, email) VALUES (${id}, '${name}', '${email}');

Update Query
UPDATE users SET name = '${name}', email = '${email}' WHERE id = ${id};

Delete Query
DELETE FROM users WHERE id = ${id};

Select All Field Query
SELECT * FROM users WHERE id = ${id};

Select Specific Field Query
SELECT id, name, email FROM users WHERE id = ${id};
```

For the `orders` table, the generated queries would look like this:

**`example_db/orders.txt`**:
```
Insert Query
INSERT INTO orders (order_id, user_id, amount) VALUES (${order_id}, ${user_id}, ${amount});

Update Query
UPDATE orders SET user_id = ${user_id}, amount = ${amount} WHERE order_id = ${order_id};

Delete Query
DELETE FROM orders WHERE order_id = ${order_id};

Select All Field Query
SELECT * FROM orders WHERE order_id = ${order_id};

Select Specific Field Query
SELECT order_id, user_id, amount FROM orders WHERE order_id = ${order_id};
```

## Contributing

Contributions are welcome! Please fork the repository and submit a pull request.

## License

This project is licensed under the MIT License.