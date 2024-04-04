import mysql.connector

# Establish a connection to the MySQL database
db = mysql.connector.connect(
    host="localhost",
    user="your_username",
    password="your_password",
    database="your_database"
)

# Create a cursor object to execute SQL queries
cursor = db.cursor()

# Create the table
create_table_query = """
CREATE TABLE IF NOT EXISTS student_marks (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    roll_number VARCHAR(20),
    mark FLOAT
)
"""
cursor.execute(create_table_query)

# Insert values (you can prompt the user for input)
insert_query = """
INSERT INTO student_marks (name, roll_number, mark)
VALUES (%s, %s, %s)
"""
values = [("Alice", "101", 85),
          ("Bob", "B102", 78),
          ("Charlie", "C103", 92)]
cursor.executemany(insert_query, values)
db.commit()

# Calculate average, total, sum, and least mark
cursor.execute("SELECT AVG(mark) FROM student_marks")
average_mark = cursor.fetchone()[0]

cursor.execute("SELECT SUM(mark) FROM student_marks")
total_mark = cursor.fetchone()[0]

cursor.execute("SELECT MIN(mark) FROM student_marks")
least_mark = cursor.fetchone()[0]

print(f"Average Mark: {average_mark:.2f}")
print(f"Total Mark: {total_mark:.2f}")
print(f"Least Mark: {least_mark:.2f}")

# Close the cursor and database connection
cursor.close()
db.close()
