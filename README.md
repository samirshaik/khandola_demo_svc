# Install MySQL + Workbench
- https://dev.mysql.com/downloads/installer/
- https://dev.mysql.com/doc/refman/8.0/en/installing.html

# Download and install putty
- https://www.putty.org/

# Download and install Git
- https://git-scm.com/downloads

# How to connect to EC2 instance
- ssh -i khandola-demo-key.pem ec2-user@54.167.10.6
- convert .pem to .ppk using puttygen and follow instruction in the video to connect to running ec2 instance

# Install Tomcat 9 on EC2 instance
- https://linuxize.com/post/how-to-install-tomcat-9-on-centos-7/

# Create Schema
- create database khandola_db; -- Creates the new database;
- create user 'springuser'@'%' identified by 'ThePassword'; -- Creates the user
- grant all on khandola_db.* to 'springuser'@'%'; -- Gives all privileges to the new user on the newly created database

# Create Tabl
```
CREATE TABLE `khandola_db`.`employee` (
`id` INT NOT NULL,
`name` VARCHAR(255) NULL,
`role` VARCHAR(255) NULL,
PRIMARY KEY (`id`));
```
# Insert record
- INSERT INTO `khandola_db`.`employee` (`id`, `name`, `role`) VALUES ('1', 'Samir', 'Architect');

# List all employees if application created anything
- select * from employee;

# Create new employee
- curl -v -X POST -H 'Content-type:application/json' -d '{"name": "Samir Shaik", "role": "Technical Architect"}' localhost:8080/khandola-demo-svc/employees

# List all employees
- curl -v localhost:8080/employees | json_pp

# List employee with id=1
- curl -v -X DELETE localhost:8080/employees/1

# Update employee with id=1
- curl -v -X PUT -H 'Content-type:application/json' -d '{"name": "Samir Hymed Shaik", "role": "Technical Architect"}' localhost:8080/employees/1

# Start application
- mvn clean spring-boot:run