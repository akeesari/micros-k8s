# **Chapter-1: Building Containerized Microservices**

## Overview

Welcome to the first chapter of our book. In this chapter, we will begin our journey by understanding microservices & docker concepts, explaining the importance of containerization, and its significance in modern microservices application development.

Next, we will start building and containerizing microservices using various technologies such as .NET Core, React Js and Node.js. These hands-on labs are designed to guide you through the process of creating containerized websites, APIs, setting up databases in containers, and running external services in containers, showcasing diverse technologies used in modern web applications.

By the end of this chapter, you will have a solid foundation in docker, microservices development, and a range of programming tools and technologies. Whether you are an expert programmer or just starting out, this journey will help you with essential skills for building and deploying modern, containerized microservice applications.


## Hands-On Labs

Here is the high-level list of labs we will cover in Chapter 1:


**Lab-1: Getting Started with Microservices** - In this lab, we'll introduce you to the concept of microservices and explain their importance in modern application development. You'll gain a high-level understanding of microservices architecture and its benefits.

**Lab-2: Getting Started with Docker** - Here, we'll look into Docker, the modern containerization technology. You'll learn how to install docker, run your first container, and explore basic docker commands.

**Lab-3: Create your First Containerized Microservice with .NET Core** - This lab guides you through creating a microservice using .NET Core and containerizing it with Docker. You'll learn how to write Dockerfiles and build container images for .NET Core microservices.

**Lab-5: Create your Second Containerized Microservice with Node.js** - In this lab, we switch gears to Node.js and create another microservice. You'll containerize a Node.js-based microservice and understand the differences compared to .NET Core.

**Lab-6: Create your First Containerized Website using ASP.NET Core MVC** - Now, it's time to create a containerized website using ASP.NET Core MVC. You'll build a web application, package it as a Docker image, and run it as a container.

**Lab-7: Create your Second Containerized Website using React JS** - In this lab, we'll focus on front-end development by creating a React.js-based website. You'll containerize a React application and understand how to work with front-end containers.

**Lab-8: Create your First Database with SQL Server** - Databases are an essential part of microservices. In this lab, we'll set up a SQL Server database within a container. You'll learn how to create and connect to containerized databases.

**Lab-9: Create a Container for executing PostgreSQL Database scripts** - PostgreSQL is another popular database choice. This lab guides you through running PostgreSQL in a docker container and executing scripts. You'll understand how to work with different database engines within containers.

**Lab-10: Running Keycloak in a Docker Container** - External services play a importent role in microservices. In this lab, we'll run Keycloak, an identity and access management system, in a Docker container. You'll configure and interact with Keycloak within the containerized environment.

**Lab-11: Running Drupal in a Docker Container** - Continuing with external services, we'll set up Drupal, a content management system, in a Docker container. You'll explore how to work with content management systems within containers.

These hands-on labs provide a practical foundation for building and containerizing microservices. By the end of these labs, you'll have hands-on experience with various technologies and a clear understanding of how to create and run microservices and external services in containers. This knowledge will be invaluable as we progress through the chapters and explore more advanced microservices concepts and deployment strategies.


<!-- ## Labs categorized into three areas: -->

Labs in Chapter 1 are categorized into three areas:

- Creating Containerized Websites (Website Development)
- Setting Up Databases in Containers (Database Containers)
- Running External Services in Docker Containers (External Services)

Let's take a look into the high-level details of these topics here:


## Creating Containerized Websites

In the labs created within this category, you'll learn how to create containerized websites using technologies like ASP.NET Core, MVC, Node.js, and React.js.


**ASP.NET Core MVC:**

1. **Introduction to ASP.NET Core MVC**: We'll start by introducing you to ASP.NET Core MVC, a cross-platform framework for building web applications. You'll understand its role in creating dynamic web content.

2. **Setting Up an ASP.NET Core MVC Project**: We'll guide you through setting up a new ASP.NET Core MVC project.

3. **Containerization with Docker**: You'll learn how to package your ASP.NET Core MVC application as a Docker container. We'll provide guidance on creating a Dockerfile for your web application.

5. **Running the Containerized ASP.NET Core MVC Application**: You'll see how to run your containerized ASP.NET Core MVC application locally and understand how containers simplify deployment.

**React.js:**

1. **Introduction to React.js**: React.js is a popular JavaScript library for building user interfaces. We'll introduce you to React.js and explain its role in modern web development.

2. **Creating a React.js Application**: You'll learn how to create a React.js application from scratch. 

3. **Containerization with Docker**: Similar to ASP.NET Core MVC, we'll guide you through containerizing your React.js application. You'll create a Dockerfile for your web app.

4. **Running the Containerized React.js Application**: You'll see how to run your containerized React.js application locally. 

By the end of these labs, you'll have hands-on experience with ASP.NET Core MVC, Node.js and React.js, along with the knowledge of how to containerize web applications. These skills are essential as we move forward to deploy these containerized websites alongside microservices in later chapters. 


## Setting Up Databases in Containers

In the labs created within this category, we'll learn setting up databases within containers for microservices data storage. You'll learn how to create containerized database instances using SQL Server and PostgreSQL. Let's explore the process:

Microservices often rely on databases to store and manage data. Containerizing databases offers numerous advantages, such as isolation, portability, and versioning. In this section, we'll focus on two popular database systems: SQL Server and PostgreSQL.

**SQL Server:**

1. **Introduction to SQL Server**: We'll introduce you to SQL Server, a robust relational database management system (RDBMS) developed by Microsoft.

2. **Containerization with Docker**: You'll learn how to containerize SQL Server by pulling an official SQL Server Docker image from the Docker Hub.

3. **Running SQL Server in a Docker Container**: We'll guide you through running a SQL Server container, configuring database settings, and connecting to the containerized SQL Server instance.

4. **Data Management**: You'll explore data management tasks within a containerized SQL Server, such as creating databases, tables, and performing CRUD (Create, Read, Update, Delete) operations.

**PostgreSQL:**

1. **Introduction to PostgreSQL**: We'll introduce you to PostgreSQL, a powerful open-source relational database system known for its scalability and extensibility.

2. **Containerization with Docker**: You'll learn how to containerize PostgreSQL by pulling an official PostgreSQL Docker image from the Docker Hub.

3. **Running PostgreSQL in a Docker Container**: We'll guide you through running a PostgreSQL container, configuring database settings, and connecting to the containerized PostgreSQL instance.

4. **Data Management**: You'll explore data management tasks within a containerized PostgreSQL database, including creating databases, tables, and executing SQL queries.

By the end of these labs, you'll have hands-on experience with containerized SQL Server and PostgreSQL databases, understanding their role in microservices data storage. These skills are crucial as you proceed through the chapters, where microservices will interact with these containerized databases to retrieve and store data. 

## Running External Services in Containers

In the labs created within this category, we'll learn integration of external services into your microservices architecture. You'll learn how to run external services like Keycloak and Drupal in Docker containers, enhancing the capabilities of your microservices.


External services play a importent role in microservices architecture, providing essential functionalities such as authentication and content management. Containerizing these external services offers several advantages, including consistency and simplified deployment. In this section, we'll focus on two prominent external services: Keycloak and Drupal.

**Keycloak:**

1. **Introduction to Keycloak**: Keycloak is an open-source identity and access management system. We'll introduce you to Keycloak and explain its significance in microservices authentication.

2. **Containerization with Docker**: You'll learn how to containerize Keycloak by pulling an official Keycloak Docker image from the Docker Hub.

3. **Running Keycloak in a Docker Container**: We'll guide you through running a Keycloak container, configuring realms, users, and roles within the containerized Keycloak instance.

4. **Microservices Integration**: You'll understand how to integrate microservices with Keycloak for secure authentication and authorization. Keycloak serves as an identity provider for your microservices.

**Drupal:**

1. **Introduction to Drupal**: Drupal is a popular open-source content management system (CMS). We'll introduce you to Drupal and its role in managing content for microservices.

2. **Containerization with Docker**: You'll learn how to containerize Drupal by pulling an official Drupal Docker image from the Docker Hub.

3. **Running Drupal in a Docker Container**: We'll guide you through running a Drupal container, setting up a website, and managing content within the containerized Drupal instance.

4. **Microservices Integration**: You'll explore how to integrate microservices with Drupal for content management. Drupal can serve as a content source for your microservices.

By the end of these labs, you'll have hands-on experience with containerized Keycloak and Drupal instances, understanding how to integrate them seamlessly into your microservices ecosystem. These skills are essential as you proceed through the chapters, where microservices will rely on these external services for authentication, authorization, and content management.