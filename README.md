# Pipeline-Documentation
Pipeline 

DevOps CI/CD & Jenkins Notes
Software Development Life Cycle (SDLC)

SDLC defines the process used to develop software efficiently and systematically.

Stages of SDLC

1. Planning

2. Designing

3. Development

4. Testing

5. Deployment

6. Support / Maintenance

| Stage       | Responsible Team          |
| ----------- | ------------------------- |
| Planning    | Management / Product Team |
| Designing   | Architecture Team         |
| Development | Developers                |
| Testing     | QA / Testers              |
| Deployment  | DevOps                    |
| Support     | Maintenance Team          |

CI/CD Pipeline Overview

Typical CI/CD pipeline flow:

```mermaid
graph LR
    A[Code] -- push --> B[SCM<br>GitHub]
    B --> C[Scan<br>SonarQube]
    C --> D[Build]
    D --> E[Artifactory]
    E --> F[Ansible]
    
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style B fill:#bbf,stroke:#333,stroke-width:2px
    style C fill:#bbf,stroke:#333,stroke-width:2px
    style D fill:#bbf,stroke:#333,stroke-width:2px
    style E fill:#bbf,stroke:#333,stroke-width:2px
    style F fill:#bfb,stroke:#333,stroke-width:2px
```

graph LR
    subgraph CI [Continuous Integration Phase]
        A[Code] -- push --> B[SCM<br>GitHub]
        B --> C[Scan<br>SonarQube]
        C --> D[Build<br>Docker]
        
        D -- Files --> E[Artifactory<br>JFrog / Nexus]
        D -- docker --> F([Images])
        
        F --> G[Registry]
    end
    
    subgraph CD [Continuous Deployment Phase]
        E --> H[K8S]
        G --> H
    end
    
    %% Styling
    style A fill:#f9f,stroke:#333,stroke-width:2px
    style B fill:#bbf,stroke:#333,stroke-width:2px
    style C fill:#bbf,stroke:#333,stroke-width:2px
    style D fill:#bbf,stroke:#333,stroke-width:2px
    style E fill:#ffb,stroke:#333,stroke-width:2px
    style F fill:#eee,stroke:#333,stroke-width:2px,stroke-dasharray: 5 5
    style G fill:#ffb,stroke:#333,stroke-width:2px
    style H fill:#bfb,stroke:#333,stroke-width:2px

Tools used in CI/CD

1. Jenkins Pipeline

2. Azure DevOps Pipeline

3. GitHub Actions

```mermaid
graph TD
    A[Infrastructure Pipeline] --> B[Terraform Automation]
    B --> C[Provision Infrastructure]
    
    style A fill:#e1f5fe,stroke:#039be5,stroke-width:2px
    style B fill:#f3e5f5,stroke:#8e24aa,stroke-width:2px
    style C fill:#e8f5e9,stroke:#43a047,stroke-width:2px
```
Jenkins

Jenkins is an open-source CI/CD automation tool written in Java.

It is used to automate:

* Build

* Test

* Integration

* Deployment

Jenkins integrates with different Version Control Systems and DevOps tools through plugins.

Jenkins Architecture

Jenkins follows a Master–Agent architecture.

Components

1. Master Node

Responsible for:

* Job scheduling

* Configuration management

* Plugin management

* UI interface

2. Agent Nodes (Workers)

Responsible for:

* Executing build jobs

* Running deployment tasks

Agents can run on different environments:

* Ubuntu

* RedHat

* CentOS

* Windows


Jenkins Architecture


```mermaid
graph LR
    %% Main VM Box
    Master["<b>VM</b><br>⚙️ Jenkins (Master node)<br>☕ Java (Worker node base)"]

    %% Target Environments
    DEV["<b>DEV</b><br>☕ (Java) ubuntu"]
    QA["<b>QA</b><br>☕ (Java) Redhat"]
    UAT["<b>UAT</b><br>☕ (Java) centos"]
    PROD["<b>PROD</b><br>☕ (Java) windows"]

    %% Architecture Connections
    Master --> DEV
    Master --> QA
    Master --> UAT
    Master --> PROD

    %% Styling
    style Master fill:#f3e5f5,stroke:#8e24aa,stroke-width:2px
    style DEV fill:#e1f5fe,stroke:#039be5,stroke-width:2px
    style QA fill:#e1f5fe,stroke:#039be5,stroke-width:2px
    style UAT fill:#e1f5fe,stroke:#039be5,stroke-width:2px
    style PROD fill:#e8f5e9,stroke:#43a047,stroke-width:2px
```