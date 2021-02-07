# Install MySQL + Workbench
- https://dev.mysql.com/downloads/installer/
- https://dev.mysql.com/doc/refman/8.0/en/installing.html

# Create Schema
- create database db_example; -- Creates the new database;
- create user 'springuser'@'%' identified by 'ThePassword'; -- Creates the user
- grant all on db_example.* to 'springuser'@'%'; -- Gives all privileges to the new user on the newly created database

# List all employees if application created anything
- select * from employee;

# Create new employee
- curl -v -X POST -H 'Content-type:application/json' -d '{"name": "Samir Shaik", "role": "Technical Architect"}' localhost:8080/employees

# List all employees
- curl -v localhost:8080/employees | json_pp

# List employee with id=1
- curl -v -X DELETE localhost:8080/employees/1

# Update employee with id=1
- curl -v -X PUT -H 'Content-type:application/json' -d '{"name": "Samir Hymed Shaik", "role": "Technical Architect"}' localhost:8080/employees/1

# Start application
- mvn clean spring-boot:run

