For basic ideas on Docker and its environments, here is the wiki page for the same describing the fundamentals : <a href="https://github.com/dikshita-git/RP_Ingress_security-IPv4_and_IPv6/wiki/Docker-%7C-Its-Environments">Read here</a>
-------------------------------------------------------------------------------------

This is a README file for the basic demo conducted to deploy a wordpress using docker-compose without using Traefik.Below is the description of each step in the docker-compose.yml.

version: "3" -------------------------   Docker-compose version

services:    -------------------------   Defines service names we want to run

db:          -------------------------   Name of the first service

    image: mysql:5.7------------------   To run the service "db", we need a mysql image from Docker Hub. Here we specified a version of 5.7
    
    volumes:--------------------------   We define the container volumes. This will map the MySQL container data to the volumes you created.
    
      - db_data:/var/lib/mysql
      
      restart: always-----------------   In case the container stops running for any reason, set it to restart. If the server reboots, the container restarts.
      
    environment:----------------------   Consists of database name, password, database username & password.WP will use these environment variables to connect to MySQL container.
    
      MYSQL_ROOT_PASSWORD: somewordpress
      
      MYSQL_DATABASE: wordpress
      
      MYSQL_USER: wordpress
      
      MYSQL_PASSWORD: wordpress
      
    
  wordpress:--------------------------    Name of the second service
  
    depends_on:-----------------------    It is to make sure that a container only starts when the services it depends on are online. 
    
               -----------------------    Since WordPress relies on the MySQL container, therefore, we specify the dependencies as below
               
      - db
      
    image: wordpress:latest-----------    To run the service "wordpress", we need wordpress image from Docker Hub
    
    volumes:
    
      - wordpress_data:/var/www/html
      
    ports:----------------------------    Here we set the Container port.
    
          ----------------------------    Since the Wordpress image on Apache where Apache runs on port no. 80, so here we have to specify the same port no. on Which Apache runs           
          ----------------------------    This port 80 is mapped to port no. 8000 of the local machine
      
      - "8000:80"
      
    restart: always
    
    environment:----------------------    Here we declare the  WordPress environment variables. 
    
                ----------------------    In order to run a wordpress container, we should declare and set the database environments which the  WordPress will utilize.
               
               ----------------------    These variables include the WP database host, WP database user name, database user password, % the database name defined in the MySQL service (first service in our case) environments.
      WORDPRESS_DB_HOST: db
      
      WORDPRESS_DB_USER: wordpress
      
      WORDPRESS_DB_PASSWORD: wordpress
      
      WORDPRESS_DB_NAME: wordpress
      
volumes:------------------------------     Declare top level volume which defines MySQL as declared in db_data & WP as declared in wordpress_data(1st, 2nd service's volumes)
 
 db_data: {}
 
 wordpress_data: {}
