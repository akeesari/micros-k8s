# **Introduction**

Welcome to "Building Microservices with Containers: A Practical Guide." In today's rapidly growing technological landscape, the demand for scalable, flexible, and resilient software solutions is necessary. In response to this demand, the architecture of choice for many modern applications is microservices. Microservices enables the development of complex systems as a set of small, independently deployable services.

This book is your detailed guide to understanding and implementing microservices architecture using containerization technology, specifically Docker. Whether you're a regular application developer looking to adopt microservices or a new to the technology, this book will provide you with the knowledge and hands-on experience necessary to succeed in building scalable and maintainable applications.

## Why Microservices?

Before looking into the technical details, let's briefly explore why microservices have become the architecture of choice for many organizations. Microservices offer several advantages over traditional monolithic architectures, including:

- **Scalability:** Microservices allow individual components of an application to scale independently, enabling better resource utilization and improved performance.
- **Flexibility:** With microservices, teams can choose the most appropriate technology stack for each service, leading to greater flexibility and innovation.
- **Resilience:** Isolating services from each other reduces the impact of failures, making the overall system more resilient.
- **Continuous Delivery:** Microservices facilitate continuous delivery and deployment practices, enabling teams to release updates quickly and frequently.

## Why Containers?

While microservices offer numerous benefits, managing a large number of services can be challenging. This is where containerization comes into play. Containers provide lightweight, portable, and isolated environments for running applications, making it easier to package, deploy, and manage microservices at scale. Docker, one of the most popular containerization platforms, has revolutionized the way developers build, ship, and run applications.

## What You'll Learn

In this book, we'll start by covering the fundamentals of microservices architecture and Docker containerization. We'll then guide you through the process of building and deploying microservices using a variety of technologies, including .NET Core, Node.js, React.js, and SQL databases. Along the way, you'll learn how to:

- Containerize microservices using Docker.
- Orchestrate containers with Docker Compose.
- Implement authentication and authorization using Keycloak.
- Build web applications with popular frameworks like .NET Core MVC and React.js.
- Set up and manage databases within containers using SQL Server and PostgreSQL.
- Deploy and scale microservices in a production environment.

Each chapter includes practical, hands-on tutorials and real-world examples to help reinforce your understanding of the concepts covered. By the end of this book, you'll have the knowledge and skills to design, build, and deploy microservices-based applications with confidence.

## Who This Book Is For

This book is designed for developers, architects, and DevOps engineers who are interested in adopting microservices architecture using containerization technology. Whether you're new to microservices or looking to expand your knowledge, this book will provide you with the essential tools and techniques to succeed in today's growing software development landscape.


**Developers:**

- If you're a developer looking to moving from traditional monolithic architectures to microservices, this book will provide you with the necessary knowledge and practical skills to design, develop, and deploy microservices-based applications using containerization technology.
- Whether you specialize in a specific programming language or framework, the hands-on tutorials and real-world examples in this book will help you gain a deeper understanding of how to implement microservices using a variety of technologies, including .NET Core, Node.js, React.js, SQL databases, and more.

**Architects:**

- For architects responsible for designing and planning the architecture of modern applications, this book will serve as a comprehensive guide to understanding the principles, patterns, and best practices of microservices architecture.
- You'll learn how to design scalable, resilient, and maintainable systems using microservices and containerization technology, and how to address common challenges such as service discovery, communication, and data management in distributed environments.

**DevOps Engineers:**

- If you're a DevOps engineer tasked with managing the deployment, scaling, and monitoring of microservices-based applications, this book will help you with the necessary skills to leverage containerization tools like Docker and orchestration platforms like Kubernetes.
- You'll learn how to automate the deployment process, implement continuous integration and continuous delivery (CI/CD) pipelines, and ensure the reliability and performance of microservices in production environments.

**Students and Researchers:**

- This book can also be valuable for students and researchers studying software engineering,  and cloud computing. It provides a practical, hands-on approach to learning about microservices architecture and containerization technology, with real-world examples and case studies to illustrate key concepts.

## Key Benefits of Reading This Book:

"Building Microservices with Containers: A Practical Guide" offers a lot of benefits to readers at various stages of their application development journey in understanding and implementing microservices architecture with containerization technology. Here are some key benefits you can expect from reading this book:


**Hands-On Tutorials:**

- Benefit from step-by-step tutorials and real-world examples that guide you through the process of building and deploying microservices using Docker containers.
- Gain practical experience by working on hands-on exercises and projects designed to reinforce your learning and enhance your skills.

**Diverse Technology Stack:**

- Explore a diverse range of technologies and frameworks, including .NET Core, Node.js, React.js, SQL databases, Docker, and Kubernetes.
- Learn how to choose the right tools and technologies for your specific use case, and how to integrate them effectively to build scalable and resilient applications.

**Transition from Monolithic to Microservices:**

- Understand the benefits and challenges of transitioning from monolithic architectures to microservices, and how to plan and execute a successful migration strategy.
- Learn how to decompose monolithic applications into smaller, loosely-coupled services, and how to leverage containerization to improve scalability, flexibility, and resilience.

Whether you're a developer, architect, DevOps engineer, student, or researcher, "Building Microservices with Containers: A Practical Guide" offers valuable insights, practical skills, and career advancement opportunities that will empower you to succeed in today's dynamic and fast-paced software development landscape.


## Hands-On Labs

Here is the high-level list of labs we will cover in this chapter:


**Lab-1: Getting Started with Microservices** - In this lab, we'll introduce you to the concept of microservices and explain their importance in modern application development. You'll gain a high-level understanding of microservices architecture and its benefits.

**Lab-2: Getting Started with Docker** - Here, we'll look into Docker, the modern containerization technology. You'll learn how to install docker, run your first container, and explore basic docker commands.

**Lab-3: Create your First Containerized Microservice with .NET Core** - This lab guides you through creating a microservice using .NET Core and containerizing it with Docker. You'll learn how to write Dockerfiles and build container images for .NET Core microservices.

**Lab-5: Create your Second Containerized Microservice with Node.js** - In this lab, we switch gears to Node.js and create another microservice. You'll containerize a Node.js-based microservice and understand the differences compared to .NET Core.

**Lab-6: Create your First Containerized Website using ASP.NET Core MVC** - Now, it's time to create a containerized website using ASP.NET Core MVC. You'll build a web application, package it as a Docker image, and run it as a container.

**Lab-7: Create your Second Containerized Website using React JS** - In this lab, we'll focus on front-end development by creating a React.js-based website. You'll containerize a React application and understand how to work with front-end containers.

**Lab-8: Create your First Database with SQL Server** - Databases are an essential part of microservices. In this lab, we'll set up a SQL Server database within a container. You'll learn how to create and connect to containerized databases.

**Lab-9: Create your Second Database with PostgreSQL** - PostgreSQL is another popular database choice. This lab guides you through running PostgreSQL in a docker container and executing scripts. You'll understand how to work with different database engines within containers.

**Lab-10: Running Keycloak application in a Docker Container** - External services play a important role in microservices. In this lab, we'll run Keycloak application, an identity and access management system, in a Docker container. You'll configure and interact with Keycloak within the containerized environment.

**Lab-11: Running Drupal website in a Docker Container** - Continuing with external services, we'll set up Drupal website, a content management system, in a Docker container. You'll explore how to work with content management systems within containers.

These hands-on labs provide a practical foundation for building and containerizing microservices. By the end of these labs, you'll have hands-on experience with various technologies and a clear understanding of how to create and run microservices and external services in containers. This knowledge will be invaluable as we progress through the chapters and explore more advanced microservices concepts and deployment strategies.


<!-- ## Labs categorized into three areas: -->
## Categories of Labs:

Labs in this Chapter are categorized into four areas, these categories provide a structured approach to learning containerization across different aspects of web development, from APIs and websites to databases and external services. By completing labs in each category, participants will gain comprehensive knowledge and skills essential for modern application development  practices. 

- Creating Containerized APIs (API Development)
- Creating Containerized Websites (Website Development)
- Setting Up Databases in Containers (Database Containers)
- Running External Services in Docker Containers (External Services)


## Creating Containerized APIs

Labs created within this category, you'll learn how to create containerized APIs using technologies like .NET Core Web API, Node.js.


**.NET Core Web API:**

1. **Introduction to .NET Core Web API**: We'll start by introducing you to .NET Core Web API, a cross-platform framework for building Restful services.

2. **Setting Up an .NET Core Web API Project**: We'll guide you through setting up a new .NET Core Web API project.

3. **Containerization with Docker**: You'll learn how to package your .NET Core Web API as a Docker container. We'll provide guidance on creating a Dockerfile for your web application.

5. **Running the Containerized .NET Core Web API**: You'll see how to run your containerized .NET Core Web API locally and understand how containers simplify deployment.

**Node.js APIs:**

1. **Introduction to Node.js**: Node.js is a popular JavaScript library for building Restful services. We'll introduce you to Node.js and explain its role in modern Rest APIs development.

2. **Creating a Node.js Rest API**: You'll learn how to create a Node.js API from scratch. 

3. **Containerization with Docker**: Similar to .NET Core Web API, we'll guide you through containerizing your Node.js API. You'll create a Dockerfile for your Restful service.

4. **Running the Containerized Node.js API**: You'll see how to run your containerized Node.js API locally. 

By the end of these labs, you'll have hands-on experience with .NET Core Web API and Node.js along with the knowledge of how to containerize web applications. These skills are essential as we move forward to deploy these containerized websites alongside microservices in later chapters. 


## Creating Containerized Websites

Labs created within this category, you'll learn how to create containerized websites using technologies like ASP.NET Core, MVC and React.js.


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

By the end of these labs, you'll have hands-on experience with ASP.NET Core MVC and React.js, along with the knowledge of how to containerize web applications. These skills are essential as we move forward to deploy these containerized websites alongside microservices in later chapters. 


## Setting Up Databases in Containers

Labs created within this category, we'll learn setting up databases within containers for microservices data storage. You'll learn how to create containerized database instances using SQL Server and PostgreSQL.

Microservices often rely on databases to store and manage data. Containerizing databases offers numerous advantages, such as isolation, portability, and versioning. In this section, we'll focus on two popular database systems: SQL Server and PostgreSQL.

**SQL Server:**

1. **Introduction to SQL Server**: We'll introduce you to SQL Server, a robust relational database management system (RDBMS) developed by Microsoft.

2. **Containerization with Docker**: You'll learn how to containerize SQL Server by pulling an official SQL Server Docker image from the Azure Container registry or Docker Hub.

3. **Running SQL Server in a Docker Container**: We'll guide you through running a SQL Server container, configuring database settings, and connecting to the containerized SQL Server instance.

4. **Data Management**: You'll explore data management tasks within a containerized SQL Server, such as creating databases, tables, and performing CRUD (Create, Read, Update, Delete) operations.

5. **Connecting to database locally**: Finally you'll explore different tools like SQL Server Management Studio (SSMS) and Azure data studio for connecting to containerized SQL Server database.

**PostgreSQL:**

1. **Introduction to PostgreSQL**: We'll introduce you to PostgreSQL, a powerful open-source relational database system known for its scalability and extensibility.

2. **Containerization with Docker**: You'll learn how to containerize PostgreSQL by pulling an official PostgreSQL Docker image from the Docker Hub.

3. **Running PostgreSQL in a Docker Container**: We'll guide you through running a PostgreSQL container, configuring database settings, and connecting to the containerized PostgreSQL instance.

4. **Data Management**: You'll explore data management tasks within a containerized PostgreSQL database, including creating databases, tables, and executing SQL queries.

5. **Connecting to database locally**: Finally you'll explore different tools like `PSQL` and Pgadmin4 for connecting to containerized PostgreSQL database.

By the end of these labs, you'll have hands-on experience with containerized SQL Server and PostgreSQL databases, understanding their role in microservices data storage. These skills are important as you proceed through the chapters, where microservices will interact with these containerized databases to retrieve and store data. 

## Running External Services in Containers

Labs created within this category, we'll learn integration of external services into your microservices architecture. You'll learn how to run external services like Keycloak and Drupal in Docker containers, enhancing the capabilities of your microservices.


External services play a importent role in microservices architecture, providing essential functionalities such as authentication and content management. Containerizing these external services offers several advantages, including consistency and simplified deployment. In this section, we'll focus on two prominent external services: Keycloak and Drupal.

**Keycloak:**

1. **Introduction to Keycloak**: Keycloak is an open-source identity and access management system. We'll introduce you to Keycloak and explain its significance in microservices authentication.

2. **Containerization with Docker**: You'll learn how to containerize Keycloak by pulling an official Keycloak Docker image from the Docker Hub.

3. **Running Keycloak in a Docker Container**: We'll guide you through running a Keycloak container, configuring realms, users, and roles within the containerized Keycloak instance.

4. **Testing the Keycloak Application Locally**: Finally you'll see how to browse your containerized Keycloak application locally and login into admin portal and intacting with Keycloak application. 

<!-- 4. **Microservices Integration**: You'll understand how to integrate microservices with Keycloak for secure authentication and authorization. Keycloak serves as an identity provider for your microservices. -->

**Drupal:**

1. **Introduction to Drupal**: Drupal is a popular open-source content management system (CMS). We'll introduce you to Drupal and its role in managing content for microservices.

2. **Containerization with Docker**: You'll learn how to containerize Drupal by pulling an official Drupal Docker image from the Docker Hub.

3. **Running Drupal in a Docker Container**: We'll guide you through running a Drupal container, setting up a website, and managing content within the containerized Drupal instance.

4. **Testing the Drupal Website Locally**: Finally you'll see how to browse your containerized Drupal Website locally and login into drupal portal and intacting with drupal website.
<!-- 4. **Microservices Integration**: You'll explore how to integrate microservices with Drupal for content management. Drupal can serve as a content source for your microservices. -->

By the end of these labs, you'll have hands-on experience with containerized Keycloak and Drupal instances, understanding how to integrate them seamlessly into your microservices ecosystem. These skills are essential as you proceed through the chapters, where microservices will rely on these external services for authentication, authorization, and content management.