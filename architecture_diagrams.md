# Application Architecture Diagrams

## 1. On-Premise Application Diagram

```mermaid
flowchart TD
    UI["UI Front End<br><i class='fa fa-desktop'></i> (React)"]
    LoadBalancer["Load Balancer<br><i class='fa fa-random'></i>"]
    WebServer["Web Server<br><i class='fa fa-server'></i> (Flask)"]
    Cache["Cache<br><i class='fa fa-database'></i> (e.g., Redis)"]
    Database["Database<br><i class='fa fa-database'></i> (Postgres)"]
    Monitoring["Monitoring<br><i class='fa fa-eye'></i>"]

    UI -->|Sends requests| LoadBalancer
    LoadBalancer -->|Distributes requests| WebServer
    WebServer -->|Queries| Cache
    Cache -->|Retrieves data| Database
    WebServer -->|Queries| Database
    Monitoring -->|Monitors| WebServer
    Monitoring -->|Monitors| Database
flowchart TD
    subgraph IaaS Deployment
        direction TB
        LoadBalancer[Load Balancer]
        WebServerVM[Web Server VM]
        UIVM[UI Front End VM]
        DatabaseVM[Database VM]
        CacheVM[Cache VM]
        MonitoringVM[Monitoring VM]
        NSG[Network Security Group]

        UIVM -->|Sends requests| LoadBalancer
        LoadBalancer -->|Distributes requests| WebServerVM
        WebServerVM -->|Queries| CacheVM
        CacheVM -->|Retrieves data| DatabaseVM
        WebServerVM -->|Queries| DatabaseVM

        NSG --> WebServerVM
        NSG --> UIVM
        NSG --> DatabaseVM
        NSG --> CacheVM
        NSG --> MonitoringVM
    end
flowchart TD
    subgraph PaaS Deployment
        direction TB
        LoadBalancer[Azure Load Balancer]
        WebApp[App Service (Flask)]
        StaticWebApp[Static Web App (React)]
        ManagedDB[Azure Database for PostgreSQL]
        Cache[Azure Cache for Redis]
        Monitoring[Azure Monitor]

        StaticWebApp -->|Sends requests| LoadBalancer
        LoadBalancer -->|Distributes requests| WebApp
        WebApp -->|Queries| Cache
        Cache -->|Retrieves data| ManagedDB
        WebApp -->|Queries| ManagedDB

        Monitoring -->|Monitors| WebApp
        Monitoring -->|Monitors| ManagedDB
    end
flowchart TD
    subgraph SaaS Deployment
        direction TB
        WebApp[Third-Party SaaS Application (Flask)]
        StaticWebApp[Third-Party SaaS UI (React)]
        ManagedDB[Third-Party SaaS Database]
        Cache[Third-Party SaaS Cache]
        Monitoring[Third-Party SaaS Monitoring]

        StaticWebApp -->|Sends requests| WebApp
        WebApp -->|Queries| Cache
        Cache -->|Retrieves data| ManagedDB
        WebApp -->|Queries| ManagedDB

        Monitoring -->|Monitors| WebApp
        Monitoring -->|Monitors| ManagedDB
    end
