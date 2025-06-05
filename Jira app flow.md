# Jira Integration System Architecture

## System Overview
This architecture diagram shows the complete flow of a Jira integration system with three main applications, credential management, and cloud deployment strategy.

## Architecture Diagram

```mermaid
graph TB
    %% User Authentication Layer
    subgraph "Authentication Layer"
        A[🔐 Python Flask Login Screen<br/>- Jira Username Input<br/>- Jira API Token Input]
        B[(🗄️ Redis Cache GCP<br/>- User Credentials Storage<br/>- Session Management)]
    end
    
    %% Main Application Layer
    subgraph "Application Layer"
        C[📊 Main Dashboard<br/>- User Interface<br/>- Navigation Hub]
        D[🧭 Navigation Bar<br/>3 Application Modules]
    end
    
    %% Application Modules
    subgraph "Application Modules"
        E1[📈 App 1: Jira Sprint Summary<br/>- Sprint Analytics<br/>- Progress Tracking]
        E2[💡 App 2: Epic & Saga Recommendations<br/>- AI-Powered Title Generation<br/>- Content Suggestions]
        E3[📊 App 3: Epic Completion Analysis<br/>- Functionality Assessment<br/>- User Story Comparison]
    end
    
    %% Local Environment
    subgraph "Local Development"
        F[📁 .env File<br/>- LLM Client ID<br/>- LLM Client Secret]
    end
    
    %% Cloud Environment
    subgraph "GCP Cloud Deployment"
        G[🔒 GCP Secret Manager<br/>- Secure Credential Storage<br/>- Centralized Secret Management]
        H[☁️ Cloud Run<br/>- Container Hosting<br/>- Scalable Deployment]
    end
    
    %% External Services
    subgraph "External APIs"
        I[🔗 Jira API<br/>- Sprint Data<br/>- Epic Information<br/>- User Stories]
        J[🤖 LLM Service<br/>- AI Processing<br/>- Content Generation]
    end
    
    %% Flow Connections
    A -->|Store/Retrieve Credentials| B
    A -->|Successful Login| C
    C --> D
    D --> E1
    D --> E2
    D --> E3
    
    %% Local Environment Connections
    E1 -.->|Read Credentials| F
    E2 -.->|Read Credentials| F
    E3 -.->|Read Credentials| F
    
    %% Cloud Migration
    F -.->|Migration Path| G
    G -->|Secure Access| H
    H --> E1
    H --> E2
    H --> E3
    
    %% External API Connections
    E1 -->|Fetch Sprint Data| I
    E2 -->|Get Epic Info| I
    E3 -->|Retrieve User Stories| I
    
    E1 -->|AI Processing| J
    E2 -->|Generate Recommendations| J
    E3 -->|Analyze Completion| J
    
    %% Styling
    classDef authStyle fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef appStyle fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef moduleStyle fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef localStyle fill:#fff3e0,stroke:#e65100,stroke-width:2px,stroke-dasharray: 5 5
    classDef cloudStyle fill:#fce4ec,stroke:#880e4f,stroke-width:2px,stroke-dasharray: 5 5
    classDef externalStyle fill:#f1f8e9,stroke:#33691e,stroke-width:2px
    
    class A,B authStyle
    class C,D appStyle
    class E1,E2,E3 moduleStyle
    class F localStyle
    class G,H cloudStyle
    class I,J externalStyle
