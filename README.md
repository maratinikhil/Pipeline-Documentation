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
    %% Defining the Nodes with distinct shapes and emojis
    A(💻 Code) -- push --> B(🐙 SCM: GitHub)
    B --> C{{🔎 Scan: SonarQube}}
    C --> D(🔨 Build)
    D --> E[(📦 Artifactory)]
    E --> F((🚀 Deploy: Ansible))

    %% Modern Class Definitions for cleaner styling
    classDef default font-family:sans-serif,font-weight:bold,color:#333;
    classDef source fill:#e3f2fd,stroke:#1e88e5,stroke-width:2px;
    classDef testing fill:#fff3e0,stroke:#fb8c00,stroke-width:2px;
    classDef storage fill:#f3e5f5,stroke:#8e24aa,stroke-width:2px;
    classDef deploy fill:#e8f5e9,stroke:#43a047,stroke-width:2px;

    %% Applying the styles
    class A,B,D source;
    class C testing;
    class E storage;
    class F deploy;
```

```mermaid
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
```

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

- Build

- Test

- Integration

- Deployment

Jenkins integrates with different Version Control Systems and DevOps tools through plugins.

Jenkins Architecture

Jenkins follows a Master–Agent architecture.

Components

1. Master Node

Responsible for:

- Job scheduling

- Configuration management

- Plugin management

- UI interface

2. Agent Nodes (Workers)

Responsible for:

- Executing build jobs

- Running deployment tasks

Agents can run on different environments:

- Ubuntu

- RedHat

- CentOS

- Windows

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

Jenkins Job Types

There are two types of Jenkins jobs:

1. Freestyle Job

2. Pipeline Project

3. Freestyle Job

A traditional Jenkins job configured through the Jenkins Web UI.

Characteristics:

Simple configuration

No coding required

Used for small automation tasks

Freestyle Build Triggers

Developers push code to the repository.

Jenkins periodically checks for updates using cron scheduling.

---

Cron Jobs

A cron job is a time-based scheduler in Unix/Linux systems used to run scripts at fixed intervals.

| Cron Expression | Meaning      |
| --------------- | ------------ |
| `* * * * *`     | Every minute |
| `0 * * * *`     | Every hour   |
| `0 0 * * *`     | Daily        |

Uses:

- Nightly builds

- Daily reports

- Scheduled CI builds

Jenkins Pipeline

A pipeline defines the entire CI/CD workflow as code.

It is written inside a file called: Jenkinsfile

Benefits:

* Pipeline as Code

* Version controlled

* Easier automation

Types of Jenkins Pipelines

1. Scripted Pipeline

2. Declarative Pipeline

1. Scripted Pipeline

A Groovy-based pipeline with flexible programming logic.

node {

    stage('Build') {
        echo 'Building...'
    }

    stage('Test') {
        echo 'Testing...'
    }

    if (currentBuild.result == 'SUCCESS') {
        echo 'Build Succeeded'
    } else {
        echo 'Build Failed'
    }

}



```mermaid
graph LR
    Start((Start)) --> Build[Stage: Build]
    Build --> Test[Stage: Test]
    Test --> Check{Result == SUCCESS?}
    Check -- Yes --> Success[echo 'Build Succeeded']
    Check -- No --> Fail[echo 'Build Failed']
    
    style Start fill:#e0e0e0,stroke:#333,stroke-width:2px
    style Build fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    style Test fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    style Check fill:#fff9c4,stroke:#fbc02d,stroke-width:2px
    style Success fill:#e8f5e9,stroke:#43a047,stroke-width:2px
    style Fail fill:#ffebee,stroke:#c62828,stroke-width:2px
```

2. Declarative Pipeline

A structured and easier-to-read pipeline syntax.

Uses predefined blocks such as:

```mermaid
graph TD
    Pipeline[Pipeline]
    
    Pipeline --> agent[agent]
    Pipeline --> ENV[ENV]
    Pipeline --> parameter[parameter]
    Pipeline --> triggers[pollSCM<br>Triggers]
    Pipeline --> Stages[Stages]
    
    Stages --> Stage1[Stage]
    Stage1 --> Steps1[Steps]
    
    Stages --> Stage2[Stage]
    Stage2 --> Steps2[Steps]

    %% Styling to separate main blocks from stages
    style Pipeline fill:#f3e5f5,stroke:#8e24aa,stroke-width:2px
    style Stages fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    style Stage1 fill:#e8f5e9,stroke:#43a047,stroke-width:1px
    style Stage2 fill:#e8f5e9,stroke:#43a047,stroke-width:1px
```

```grovy
pipeline {

    agent {
        label 'JAVA'
    }

    triggers {
        pollSCM('* * * * *')
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

    }
}
```


Basic Jenkins Declarative Pipeline Example
```grovy
pipeline {

    agent { label 'JAVA' }

    stages {

        stage('Git Checkout') {
            steps {
                git url: 'git-tool-link', branch: 'main'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }

    }

}
```

Jenkins Installation
On Master Node

sudo apt update
sudo apt install openjdk-21-jdk -y
sudo apt install jenkins -y

On Worker Node
Install Maven:

sudo apt update
sudo apt install maven -y
mvn --version

Jenkins Pipeline Execution

Steps:

1. Login to Jenkins Dashboard

2. Click New Item

3. Select Pipeline

4. Configure the pipeline

5. Add Jenkinsfile

6. Click Build Now


SonarQube

SonarQube is an open-source code quality and security analysis tool.

It scans application source code to detect:

* Bugs

* Security vulnerabilities

* Code smells

* Technical debt

Why Use SonarQube
It helps check the overall code health before deployment.

SonarQube Key Features

1. Security

Detects vulnerabilities like:

* SQL Injection

* Hardcoded credentials

* XSS vulnerabilities

2. Reliability

Identifies bugs that may cause application failures.

Ensures stable production applications.

3. Maintainability

* Detects:

* Code smells

* Technical debt

Helps keep code:

* Clean

* Readable

* Easy to modify

4. Security Hotspots

Flags sensitive areas such as:

* Encryption

* Authentication
  
* Authorization

Requires manual developer review.

5. Dependency Risks

Scans third-party libraries for known vulnerabilities.

Prevents insecure packages from entering production.