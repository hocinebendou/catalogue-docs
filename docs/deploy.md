# Catalogue application deployement

The H3Africa catalogue is a Java web based application written with [Spring Boot](https://projects.spring.io/spring-boot/) 
framework. Contrary to Spring, Spring Boot alleviates the application configuration complexity, a known drawback of 
Spring framework.

In what follow we describe how to deploy the catalogue in an Ubuntu virtual machine (VM).

## Download the source:

!!!Note
    The deployment procedure is for system administrators or users with Unix knowledge i.e, knowing how to work 
    with command lines via Terminal.
    
Access the VM as user with root or normal privileges. Normally, it is done via `ssh` command line. You may be prompt 
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

To verify if the installation worked perfectly, the following executable is available and you can run it in your terminal
    
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
    
## Add an admin user of the catalogue

