 Secure API Access with AWS Inspector

This demo showcases how to implement secure API access patterns using AWS Inspector to detect and prevent vulnerabilities in API components.

## Architecture

```mermaid
flowchart TD
    Inspector[AWS Inspector] -->|Scan| LambdaFunctions[Lambda Functions]
    Inspector -->|Scan| ContainerImages[Container Images]
    
    EventBridge[Amazon EventBridge] -->|Trigger| APISecurityLambda[API Security Lambda]
    Inspector -->|Findings| EventBridge
    
    APISecurityLambda -->|Update| WAFRules[WAF Rules]
    APISecurityLambda -->|Modify| APIGateway[API Gateway]
    
    APIGateway -->|Protect| APIs[API Endpoints]
    WAFRules -->|Filter| APIs
```
