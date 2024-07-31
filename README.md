# 5 Things you can do with GitHub Copilot

This repository contains resources and steps that I used to demonstrate some techniques I use with GitHub Copilot. The goal is to show how to use GitHub Copilot to improve code quality, write tests, refactor code, and create DevOps workflows.

## Prep

1. Open VS Code and install the GitHub Copilot extension.
1. Clone https://github.com/PlagueHO/game-master-copilot/tree/GitHub-Copilot-Demo (make sure to use the `GitHub-Copilot-Demo` branch)
1. Open `AccountController.cs`
1. Open `AccountController.Tests`
1. Open `Location.cs`
1. Open `build-application-containers.yml`
1. Open `Repository.cs`
1. Open `OptionsServiceExtensions.cs`

## Part 1 - Align Tests

1. Creating a new controller, based on an existing pattern with:

```text
Create a new Tenant API controller for using the #file:ITenantRepository.cs and copying the pattern from #file:AccountController.cs
```

1. Save as `TenantController.cs`.
1. Change `if (tenant == null)` to `if (tenant != null)` or some other logic error.
1. Create a new tests using the:

```text
Create unit tests for #file:TenantController.cs using the same pattern as #file:AccountController.Tests.cs. Include similar edge cases and explain why you're covering each edge case. Test intent of the code rather than the implementation.
```

1. Then evaluate the quality of the tests with:

```text
With the above tests run over #file:TenantController.cs , are you expecting any of them to currently fail?
```

1. Follow up with:

```text
How could I make the code in #file:TenantController.cs more testable?
```

## Part 2 - Write RegEx

1. Create a regex inline. Select `Location.cs` lines 47 to 63.

```text
Add regex validation for each of these properties.
```

1. Select the phone number validation regex and delete the content.
1. Add comment `// Nah, make this a kiwi as phone number mate!`
1. Let Copilot suggest the correct RegEx.

1. Create a regex from reference to extract providers from a Terraform file.

```text
Create a C# regex and code to extract all providers used in a Terraform file.
```

## Part 3 - Refactor Code

1. Open `AccountController.cs`
1. Select all code and press CTRL+I.
1. Enter `How would I refactor this controller to meet SOLID principles?`

1. Open `OptionsServiceExtensions.cs`
1. Select `TrimStringProperties()` method.
1. Enter `How could I improve the performance of this method and explain why this would help?`

## Part 4 - DevOps Techniques

1. Refactor a GitHub Workflow to be a matrix build:

```text
Change this GitHub Actions workflow #file:build-application-containers.yml to be a matrix build to build an array of containers.
```

1. Use Microsoft Copilot to analyze an architecture diagram, include the `azure-web-app-architecture.jpeg` image:

```text
I am an SRE platform engineer creating Terraform for the attached referrence Azure architecture.

I need to identify the Azure Services and thier relationships to each other to provide to GitHub Copilot Chat to help me create the Terraform code.

Provide a list of Azure services that are used in this architecture

For each Azure service:
- Define the relationship between each Azure service and how they interact with each other. Explain relationship in detail.
- Recommend if the Azure service should be created as a separate Terraform module or as part of the main Terraform configuration. Explain your reasoning.

Output the information as a table and don't provide any other information except the table.
```

1. Combine output with this prompt:

```text
I am an SRE platform engineer creating Terraform for Azure architecture as per the following table. The table of Azure Services was extracted from an architecture diagram by Microsoft Copilot. The application is for an online shopping platform for Contoso.

Create the terraform files that will need to be created for this architecture. Use Azure recommended resource naming standards that will support prod, test and dev enviornments.

Based on the image I provided, here is the list of Azure services used in the architecture and their relationships:

| Azure Service | Relationship with Other Services | Terraform Module Recommendation |
| --- | --- | --- |
| Azure Active Directory | Provides identity services to App Service through authentication. | Separate module due to its potential reuse across different parts of the infrastructure and its centrality in managing identities. |
| Azure Key Vault | Stores secrets used by App Service web app and SQL Database for secure access without exposing sensitive data in configuration files. | Separate module because it handles security credentials which might be used by multiple resources, thus enhancing modularity and security practices. |
| App Service Plan | Hosts the App Service web app and defines the physical resources where the web app will run. | Part of main configuration as it is closely tied to the lifecycle of the web app itself. |
| App Service web app | Hosts the application code; interacts with SQL Database for data storage/retrieval, uses deployment slots for staged deployments. | Part of main configuration since it's directly related to application deployment and operation. |
| Deployment slots | Used by App Service web app for staging new versions before production; allows testing without affecting production environment. | Part of main configuration as they are an integral feature of the App Service that facilitates CI/CD processes. |
| SQL Database | Stores application data accessed by App Service web app; sends diagnostic logs to Log Analytics for monitoring purposes. | Separate module because databases can often be managed independently from applications, allowing for clearer separation of concerns. |
| Azure Monitor | Collects performance metrics from both App Service web app and SQL Database; sends log data to Log Analytics. | Part of main configuration if only used for this specific architecture's monitoring needs, otherwise separate if used across multiple systems or environments. |
| Log Analytics | Receives log data from both SQL Database and Azure Monitor; performs analysis on logs for insights into system health/performance. | Separate module due to its potential use across various services beyond just this architecture, providing centralized logging and analysis capabilities. |
```

## Part 5 - Techniques for Architects and Senior Developers

1. Use advanced prompt engineering techniques to create prompts for other engineers. Expert critique.

```text
Imagine you are a luminary software engineer, such as Martin Fowler. Make software design improvement recommendations for the #file:Repository.cs and for each explain why it is important and what benefits it will have.
```

1. Follow up with:

```text
Imagine you are DevOps expert, Gene Kim. What counter recommendations would you make to the above recommendations on #file:Repository.cs Martin Fowler?
```
