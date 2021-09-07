# PYODBC (Python Open Database Connectivity)

1. Definition : https://pypi.org/project/pyodbc/ Or https://github.com/mkleehammer/pyodbc
2. Documentation : https://github.com/mkleehammer/pyodbc/wiki ( Contains Everything you are looking for )
3. Release Notes : https://github.com/mkleehammer/pyodbc/releases

### Difference b/w ODBC / JDBC ?
https://www.tutorialspoint.com/what-is-the-difference-between-odbc-and-jdbc

### What is Driver?
A driver, or device driver, is a set of files that tells a piece of hardware how to function by communicating with a computer's operating system. All pieces of hardware require a driver, from your internal computer components, such as your graphics card, to your external peripherals, like a printer.  
Link to read in detail regarding driver : https://www.hp.com/us-en/shop/tech-takes/what-are-computer-drivers
```
#Loop through all the drivers you have access to
for driver in pyodbc.drivers():
     print(driver) 
```     

### Connection
https://github.com/mkleehammer/pyodbc/wiki/Connection

### All about Cursors?
https://github.com/mkleehammer/pyodbc/wiki/Cursor  
https://code.google.com/archive/p/pyodbc/wikis/Cursor.wiki

### Common Error : Connection is busy with results for another hstmt 
This is caused by you have a opening result set open against the same connection. 
For example, you execute a SqlCommand to select all rows from a table, while the result set is not drained, you try to execute another SqlCommand using the same connection, you will hit this error message.

To solve this, you have two choices:
a. Make sure you read the rest data from the pending result set before you send the next SqlCommand.
b. Use MARS (Multiple Active ResultSet) connection setting to enable multiple active result set in a connection.

All about MARS : https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/enabling-multiple-active-result-sets

### Microsoft Setup pyodbc
https://docs.microsoft.com/en-us/sql/connect/python/pyodbc/python-sql-driver-pyodbc?view=sql-server-ver15
##### Download ODBC Driver for SQL Server
https://docs.microsoft.com/en-us/sql/connect/odbc/download-odbc-driver-for-sql-server?view=sql-server-ver15#download-for-windows


### Steps to performs ODBC connection for Oracle using pyodbc
https://www.devart.com/odbc/oracle/docs/python.htm

##### Example : Connection with Oracle
In the following exmaple I am establishing a connection with Oracle database & then inserting sample data into it & after that fetching the same.
```
import pyodbc 

#Defining Connection
cnxn = pyodbc.connect('DRIVER={Devart ODBC Driver for Oracle}; \
                       Host=localhost; \
                       Service Name=orcl; \
                       User ID=c##scott;Password=tiger')

#Sample Data to be inserted in a table 
emp_data =[
    [1,'Gurudutt Goswami','Team Lead',1220],
    [2,'Anurag Singh','Lead',1221],
    [3,'Deepak Singh','Associate',1222],
    [4,'Prasoon Verma','Programmer',1223],
    [5,'Prakhar Jain','Manager',1224],
]

#Defining a cursor using connection
cursor = cnxn.cursor()

#You can also direcly execute an insert Query by cursor.execute("Your Insert Query")
#insert_statement = "INSERT INTO EMP (EMPNO, ENAME, JOB1, MGR) VALUES (535, 'Scott123', 'Deputy Manager', 545)"

# ? are just placeholders
insert_statement = "INSERT INTO EMP (EMPNO, ENAME, JOB1, MGR) VALUES (?, ?, ?, ?);"

for row in emp_data:
    values = (row[0],row[1],row[2],row[3])
    cursor.execute(insert_statement,values)
cursor.execute("commit")

cursor.execute("SELECT * FROM EMP")  #this will return an object 

#Fetching table data
for row in cursor:
    print(row)

#Printing table data using while loop
# row = cursor.fetchone()
# while row in cursor:
#     print(row) 
#     row = cursor.fetchone()

#Closing the Connection & Cursor
cursor.close()
cnxn.close()
```

