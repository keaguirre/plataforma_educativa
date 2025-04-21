#### Diseñar un diagrama de arquitectura en Visio (u otra herramienta similar) que incluya los principales servicios de Azure necesarios (App Services, Azure SQL, Blob Storage, Application Gateway, etc.).
```mermaid
%%{init: {'theme': 'default'}}%%
graph LR
    %% ------------------ Internet Layer -------------------
    subgraph Internet
        direction LR
        A[Navegador Web Estudiante/Docente]
    end

    %% ------------------ Azure Virtual Network -------------------
    subgraph Azure Virtual Network
        direction LR

        %% ------------------ Subnet Edge -------------------
            B[Azure App Gateway WAF]
        %% ------------------ Subnet APIM -------------------
            X[Azure API Management]
        AS[Azure App Service AutoScale]

        %% ------------------ Subnet Frontend AutoScale -------------------
        subgraph Frontend AutoScale Subnet
            direction LR
            C1[Frontend 1]
            C2[Frontend 2]
            C3[Frontend N]
        end

        %% ------------------ Subnet Backend AutoScale -------------------
        subgraph Backend AutoScale Subnet
            direction LR
            D1[Backend 1]
            D2[Backend 2]
            D3[Backend N]
        end

        %% ------------------ Subnet Data -------------------
        subgraph Data Subnet
            E[Azure SQL DB HA + Backup]
            F[Azure Blob Storage]
        end

        %% ------------------ Subnet Security -------------------
            KV[Azure Key Vault]

        A -- "<span style='background-color:#aaffaa;'>HTTPS</span>" --> B
        B -- "<span style='background-color:#aaffaa;'>WAF + Balanceo</span>" --> X
        X -- "<span style='background-color:#aaffaa;'>Proxy Seguro API (AAD Auth)</span>" --> AS
        AS -- "<span style='background-color:#fdd;'>Gestiona</span>" --> C1 & C2 & C3
        AS -- "<span style='background-color:#fdd;'>Gestiona</span>" --> D1 & D2 & D3
        C1 -- "<span style='background-color:#e0f8f8;'>API Call</span>" --> D1
        C2 -- "<span style='background-color:#e0f8f8;'>API Call</span>" --> D2
        C3 -- "<span style='background-color:#e0f8f8;'>API Call</span>" --> D3
        D1 -- "<span style='background-color:#e0e0ff;'>Consulta SQL</span>" --> E
        D2 -- "<span style='background-color:#e0e0ff;'>Consulta SQL</span>" --> E
        D3 -- "<span style='background-color:#e0e0ff;'>Consulta SQL</span>" --> E
        D1 -- "<span style='background-color:#a0daff;'>Almacenar Archivo</span>" --> F
        D2 -- "<span style='background-color:#a0daff;'>Almacenar Archivo</span>" --> F
        D3 -- "<span style='background-color:#a0daff;'>Almacenar Archivo</span>" --> F
        KV -- "<span style='background-color:#ffe082;'>Secrets / Tokens</span>" --> AS & D1 & D2 & D3
    end

    subgraph Azure Monitoring Services
        direction LR
        H[Azure Monitor]
        LA[Log Analytics]
    end

    subgraph Azure Disaster Recovery
        SR[Azure Site Recovery]
    end

    subgraph Azure Identity
        G[Azure AD]
    end

    H -- "Recopila métricas de" --> AzureVirtualNetwork
    LA -- "Recopila logs de" --> AzureVirtualNetwork
    SR -- "Protege" --> AzureVirtualNetwork
    AzureVirtualNetwork -- "Envía logs y métricas a" --> H & LA
    AzureVirtualNetwork -- "Es protegido por" --> SR
    AS -- "Envía logs y métricas a" --> H & LA
    B -- "Envía logs y métricas a" --> H & LA
    X -- "Envía logs y métricas a" --> H & LA
    C1 -- "Envía logs y métricas a" --> H & LA
    C2 -- "Envía logs y métricas a" --> H & LA
    C3 -- "Envía logs y métricas a" --> H & LA
    D1 -- "Envía logs y métricas a" --> H & LA
    D2 -- "Envía logs y métricas a" --> H & LA
    D3 -- "Envía logs y métricas a" --> H & LA
    E -- "Envía logs y métricas a" --> H & LA
    F -- "Envía logs y métricas a" --> H & LA
    G -- "<span style='background-color:#ffe082;'>Autentica</span>" --> X
    G -- "<span style='background-color:#ffe082;'>Autentica</span>" --> AS
    G -- "<span style='background-color:#ffe082;'>Autentica</span>" --> B


    %% ------------------ Estilos -------------------
    style A fill:#f9f,stroke:#333,stroke-width:1px
    style B fill:#aaffaa,stroke:#333,stroke-width:1px
    style X fill:#aaffaa,stroke:#333,stroke-width:1px
    style AS fill:#fdd,stroke:#333,stroke-width:1px
    style C1 fill:#e0f8f8,stroke:#333,stroke-width:1px
    style C2 fill:#e0f8f8,stroke:#333,stroke-width:1px
    style C3 fill:#e0f8f8,stroke:#333,stroke-width:1px
    style D1 fill:#e0e0ff,stroke:#333,stroke-width:1px
    style D2 fill:#e0e0ff,stroke:#333,stroke-width:1px
    style D3 fill:#e0e0ff,stroke:#333,stroke-width:1px
    style E fill:#cc99ff,stroke:#333,stroke-width:1px
    style F fill:#cc99ff,stroke:#333,stroke-width:1px
    style KV fill:#ffe082,stroke:#333,stroke-width:1px
    style H fill:#b2ebf2,stroke:#333,stroke-width:1px
    style LA fill:#b2ebf2,stroke:#333,stroke-width:1px
    style SR fill:#ffcc80,stroke:#333,stroke-width:1px
    style G fill:#ffe082,stroke:#333,stroke-width:1px
```
