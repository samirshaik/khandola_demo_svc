# Install MySQL + Workbench
- https://dev.mysql.com/downloads/installer/
- https://dev.mysql.com/doc/refman/8.0/en/installing.html

# Download and install putty
- https://www.putty.org/

# Download and install Git
- https://git-scm.com/downloads

# Create EC2 instance
- Create EC2 instance using AWS management console

# Connect to EC2 instance
You have 2 ways to connect to the newly commissioned EC2 instance. 
1. If you have GIT Bash use the following command:
   - ssh -i khandola-demo-key.pem ec2-user@54.167.10.6

2. If you would like to use Putty instead, make sure you convert .pem to .ppk using PuttyGen and follow instruction in the video to connect to running ec2 instance

# Install Tomcat 9 on EC2 instance
Once you are connected to the EC2 instance you can use following commands to install Tomcat9 on the instance.
```
sudo yum install -y java-1.8.0-openjdk.x86_64

sudo groupadd --system tomcat

sudo useradd -d /usr/share/tomcat -r -s /bin/false -g tomcat tomcat

export TOMCAT_VER="9.0.39"

sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v${TOMCAT_VER}/bin/apache-tomcat-${TOMCAT_VER}.tar.gz

sudo tar xvf apache-tomcat-${TOMCAT_VER}.tar.gz -C /usr/share/

sudo ln -s /usr/share/apache-tomcat-${TOMCAT_VER}/ /usr/share/tomcat

sudo chown -R tomcat:tomcat /usr/share/tomcat

sudo chown -R tomcat:tomcat /usr/share/apache-tomcat-${TOMCAT_VER}/ 

sudo tee /etc/systemd/system/tomcat.service<<EOF
[Unit]
Description=Tomcat Server
After=syslog.target network.target

[Service]
Type=forking
User=tomcat
Group=tomcat

Environment=JAVA_HOME=/usr/lib/jvm/jre
Environment='JAVA_OPTS=-Djava.awt.headless=true'
Environment=CATALINA_HOME=/usr/share/tomcat
Environment=CATALINA_BASE=/usr/share/tomcat
Environment=CATALINA_PID=/usr/share/tomcat/temp/tomcat.pid
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M'
ExecStart=/usr/share/tomcat/bin/catalina.sh start
ExecStop=/usr/share/tomcat/bin/catalina.sh stop

[Install]
WantedBy=multi-user.target
EOF


sudo systemctl enable tomcat
```
# Setup Aurora Serverless
- https://aws.amazon.com/getting-started/hands-on/building-serverless-applications-with-amazon-aurora-serverless/

# Connect to Aurora from Local
- You basically need to spin up a new EC2 instance and then use the MySQL SSH server
- https://medium.com/misfit-technologies/connecting-to-a-serverless-aurora-database-locally-cd479cd5aa42
- 
# Create Schema
```
create database khandola_db; -- Creates the new database;
create user 'khandolauser'@'%' identified by 's3cr3t2021'; -- Creates the user
grant all on khandola_db.* to 'khandolauser'@'%'; -- Gives all privileges to the new user on the newly created database
```

# Create Table
```
CREATE TABLE `khandola_db`.`employee` (
`id` INT NOT NULL,
`name` VARCHAR(255) NULL,
`role` VARCHAR(255) NULL,
PRIMARY KEY (`id`));
```
# Insert record in the table
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
