This is a README file for the basic demo conducted to deploy a wordpress using docker-compose without using Traefik.Below is the description of each step in the docker-compose.yml.

version: "3" ------------   Docker-compose version

services:    ------------   Defines service names we want to run

db:          ------------   Name of the first service
    image: mysql:5.7-----   To run the service "db", we need amzsql image from Docker Hub. Here we specified a version of 5.7
    volumes:
      - db_data:/var/lib/mysql
