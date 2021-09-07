# PYODBC (Python Open Database Connectivity)

1. Definition : https://pypi.org/project/pyodbc/ OR https://github.com/mkleehammer/pyodbc
2. Documentation : https://github.com/mkleehammer/pyodbc/wiki
3. Release Notes : https://github.com/mkleehammer/pyodbc/releases

### ODBC / JDBC 
https://www.tutorialspoint.com/what-is-the-difference-between-odbc-and-jdbc

### What is Driver?
A driver, or device driver, is a set of files that tells a piece of hardware how to function by communicating with a computer's operating system. All pieces of hardware require a driver, from your internal computer components, such as your graphics card, to your external peripherals, like a printer.  
Link to read in detail regarding driver : https://www.hp.com/us-en/shop/tech-takes/what-are-computer-drivers

### ODBC Driver for Microsoft SQL Server
https://docs.microsoft.com/en-us/sql/connect/python/pyodbc/python-sql-driver-pyodbc?view=sql-server-ver15

### ODBC Driver for Oracle 
https://www.devart.com/odbc/oracle/docs/python.htm

### All about Cursors?
https://github.com/mkleehammer/pyodbc/wiki/Cursor  
https://code.google.com/archive/p/pyodbc/wikis/Cursor.wiki

### Example : Connection with Oracle
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

#Loop through all the drivers we have access to
# for driver in pyodbc.drivers():
#     print(driver) 

cursor.execute("SELECT * FROM EMP")  #this will return an object 

#Fetching table data
for row in cursor:
    print(row)

#Printing table data using while loop
# row = cursor.fetchone()
# while row in cursor:
#     print(row) 
#     row = cursor.fetchone()
cursor.close()
cnxn.close()
```

