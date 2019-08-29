## Authorization & Authentication

### Objective:

Outline the chosen approach for authorization and authentication for the clients of the `FHIR` services.

Require minimal changes on the REST API application.

### Flow

![flow](./flow.png)

#### Sequence Diagram

```mermaid
sequenceDiagram
    participant FHIRClient
    participant Istio
    participant KeyCloak
    participant DataServicesREST
    FHIRClient->>Istio: request Authentication/RBAC Authorization
    loop AuthN_AuthZ
      Istio->>KeyCloak: OAUTH/OIDC Token
      Istio->>Istio: check RBAC
      Istio->>Istio: Multi-tenant validation check
    end
    Note right of Istio: Part of Kubernetes
    alt success
     Istio->>DataServicesREST: fetch content
     DataServicesREST->>FHIRClient: Successful response
    else failure
     Istio-->>FHIRClient: invalid credentials
    end
   
```



### KeyCloak:

The approach will use `KeyCloak` server to provide `OAUTH`/`OIDC` token as well as perform RBAC authorization



### Long Term:

KeyCloak provides `social logins` as well, which will enable external clients to authenticate against other providers.