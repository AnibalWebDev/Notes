# OAuth 2.0 RFC 6749 Notes

## Introduction

In the traditional client-server authentication model, the client requests an access-restricted resource (protected resource) on the server by authenticating with the server using the resource owner's credentials. To provide third-party applications access to restricted resources, the resource owner shares its credentials with the third party.

Instead of using the resource owner's credentials to access protected resources, the client obtains an **access token** — a string denoting a specific scope, lifetime, and other access attributes. Access tokens are issued to third-party clients by an authorization server with the approval of the resource owner. The client then uses the access token to access the protected resources hosted by the resource server.

---

## Roles

- **Resource Owner (RO):** The end-user who can grant access to a protected resource (PR).
- **Resource Server (RS):** Hosts the protected resource; accepts and responds to requests using access tokens (AT).
- **Client (CL):** Application making protected resource requests on behalf of the resource owner with their authorization.
- **Authorization Server (AS):** Issues access tokens to clients after authenticating the resource owner and obtaining authorization.

---

## Protocol Flow

### Main Steps

- **Authorization Request (AR)**
- **Authorization Grant (AG):**

An authorization grant is a credential representing the resource owner's authorization to access protected resources. It is used by the client to obtain an access token.

#### Grant Types (GT)

- **Authorization Code (AC):** Auth code obtained via AS intermediary between client and resource owner.
- **Implicit (IMP):** Client is issued an access token directly instead of an authorization code.
- **Resource Owner Password Credentials (ROPC):** Username and password used directly as an AG to obtain an access token.
- **Client Credentials (CLCD):** Used when the client is also the resource owner or is authorized to access protected resources directly.

---

### Figure 1 - OAuth 2.0 Authorization Flow

```
    +----+          +----+
A:  | CL |====AR===>| RO | - CL sends AR to RO
B:  |    |<===AG====|    | - CL recieves AG
    |    |          +----+
    |    |          +----+
C:  |    |====AG===>| AS | - CL requests AT by authenticating w/ AS & presenting AG
D:  |    |<===AT====|    | - AS authenticates CL & validates AG, if valid, issues AT
    |    |          +----+
    |    |          +----+
E:  |    |====AT===>| RS | - CL requests PR from RS and authenticates by presenting AT
F:  |    |<===PR====|    | - RS validates AT, if valid, serves PT
    +----+          +----+
```

> The preferred method for the client to obtain an authorization grant from the resource owner (steps A and B) is to use the authorization server as an intermediary.

---

## Access Tokens

Access tokens are used to access protected resources. They are strings representing authorization with specific scopes and durations, issued to the client.

---

## Refresh Tokens

- Issued by the authorization server to the client.
- Used to obtain a new access token when the current token becomes **invalid** or **expires**.

---

### Figure 2 - Refreshing Expired Access Tokens

```
    +----+                  +----+
A:  | CL |=======AG========>| AS | -
B:  |    |<======AT/RT=====>|    | -
    |    |        +----+    |    |
C:  |    |===AT==>| RS |    |    | -
D:  |    |<==PR===|    |    |    | -
E:  |    |===AT==>|    |    |    | -
F:  |    |<==INV==|    |    |    | -
    |    |        +----+    |    |
G:  |    |=======RT========>|    | -
H:  |    |<======AT/RT======|    | -
    +----+                  +----+
```