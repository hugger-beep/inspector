# Multi-Account Security Posture Management with AWS Inspector

This demo showcases how to implement a centralized vulnerability management solution across multiple AWS accounts using AWS Inspector, Security Hub, and Lambda functions.

## Architecture

```mermaid
flowchart TD
    subgraph "Member Accounts"
        Inspector1[AWS Inspector] -->|Findings| SecurityHub1[Security Hub]
        Inspector2[AWS Inspector] -->|Findings| SecurityHub2[Security Hub]
        Inspector3[AWS Inspector] -->|Findings| SecurityHub3[Security Hub]
    end
    
    subgraph "Security Account"
        SecurityHub1 -->|Aggregated Findings| CentralHub[Central Security Hub]
        SecurityHub2 -->|Aggregated Findings| CentralHub
        SecurityHub3 -->|Aggregated Findings| CentralHub
        
        CentralHub -->|All Findings| EventBridge[EventBridge]
        EventBridge -->|Critical Findings| AlertLambda[Alert Lambda]
        EventBridge -->|All Findings| DashboardLambda[Dashboard Lambda]
        EventBridge -->|Selected Findings| RemediationLambda[Remediation Lambda]
        
        AlertLambda -->|Notifications| SNS[SNS Topic]
        DashboardLambda -->|Update| Dashboard[QuickSight Dashboard]
        DashboardLambda -->|Store Data| S3[S3 Data Lake]
        RemediationLambda -->|Cross-Account Role| RemediationRole[Remediation Role]
    end
    
    RemediationRole -->|Fix Issues| Resources[AWS Resources]
    SNS -->|Alerts| Email[Email]
    SNS -->|Alerts| Slack[Slack]
```
