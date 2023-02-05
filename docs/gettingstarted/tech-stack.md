Here is a list of technologies that are used in the development of Microservices development, deploy & run those applications  on Kubernetes cluster:

Here you will also see table of contents of each technology so that you can learn them offline when needed.

You may encounter some specific tools and technologies that are not listed here but you will see the references end of each lab.

## .NET Core REST API, C\#

??? question ".NET Core REST API, C\#"

    Here is a list of technologies that are commonly used in the development of a .NET Core REST API:

    .NET Core: .NET Core is a cross-platform version of the .NET framework that is used to build web applications and APIs.

    This is just a sample of the technologies that may be used in a .NET Core REST API stack.

    - .NET Core Web API Scalable Project structure
    - Web API Clean Architecture
    - Dependency Injection
    - Middleware & Routing Configuration
    - Web API Versioning & filtering, pagination
    - API Request Model Validation
    - Exception handling, Logging using Nlog
    - Caching using Redis
    - Configure Swagger & using NSwag
    - Automapper
    - Repository Pattern
    - Entity Framework Core & LINQ queries
    - Security & Identify
    - Authorization, Authentication
    - Data protection, Secrets management
    - Unit & Integration testing using Xunit, Moq, AutoFixure
    - Performance Load and Stress testing
    - Microservices Architecture
    - Invoking Legacy webservices

## Azure DevOps

??? question "Azure DevOps"

    Azure DevOps is a set of development tools, services, and features that enable teams to plan, develop, deliver, and maintain software more efficiently. The technology stack for Azure DevOps includes:

    - Azure Boards
    - Azure Repos
    - Azure Pipelines
    - Azure Test Plans
    - Azure Artifacts
    - YAML Schema
    - Build, Test, & deploy .NET Core apps to Azure
    - Build, Test, & deploy React JS apps to Azure
    - Build, Test, & deploy Blazor apps to Azure
    - Build & deploy Azure SQL database using [DACPAC]
    - Validate and deploy ARM Templates
    - Git Clone, Code review with Pull request
    - Private Agent configuration, Approvals, Artifact
    - Azure DevOps Pipelines with YAML
    - Continuous integration (CI)
    - Continuous Delivery (CD)
    - Release variables, Replace Tokens
    - Security setup in Azure DevOps

## Azure Cloud

??? question "Azure Cloud"

        Azure is a cloud computing platform and infrastructure created by Microsoft that provides a range of cloud services, including computing, analytics, storage, and networking. The technology stack for Azure includes:

        - Azure Accounts, Subscriptions, and Billing
        - Manage access to Azure resources using RBAC
        - Create Resource Groups
        - Create Azure App Service Plan & ASE
        - Create Web App, API App, Mobil App
        - Create Azure SQL Database, Cosmos DB
        - Create Storage account
        - Application Insights
        - Redis Cache
        - Azure API Management
        - Create Azure Key Vault for secrets
        - Provision Azure resources via ARM Templates & Portal
        - Azure Monitor & Log Analytics
        - Calculate Azure Pricing
        - Create API Gateway using API Management
        - Create Azure Application Gateway
        - Create VNet, Subnet with Network Security Groups

## Azure Kubernetes Service (AKS)

??? question "Azure Kubernetes Service (AKS)"

    Azure Kubernetes Service (AKS) is a managed Kubernetes service that allows you to deploy and manage containerized applications on Azure. Here is the list of topics form AKS you may need to learn before begin the labs.


    - Introduction to AKS
        - What is AKS
        - Benefits of using AKS
    - What Is Azure Kubernetes Service (AKS)?
        - Kubernetes ─ Master Machine Components
        - Kubernetes ─ Node Components
    - Kubernetes Architecture Components
        - Kubernetes ─ Master Machine Components
        - Kubernetes ─ Node Components
    - Creating AKS cluster
        - Prerequisites
        - Creating an AKS cluster using the Azure portal
        - Creating an AKS cluster using the Azure CLI
        - Testing cluster connection & creating namespace
    - Deploying applications to AKS
    - Creating a container image
    - Pushing the image to a container registry
    - Deploying the application to AKS
    - Managing and scaling an AKS cluster
    - Upgrading AKS clusters
    - Scaling the number of nodes in an AKS cluster
    - Monitoring and logging AKS clusters
    - Deploying k8s ingress controller
        - Deploying k8s ingress controller part 1
        - Deploying k8s ingress controller part 2
    - Adding TLS/SSL to the ingress
    - K8s horizontal pod autoscaler [HPA]
        - K8s horizontal pod autoscaler [HPA]
        - HPA in action
        - AKS cluster autoscaling
    - Integrating AKS with Azure Monitor
    - AKS Storage and Networks
    - AKS storage overview
        - Creating storage classes
        - Storage: Persistent claims
        - Shared volumes
        - Create resource for shared volume
        - Challenge: Lost volumes
        - Solution: Find and remove PVs
        - Networking and AKS
        - Load balancing and Ingress: Setup
    
## Terraform 

??? question "Terraform"

    Terraform is a tool for building, changing, and versioning infrastructure safely and efficiently.

    - Introduction to Terraform
    - What is Infrastructure as Code?
    - What is Terraform
    - What is HCL - Hashicorp Configuration Language?
    - Benefits of using Terraform
    - Terraform Features
    - Getting started with Terraform
    - Installing Terraform
    - Creating your first Terraform configuration
    - Terraform CLI
    - Working with Terraform modules
    - Creating and using modules
    - Sharing modules with the Terraform Registry
    - Terraform Variables
    - State Management
    - Remote Backends 
    - Managing infrastructure with Terraform
    - Declaring resources in a configuration
    - Applying changes to infrastructure
    - Viewing and modifying infrastructure
    - Collaborating with Terraform
    - Version control best practices for Terraform configurations
    - Collaborating with team members using Terraform
    - Advanced Terraform features
    - Using Terraform variables and outputs
    - Using Terraform workspaces
    - Terraform Cloud and Terraform Enterprise
    - Mutable vs Immutable Infrastructure
    - Lifecycle Rules in Terraform
    - Data Sources in Terraform
    - Use of for_each meta arguments in Terraform
    - Version Constraints in Terraform

## Argocd

??? question "ArgoCD"

    Argo CD is a continuous delivery tool that uses GitOps to deploy applications to Kubernetes. The technology stack for Argo CD includes:

    1. Introduction to Argo CD
    - What is Argo CD
    - Benefits of using Argo CD
    2. Setting up Argo CD
    - Prerequisites
    - Installing Argo CD using the Helm chart
    - Accessing the Argo CD UI
    3. Creating and managing applications in Argo CD
    - Defining an application in a Git repository
    - Adding an application to Argo CD
    - Updating and rolling back application deployments
    4.  Working with Argo CD configurations
        -  Managing application configurations in Git
        -  Using Git branches and tags in Argo CD
        -  Handling conflicts and errors in configurations
    5.  Collaborating with Argo CD
        -  Using Argo CD with Git hosting platforms
        -  Setting up multi-user access to Argo CD
    6.  Advanced Argo CD features
        -  Using Argo CD with GitOps workflows
        -  Integrating Argo CD with CI/CD pipelines
        -  Using Argo CD with GitLab CI/CD


## Helm Chart

??? question "Helm Chart"


    Helm is a package manager for Kubernetes that allows you to easily install, upgrade, and manage applications on your Kubernetes cluster. A Helm chart is a package of pre-configured Kubernetes resources that defines the structure and dependencies of an application.

    1. Introduction to Helm
        - What is Helm
        - Benefits of using Helm
    2. Getting started with Helm
        - Installing Helm
        - Initializing Helm and installing Tiller
    3. Working with Helm charts
        - Finding and downloading charts from the Helm chart repository
        - Creating and deploying your own charts
        - Upgrading and rolling back chart deployments
    4. Managing dependencies with Helm
        - Using Helm to manage the dependencies of a chart
        - Sharing charts and chart dependencies with the Helm chart repository
    5. Collaborating with Helm
        - Using Helm with version control systems
        - Collaborating with team members using Helm
    6. Advanced Helm features
        - Using Helm with continuous delivery pipelines
        - Customizing Helm behavior with hooks
        - Extending Helm with plugins

## React JS

??? question "React JS"

    React is a JavaScript library for building user interfaces. It was developed by Facebook, and is often used for building single-page applications and mobile applications.

    1. Introduction to React
        - What is React
        - Why use React
    2. Getting started with React
        - Setting up a React development environment
        - Creating a React application
        - Understanding the structure of a React application
    3. Working with React components
        - Defining and rendering components
        - Props and state in components
        - Lifecycle methods in components
    4. Building user interfaces with React
        - Working with JSX
        - Using React hooks
        - Managing data with context
    5. Managing application state with Redux
        - Introduction to Redux
        - Setting up Redux in a React application
        - Working with actions, reducers, and the store
    6. Routing in React applications
        - Introduction to React Router
        - Setting up routes and navigating between them
        - Working with dynamic routes and parameters

## Development Tools

??? question "Development Tools"

    There are many tools that are commonly used in the development of software applications. Some examples of development tools include:

    - Visual Studio 2022/2019
    - Visual Studio Code
    - SQL Server Management Studio, SQL Profiler
    - Node JS, NPM, Notepad++
    - Postman, SOAP UI
    - Brower Developer Tools
    - Agile & Scrum with JIRA or Azure Board
    - Nuget Manager, GitHub Desktop
    - Open SSL
    - JSON Viewer/Formatter, JWT debugger, SAML-Tracer
    - Azure Storage Explorer

## Testing Area

??? question "Testing Area"

    - Create BDDs with SpectFlow
    - Unit testing with XUnit, Moq & AutoFixure.
    - Automate Unit test cases in Azure Build Pipeline
    - Automate BDD test cases in Azure Build Pipeline
    - UI test with Selenium
    - Review test results
    - Test Analytics
    - Review & Report code coverage results
    - API Automation testing using SOAP UI
    - Create Regression test suite
    - Create Smoke / Sanity test suite

## Network Testing Tools

??? question "Network Testing Tools"

    - Ipconfig - Used to get the IP address
    - Ping - Used to send signals to another device on the network to see if it is active.
    - Trace route - Used to see the step by step route a packet takes to the destination.
    - nslookup - used to check firewall, fetches the DNS records for given domain name or IP address
    - Telnet - connection and communication with a remote or local host via the Telnet TCP/IP protocol.
    - Netstat - used to view of active ports on your machine and their status. This helps user to understand which ports are open, closed, or listening for incoming connections.


## Blazor [optional]

??? question "Blazor"

    Blazor is a framework for building client-side web applications using C# and .NET. It uses WebAssembly, a standard for running compiled code in web browsers, to allow .NET code to run in the browser alongside HTML and JavaScript.

    1. Introduction to Blazor
        - What is Blazor
        - Benefits of using Blazor
    2. Getting started with Blazor
        - Setting up a Blazor development environment
        - Creating a Blazor application
        - Understanding the structure of a Blazor application
    3. Building user interfaces with Blazor
        - Defining and rendering components
        - Working with JSX
        - Using Blazor templates
    4. Managing application state with Blazor
        - Introduction to component state and lifecycle
        - Using component parameters and events
        - Sharing state between components
    5. Data access in Blazor
        - Introduction to Entity Framework and LINQ
        - Querying and updating data with Entity Framework Core
        - Using dependency injection to manage data access
    6. Routing in Blazor applications
        - Introduction to Blazor routing
        - Setting up routes and navigating between them
        - Working with dynamic routes and parameters