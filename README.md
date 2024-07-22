# Devops Project
This project involves deploying a web application using container technologies on AWS or Azure cloud platforms. The web application, named my-app, utilizes PostgreSQL as the database server and Gradle as the build tool.

## Features
### Web Application 
Developed using Spring Boot, with dependencies including Lombok, Spring Web, Thymeleaf, Spring Data JPA, and PostgreSQL Driver.
### Database 
PostgreSQL with a table named person containing the following columns:</br>
id (int)</br>
name (varchar(16))</br>
address (varchar(32))</br>
img_url (varchar(1024))
## Views
**Show all people:** Displays all records in the person table.</br>
**CRUD Operations:** Buttons for adding, updating, and deleting records.</br>
**Image Upload:** Supports image file uploads with a max size of 5 MB, stored locally in /resources/static/images during development and in S3 (AWS) or Blob Storage (Azure) in production.
## Deployment Steps
### Local Development:

Use Gradle to build a jar file.</br>
Use Spring Boot for development with properties set in application.properties.
### Containerization:

Create a Dockerfile for the application.</br>
Create a docker-compose.yml file for my-app, my-db, and my-net.
### Cloud Deployment:

Push the containerized app to Docker Hub.</br>
Create a Linux instance on AWS or Azure with proper security configurations.</br>
Install Docker and Nginx on the cloud instance.</br>
Pull the latest application image from Docker Hub.</br>
Start the application using docker-compose.
## Jenkins CI/CD Pipeline
### Pipeline Setup:

Use Jenkins to create a declarative pipeline.</br>
Set up Jenkins to create a new jar file, build a Docker image, push the image to Docker Hub, pull the image to the cloud, and start the application automatically on a Kubernetes cluster.
### Jenkinsfile Stages:

Create jar file upon update or merge.</br>
Build Docker image.</br>
Push Docker image to Docker Hub.</br>
Pull the updated image from Docker Hub to the cloud.</br>
Run containerized applications (webapp and db) on the cloud.
## Configuration
The web application and database server accept external parameters for DB and HTTP port configurations, ensuring no need for recompilation during local and cloud testing.</br>
The application and its pages must be accessible from the public IP of the cloud instance.
