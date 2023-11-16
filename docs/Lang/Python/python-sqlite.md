# Python - SQLite Database course

`2023-02-26` - Found some videos to Watch
Ref = <https://www.youtube.com/watch?v=byHcYRpMgI4>

`2023-05-15` - Found a nice article on Join
<https://www.geeksforgeeks.org/python-sqlite-join-clause/>

Related project now archived **[Project Archive](./python-sqlite/python-sqlite.7z)**

## Create the Environment on Windows

```batch
python -m venv venv
venv\Scripts\activate
```

## Creating a Database

```py
import sqlite3  # It comes built In to Python No need anything Extra

# Create Connection to Database - Here its a file
conn = sqlite3.connect(__file__ + '.db')
# Close the Database
conn.close()
```

This would open a file with the same name as that of the python program.

We can also create a in memory database for doing temporary work.

```py
import sqlite3
# This is memory connection Database it would ge erased after program exit
conn_mem = sqlite3.connect(':memory:')
# Close the Database
conn_mem.close()
```

## Creating tables in a Database

```py
import sqlite3
conn = sqlite3.connect(__file__ + '.db')

# We need to Create a Cursor to do things
c = conn.cursor()

# Create a Table
try:
    c.execute("""--sql
        CREATE TABLE customers (
            first_name TEXT,
            last_name TEXT,
            email TEXT
        )
            """)
# There are 5 datatypes supported in Sqlite
# NULL, INTEGER, REAL, TEXT, BLOB
# NULL - Boolean kind Exists or No Exists
# INTEGER - Whole numbers
# REAL - Decimals (Currency etc.)
# TEXT - Strings
# BLOB - Images, MP3file etc.
except sqlite3.OperationalError:
    print(" Table already Exists !!")

# Save the Data of all transactions
conn.commit()
conn.close()
```

The `cursor()` creates a way to talk to the database and perform transactions.

To execute an SQL query we use the `execute()` function on the cursor created for the database connection.

We use the `"""` for creating a multi-line SQL query.
Note that the `--sql` helps to add SQL highlighting in the editor.
Since in SQL `--` is used to create comments, it does not effect the other part of the SQL code.

Finally we use `commit()` function to save the configuration to a database.
This needs to be done, each time we are altering the database.

We are also accepting errors based on the exceptions raised by the particular function.
In this specifically the `execute`, since we are creating a table - it might already exist.
Hence, we try to catch that exception.

## Add data to Table

```py
... continuing from earlier table clreation

# Insert One Record
try:
    c.execute("""--sql
        INSERT INTO customers VALUES ('Mohit', 'Khatri', 'mohitk@gmail.com')
        """)
except sqlite3.OperationalError:
    print("Problem happened here - may be table does not exists !")

# Insert Multiple Records
many_customers = [
    ('Jevan', 'Khatri', 'jevank@gmail.com'),
    ('Swati', 'BL', 'blswati@gmail.com'),
    ('Radhika', 'T', 'radhika@msn.com'),
]
try:
    c.executemany("""--sql
    INSERT INTO customers values (?,?,?)
              """, many_customers)
except sqlite3.OperationalError:
    print("Problem with Inserting data !")

... usual exit sequence
```

While creating the table `customers` we added 3 fields `first_name`, `last_name`, `email`.
Hence we are including the 3 fields.

The first example shows adding a single record and the second one shows how to add multiple records at the same time.

## Basic query into the Database

Here is some pre-requisite code to create the data first:

```py
import sqlite3
import os

os.remove(__file__ + '.db')
conn = sqlite3.connect(__file__ + '.db')

c = conn.cursor()

try:
    c.execute("""--sql
        CREATE TABLE customers (
            first_name TEXT,
            last_name TEXT,
            email TEXT
        )
            """)
except sqlite3.OperationalError:
    print(" Table already Exists !!")

many_customers = [
    ('Mohit', 'Khatri', 'mohitk@gmail.com'),
    ('Jevan', 'Khatri', 'jevank@gmail.com'),
    ('Swati', 'BL', 'blswati@gmail.com'),
    ('Radhika', 'T', 'radhika@msn.com'),
    ('Swami', 'N', 'swaminath@gmail.com'),
]
try:
    c.executemany("""--sql
    INSERT INTO customers values (?,?,?)
              """, many_customers)
except sqlite3.OperationalError:
    print("Problem with Inserting data !")

conn.commit()
```

Now lets perform query to fetch data from the above connection.

```py
# Query the DB
c.execute("""--sql
    SELECT * FROM customers
          """)
# Print the All together Result
print(f"\nAll of them: \n {c.fetchall()}")

# Query the DB
c.execute("""--sql
    SELECT * FROM customers
          """)
# Only one record
print(f"\nOnly One : \n {c.fetchone()}")

# Query the DB
c.execute("""--sql
    SELECT * FROM customers
          """)
# Only first 3 records
print(f"\nFirst two: \n {c.fetchmany(2)}")

# Query the DB
c.execute("""--sql
    SELECT * FROM customers
          """)
print("\nFormatted Printing:\n")
items = c.fetchall()
# Print Fields
for i in items:
    print(f"{i[0]:10} {i[1]:4} | {i[2]}")

conn.close()
```

Above are the examples on first how to query for data and then play back of the data in different ways.

Note that the same cursor is used over and over again to query and insert data.

### Read the hidden primary key as well

Typically while creating a table there is also an `id` field created used to index the table.
We can read that as well. This field increments monotonously and is always guaranteed to be unique.

```py
... usual prerequsite data creation

# Query with ID
c.execute("""--sql
    SELECT rowid, * FROM customers
          """)
# Print the Values
items = c.fetchall()

for i in items:
    print(i)

conn.close()
```

## Specific type Queries

We can filter out the specific records based on matching criteria using the `WHERE` clause.

```py
... usual prerequsite data creation

# Query the DB using WHERE clause
c.execute("""--sql
    SELECT * FROM customers WHERE first_name = 'Swati'
          """)
# Print the All together Result
print(f"\nAll of them: \n {c.fetchall()}")

conn.close()
```

Though we are using `fetchall()` we would only get one record here as per the query.

We can also form conditions for number fields like `<= >= > < .etc.`.

### Wild card kind of query

```py
... usual prerequsite data creation

# Query the DB using WHERE clause and LIKE wiled card
c.execute("""--sql
    SELECT * FROM customers WHERE first_name LIKE 'Sw%'
          """)
# Print the All together Result
print(f"\nAll of them: \n {c.fetchall()}")

conn.close()
```

The use of the `LIKE` allows us to use the wild card character `%` percentage.

Hence the above would fetch records for both `Swami` and `Swati`.

Another one to identify all the `gmail.com` email IDs :

```py
... usual prerequsite data creation

# Query the DB using WHERE clause and LIKE wiled card
c.execute("""--sql
    SELECT * FROM customers WHERE email LIKE '%gmail.com'
          """)
# Print the All together Result
print(f"\nAll of them: \n {c.fetchall()}")

conn.close()
```

## Update records

```py
... usual prerequsite data creation

# Update a customer with specific name
c.execute("""--sql
    UPDATE customers SET last_name = 'Nath'
        WHERE email = 'swaminath@gmail.com'
          """)
conn.commit()

c.execute("SELECT rowid, * FROM customers")
items = c.fetchall()

for i in items:
    print(i)

conn.close()
```

This works but is not a good way to perform alteration.

Its better to use the `rowid` to perform the update.

```py
... usual prerequsite data creation

# Update a customer with specific name
c.execute("""--sql
    UPDATE customers SET last_name = 'Nath'
        WHERE rowid = 5
          """)
conn.commit()

c.execute("SELECT rowid, * FROM customers")
items = c.fetchall()

for i in items:
    print(i)

conn.close()
```

## Delete records

```py
... usual prerequsite data creation

# Delete a record using Rowid
c.execute("DELETE FROM customers WHERE rowid = 5")
conn.commit()

c.execute("SELECT rowid, * FROM customers")
items = c.fetchall()

for i in items:
    print(i)

conn.close()
```

## Ordering data

It helps to sort out the data in a particular order.

```py
... usual prerequsite data creation

# Execute the quer and use ORDER BY
c.execute("SELECT rowid, * FROM customers ORDER BY rowid")
items = c.fetchall()

for i in items:
    print(i)

conn.close()
```

The above example is the default way that the data is ordered while querying.
And the order is *ascending* order `ASC`.

We can reverse this to *descending* order `DESC` -

```py
... usual prerequsite data creation

# Execute the quer and use ORDER BY
c.execute("SELECT rowid, * FROM customers ORDER BY rowid DESC")
items = c.fetchall()

for i in items:
    print(i)

conn.close()
```

Another example using a Alphabetic field with Ascending order.

```py
... usual prerequsite data creation

# Execute the quer and use ORDER BY - Alphabetic Ascending
c.execute("SELECT rowid, * FROM customers ORDER BY last_name")
items = c.fetchall()

for i in items:
    print(i)

conn.close()
```

## Selection using conditions with AND / OR

```py
... usual prerequsite data creation

# Select using AND
c.execute("""--sql
    SELECT rowid, * FROM customers
      WHERE last_name LIKE 'Kh%' AND rowid = 2
      """)
items = c.fetchall()

for i in items:
    print(i)

conn.close()
```

This would display the record `('Jevan', 'Khatri', 'jevank@gmail.com')`.

However there are two records where `last_name` is `Khatri`.

Lets look at that :

```py
... usual prerequsite data creation

# Select using AND
c.execute("""--sql
    SELECT rowid, * FROM customers
      WHERE last_name LIKE 'Kh%' OR rowid = 2
      """)
items = c.fetchall()

for i in items:
    print(i)

conn.close()
```

This would show both records.

## Limiting Results

```py
... usual prerequsite data creation

# Select with LIMIT to only 3 records
c.execute("""--sql
    SELECT rowid, * FROM customers LIMIT 3
      """)
items = c.fetchall()

for i in items:
    print(i)

conn.close()
```

## Deleting a table

```py
... usual prerequsite data creation

# Drop the table
c.execute("DROP TABLE customers")
c.commit()

conn.close()
```

This would remove the table from the database.

## Copy data from one table to another

```py
import sqlite3
import os
# new Database
conn = sqlite3.connect('test.db')

c = conn.cursor()

# Main Table
c.execute("CREATE TABLE main (name TEXT,comment TEXT,date TEXT)")
conn.commit()
# Logging Table
c.execute("CREATE TABLE log (name TEXT,comment TEXT,date TEXT)")
conn.commit()

# Add data to the Main database
stuff = [
    ('Hari','I am here.','2023-03-23'),
    ('Gaurav', 'I need a friend', '2023-03-04'),
    ]
c.executemany("INSERT INTO main VALUES (?,?,?)",stuff)
conn.commit()

print(" Main Table ")
print("-------------")
# Display all records
c.execute("SELECT rowid, * from main")
items = c.fetchall()
for i in items:
    print(i)

# Copy one record to Log table
c.execute("INSERT INTO log SELECT * FROM main WHERE rowid = 1")
conn.commit()

print(" Log Table ")
print("-------------")
# Display all records
c.execute("SELECT rowid, * from log")
items = c.fetchall()
for i in items:
    print(i)

# Remove the Record from main table
c.execute("DELETE FROM main WHERE rowid = 1")
conn.commit()

print(" Main Table ")
print("-------------")
# Display all records
c.execute("SELECT rowid, * from main")
items = c.fetchall()
for i in items:
    print(i)

# Drop the Tables
c.execute("DROP TABLE main")
conn.commit()
c.execute("DROP Table log")
conn.commit()
# Close the Connection
conn.close()
# Clear the DB file
os.remove('test.db')
```

## Relating data using Foreign Keys

```py
import os
import os.path
import sqlite3
# Clean-up the file
if os.path.exists('test.db'): os.remove('test.db')
# Open the DB
conn = sqlite3.connect('test.db')
c = conn.cursor()
# Create a Table of Types
c.execute("""--sql
   CREATE TABLE IF NOT EXISTS types(
     id INTEGER UNIQUE PRIMARY KEY,
     type TEXT
   )""")
# NOTE We can't use the `rowid` for foreign key
#     Hence we need to use additional column to actually do it.
conn.commit()
# Add the Types - id field is auto generated as its the primary key.
stuff=[(None,'TODO'),(None,'PROJ'),(None,'REPEAT'),]
c.executemany("INSERT INTO types VALUES (?,?)",
              stuff)
conn.commit()
# Print the Types
print("  types Table  \n--------------------")
c.execute("SELECT * FROM types")
items = c.fetchall()
for i in items: print(i)
# Create table of Tasks
c.execute("""--sql
   CREATE TABLE IF NOT EXISTS tasks(
     id INTEGER UNIQUE PRIMARY KEY,
     type_id INTEGER,
     task TEXT,
     FOREIGN KEY (type_id) REFERENCES types(id)
   )""")
# We form the Relation here
conn.commit()
# Add Tasks
stuff=[
    (None,1,"Do Homework"),(None,3,"Wash Dishes"),
    (None,2,"Build a Plane"),(None,1,"Fetch the groceries list"),
]
c.executemany("INSERT INTO tasks VALUES (?,?,?)", stuff)
conn.commit()
# Display the Values with Inner Join
print(" tasks and types Inner Join\n-------------------------------")
c.execute("""--sql
   SELECT tasks.id, type, task FROM tasks
    LEFT JOIN types ON tasks.type_id = types.id
   """)
items = c.fetchall()
for i in items: print(i)
# Close
conn.close()
# Cleanup
os.remove('test.db')
```

!!! note
    It's better to keep the `id` field named after the corresponding table.
    Like `id` in table `tasks` should be `task_id`.
    This way there is no confusion on which table hosts which `id` field.

----
<!-- Footer Begins Here -->
## Links

- [Back to Python Hub](./README.md)
- [Back to Computer Programming Languages Hub](../README.md)
- [Back to Root Document](../../README.md)
