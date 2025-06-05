# DevSecOps Pipeline Integration with AWS Inspector

This demo showcases how to integrate AWS Inspector into a DevSecOps pipeline to scan container images during the CI/CD process, block deployments with critical vulnerabilities, and generate security reports for development teams.

## Architecture

```mermaid
flowchart TD
    Developer([Developer]) -->|Push Code| CodeRepo[Code Repository]
    CodeRepo -->|Trigger| Pipeline[CI/CD Pipeline]
    Pipeline -->|Build| ContainerImage[Container Image]
    ContainerImage -->|Push| ECR[Amazon ECR]
    
    ECR -->|Scan Trigger| Inspector[AWS Inspector]
    Inspector -->|Findings| EventBridge[Amazon EventBridge]
    
    EventBridge -->|Critical Findings| BlockDeploy[Block Deployment Lambda]
    EventBridge -->|All Findings| ReportGen[Report Generator Lambda]
    
    BlockDeploy -->|Stop Pipeline| Pipeline
    ReportGen -->|Generate Report| S3[S3 Bucket]
    ReportGen -->|Notify Team| SNS[Amazon SNS]
    
    subgraph "Deployment Decision"
        EventBridge
        BlockDeploy
    end
    
    subgraph "Reporting"
        ReportGen
        S3
        SNS
    end
```
