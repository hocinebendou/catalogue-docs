# Catalogue application deployment

The H3Africa catalogue is a Java web based application written with [Spring Boot](https://projects.spring.io/spring-boot/) 
framework. Contrary to Spring, Spring Boot alleviates the application configuration complexity, a known drawback of 
Spring framework.

In what follow we describe how to deploy the catalogue in an Ubuntu virtual machine (VM).

## Download the source:

!!!Note
    The deployment procedure is for system administrators or users with Unix knowledge i.e, knowing how to work 
    with command lines via Terminal.
    
Access the VM as a user with root or normal privileges. Normally, it is done via `ssh` command line. You may be prompted 
to enter a password if the access to the VM is protected.
    
    ssh hocine@catalogue.sanbi.ac.za

The catalogue is an open source application under **MIT** license. It is possible to fork it and make changes to it.
The source code is deposited on [github](https://github.com/h3abionet/h3acatalog.git). Download locally the source code

    git clone https://github.com/h3abionet/h3acatalog.git
    
A new folder `h3acatalog` will be created in the current location.

## Install Neo4j

[Neo4j](https://github.com/neo4j/neo4j.git) is a graph database management system, by contrast to relational databases 
working with static table i.e, the number and the name of the columns must be predefined before the creation, it is 
schemaless and store the information, after modeling it to nodes and relationships, as graphs.

Neo4j requires Java a priori installed. To install it run
    
    sudo apt install default-jre default-jre-headless 

To verify if the installation worked perfectly, check using a terminal if the following executable is available
    
    java
    
Assuming Java is installed, it is time to install Neo4j. Add the Neo4j repository to the VM keychain and to the list
of `apt` sources. Both commands needs root access.
    
    wget --no-check-certificate -O - https://debian.neo4j.org/neotechnology.gpg.key | sudo apt-key add -
    echo 'deb http://debian.neo4j.org/repo stable/' | sudo tee /etc/apt/sources.list.d/neo4j.list

Finally, update the repository and install Neo4j

    sudo apt update
    sudo apt install neo4j
    
Verify Neo4j is running, if not start it with the next command

    sudo systemctl status neo4j
    sudo systemctl start neo4j
    
## Create an administrator user in the graph database

Before any commitment with the catalogue application, an administrator account has to exist to fulfill some required 
tasks, e.g register new upcoming users.

!!!Note
    The registration form is accessible merely by administrator users. Its function is to add new accounts for known 
    users characterised by the following roles: Admin, Archive, Biorepository or Dbac. Therefore, there is no access 
    granted for normal users.
   
Neo4j provides a multitude of ways to interact with its database, via command line using `cypher-shell` tool or from 
browser at this URL `http://<your VM domain>:7474`. For simplicity, we will use the browser option for the remaining of this 
process. Minimum essential information are needed to create a user account, a username, an encrypted password and 
a specific role. There is a plenty of online services to encrypt a human readable password, e.g. 
[BCrypt Calculator](https://www.dailycred.com/article/bcrypt-calculator) or if preferred integrate a Java library 
to do the work [jBCrypt](https://github.com/jeremyh/jBCrypt), however it is beyond the scope of this documentation.
Open Neo4j in you preferred browser and run the following Cypher query
    
    CREATE (:NeoUser {username: 'admin', password: '$2a$09$KyQwV3Hv0Nu4jmP753i.FOB7nFfDF3SofT1MalcohIS4ZWyysnklK', role: 'ADMIN'})
    
The query creates a new node with the values defined in the proprieties between brackets, i.e., the admin user is created.

## Install Apache Maven

Download the latest version of Apache Maven to the `/opt` folder

    cd /opt/
    wget http://www-eu.apache.org/dist/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
    
Extract the downloaded archive and rename it to `maven`

    sudo tar -xvzf apache-maven-3.3.9-bin.tar.gz
    sudo mv apache-maven-3.3.9 maven
    
Setup the environment variables. Add the following lines to the `~/.bashrc` file
    
    export M2_HOME=/opt/maven
    export PATH=${M2_HOME}/bin:${PATH}
    
Save and close the file, then reload the `.bashrc`

    source ~/.bashrc
    
Run `mvn -version` to verify the installation has been successfully configured.

## Run the catalogue

From within the catalogue source code run the following command to start the application

    mvn spring-boot:run
    
In your preferred browser open a new tab and enter `http:/<your VM domain name>:8080` to start working with the admin user 
created before.