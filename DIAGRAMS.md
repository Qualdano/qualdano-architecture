# Qualdano System Diagrams

This document provides detailed explanations of the architectural and process diagrams for the Qualdano project.

## 1. High-Level System Architecture
```mermaid
graph TD
    A[Qualtrics Survey Platform] -->|Survey Data| B[Qualtrics Plugin]
    B -->|Hashed Data| C[Web Server]
    C -->|Process Data| D[Smart Contract Executor]
    D -->|Execute Contract| E[Cardano Blockchain]
    F[User Interface] -->|Interact| C
    F -->|View Results| C
    G[Researcher] -->|Create Survey| A
    G -->|Verify Data| F
    H[Survey Participant] -->|Respond| A
```

This diagram illustrates the main components of the Qualdano system and their interactions. It shows how data flows from the Qualtrics Survey Platform through our plugin, web server, and ultimately to the Cardano blockchain.

## 2. Data Flow Diagram
```mermaid
graph LR
    A[Survey Participant] -->|Input Data| B[Qualtrics Survey]
    B -->|Raw Data| C[Qualtrics Plugin]
    C -->|Hash Data| D[Hashed Data]
    D -->|Send| E[Web Server]
    E -->|Process| F[Smart Contract Executor]
    F -->|Store Hash| G[Cardano Blockchain]
    E -->|Retrieve Hash| G
    E -->|Verify Data| H[User Interface]
    H -->|Display Results| I[Researcher]
```

This diagram details how data moves through the Qualdano system, from survey participant input to blockchain storage and verification.

## 3. Qualtrics Plugin Architecture
```mermaid
graph TD
    A[Qualtrics Survey] -->|Trigger| B[Plugin Workflow]
    B -->|Extract Data| C[Data Extractor]
    C -->|Raw Data| D[Data Hasher]
    D -->|Hashed Data| E[Data Sender]
    E -->|Send to Server| F[Web Server]
    G[Configuration Module] -->|Settings| B
    H[Error Handler] -->|Manage Errors| B
    I[Logging Module] -->|Log Activities| B
```

This diagram shows the internal structure of our Qualtrics plugin, including components for data extraction, hashing, and sending to our web server.

## 4. Web Application Architecture
```mermaid
graph TD
    subgraph "Frontend"
        A[Pages] -->|Render| B[Components]
        C[API Routes] -->|Fetch Data| B
    end
    subgraph "Backend"
        C -->|Process Requests| D[Controllers]
        D -->|Business Logic| E[Services]
        E -->|Data Access| F[Models]
    end
    G[Database] <-->|Query/Store| F
    H[Blockchain Adapter] <-->|Interact| E
    I[Authentication Module] -->|Secure| C
    J[Error Handling] -->|Manage| D
```

This diagram illustrates the architecture of our Next.js web application, showing both frontend and backend components and their interactions.

## 5. Database Schema
```mermaid
erDiagram
    RESEARCHER ||--o{ SURVEY : creates
    SURVEY ||--|{ RESPONSE : has
    RESPONSE ||--|{ DATA_POINT : contains
    DATA_POINT ||--|| HASH : generates
    HASH ||--|| BLOCKCHAIN_RECORD : stored_as
    RESEARCHER {
        int id
        string name
        string email
    }
    SURVEY {
        int id
        string title
        date created_at
        int researcher_id
    }
    RESPONSE {
        int id
        int survey_id
        date submitted_at
    }
    DATA_POINT {
        int id
        int response_id
        string question
        string answer
    }
    HASH {
        int id
        int data_point_id
        string hash_value
    }
    BLOCKCHAIN_RECORD {
        int id
        int hash_id
        string transaction_id
        date recorded_at
    }
```

This diagram represents the structure of our database, showing tables and their relationships.

## 6. Cardano Smart Contract Architecture
```mermaid
graph TD
    A[Smart Contract] -->|Defines| B[Data Structures]
    A -->|Implements| C[Functions]
    B --> D[Hash]
    B --> E[Timestamp]
    B --> F[Researcher ID]
    C --> G[Store Hash]
    C --> H[Verify Hash]
    C --> I[Retrieve Hash]
    J[Web Server] -->|Calls| G
    J -->|Calls| H
    J -->|Calls| I
    K[Cardano Node] -->|Executes| A
    L[Blockchain] -->|Stores| M[Contract State]
```

This diagram shows the structure of our smart contracts on the Cardano blockchain, including data structures and functions.

## 7. API Endpoints Diagram
```mermaid
graph TD
    A[API] --> B[auth]
    A --> C[surveys]
    A --> D[hashes]
    A --> E[verify]
    B --> B1[POST login]
    B --> B2[POST logout]
    C --> C1[GET surveys]
    C --> C2[POST surveys]
    C --> C3[GET surveys/:id]
    D --> D1[POST hashes]
    D --> D2[GET hashes/:id]
    E --> E1[POST verify]
```

This diagram provides an overview of our API endpoints, showing the structure and available methods.

## 8. User Journey Map
```mermaid
graph TD
    A[Researcher Login] --> B{Create New Survey?}
    B -->|Yes| C[Design Survey in Qualtrics]
    B -->|No| D[Select Existing Survey]
    C --> E[Publish Survey]
    D --> E
    E --> F[Collect Responses]
    F --> G[View Results]
    G --> H{Verify Data Integrity?}
    H -->|Yes| I[Request Verification]
    I --> J[View Verification Results]
    H -->|No| K[Export/Analyze Data]
    J --> K
    K --> L[Logout]
```

This diagram illustrates the typical user journey through our system, from login to data verification.

## 9. Security Architecture
```mermaid
graph TD
    subgraph "User-facing Layer"
        A[HTTPS] --> B[WAF]
        B --> C[Load Balancer]
    end
    subgraph "Application Layer"
        C --> D[Authentication]
        C --> E[Authorization]
        D --> F[Session Management]
        E --> F
        F --> G[Input Validation]
    end
    subgraph "Data Layer"
        G --> H[Data Encryption]
        H --> I[Secure Data Storage]
    end
    subgraph "Blockchain Layer"
        I --> J[Smart Contract Security]
        J --> K[Blockchain Network Security]
    end
    L[Security Monitoring] --> A
    L --> C
    L --> F
    L --> I
    L --> K
```

This diagram shows the security measures implemented across different layers of our system.

## 10. Deployment Architecture
```mermaid
graph TD
    subgraph "Client Side"
        A[Web Browser] --> B[CDN]
    end
    subgraph "Server Side"
        B --> C[Load Balancer]
        C --> D[Web Server Cluster]
        D --> E[Application Server Cluster]
        E --> F[Database Cluster]
        E --> G[Cardano Node]
    end
    subgraph "External Services"
        E --> H[Qualtrics API]
        I[Monitoring Services] --> D
        I --> E
        I --> F
        I --> G
    end
    J[DevOps Tools] --> D
    J --> E
    J --> F
    J --> G
```

This diagram illustrates how our system is deployed, including server clusters and external services.

## 11. Integration Diagram
```mermaid
graph TD
    A[Researcher] -->|Creates Survey| B[Qualtrics]
    B -->|Survey Data| C[Qualdano Plugin]
    C -->|Hashed Data| D[Qualdano Web App]
    D -->|Blockchain Tx| E[Cardano Blockchain]
    F[Participants] -->|Responses| B
    G[Analysis Tools] -->|Import Data| B
    D -->|Verification| G
    H[Institutional Review Board] -->|Approval| A
    I[Data Repositories] <-->|Data Exchange| D
```

This diagram shows how Qualdano integrates with existing research workflows and tools.

## 12. Blockchain Interaction Sequence Diagram
```mermaid
sequenceDiagram
    participant R as Researcher
    participant Q as Qualtrics
    participant P as Qualdano Plugin
    participant W as Web App
    participant S as Smart Contract
    participant B as Blockchain

    R->>Q: Create Survey
    Q->>P: Trigger Plugin
    P->>P: Hash Data
    P->>W: Send Hashed Data
    W->>S: Call Smart Contract
    S->>B: Store Hash
    B-->>S: Confirm Storage
    S-->>W: Return Transaction ID
    W-->>P: Confirm Blockchain Storage
    P-->>Q: Update Survey Metadata
    Q-->>R: Confirm Process Complete
```

This diagram details the sequence of interactions between system components and the blockchain during a typical operation.

## 13. Error Handling and Recovery Flow
```mermaid
graph TD
    A[Operation Start] -->|Execute| B{Error Occurs?}
    B -->|No| C[Operation Complete]
    B -->|Yes| D[Log Error]
    D --> E{Error Type}
    E -->|Network| F[Retry Operation]
    E -->|Data| G[Data Validation]
    E -->|Blockchain| H[Check Transaction Status]
    F -->|Retry Limit Reached| I[Alert Admin]
    G -->|Invalid Data| J[Request Data Correction]
    H -->|Transaction Failed| K[Resubmit Transaction]
    I --> L[Manual Intervention]
    J --> M[User Corrects Data]
    K -->|Resubmission Failed| I
    L --> A
    M --> A
```

This diagram illustrates how our system handles errors and the recovery processes in place.

## 14. Verification Process Flowchart
```mermaid
graph TD
    A[Start Verification] -->|Input Data| B[Hash Input Data]
    B --> C{Hash Exists on Blockchain?}
    C -->|Yes| D[Compare Timestamps]
    C -->|No| E[Verification Failed]
    D -->|Matches| F[Verification Successful]
    D -->|Doesn't Match| G[Potential Tampering Detected]
    E --> H[Log Result]
    F --> H
    G --> H
    H --> I[Notify User]
    I --> J[End Verification]
```

This diagram shows the step-by-step process of verifying data integrity using our system.

## 15. Component Dependency Diagram
```mermaid
graph TD
    A[Qualtrics Plugin] -->|Depends on| B[Web App API]
    B -->|Depends on| C[Authentication Service]
    B -->|Depends on| D[Data Processing Service]
    D -->|Depends on| E[Hashing Service]
    D -->|Depends on| F[Blockchain Service]
    F -->|Depends on| G[Cardano Node]
    B -->|Depends on| H[Database Service]
    I[User Interface] -->|Depends on| B
    I -->|Depends on| C
    J[Admin Dashboard] -->|Depends on| B
    J -->|Depends on| C
    K[Monitoring Service] -->|Monitors| A
    K -->|Monitors| B
    K -->|Monitors| F
    K -->|Monitors| G
```

This diagram illustrates the dependencies between different components of our system.

These diagrams provide a comprehensive overview of the Qualdano system architecture and processes. They serve as a reference for development, documentation, and understanding the system's structure and workflows.
