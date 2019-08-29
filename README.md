## Authorization & Authentication

### Objective:

Outline the chosen approach for authorization and authentication for the clients of the `FHIR` services.

### Flow

![flow](./flow.png)

#### Sequence Diagram

```mermaid
sequenceDiagram
    participant FHIRClient
    participant SDO
    participant KeyCloak
    participant DataServicesREST
    FHIRClient->>SDO: request Authentication/RBAC Authorization
    loop AuthN_AuthZ
      SDO->>KeyCloak: OAUTH/OIDC Token
      SDO->>SDO: check RBAC
      SDO->>SDO: Multi-tenant validation check
    end
    Note right of SDO: Part of Kubernetes
    SDO->>DataServicesREST: pass through on success
    SDO-->>FHIRClient: failure status
    DataServicesREST->>FHIRClient: Successful response
```



### KeyCloak:

The approach will use `KeyCloak` server to provide `OAUTH`/`OIDC` token as well as perform RBAC authorization



### Long Term:

KeyCloak provides `social logins` as well, which will enable external clients to authenticate against other providers.