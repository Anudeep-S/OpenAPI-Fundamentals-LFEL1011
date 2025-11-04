# The Role of HTTP in OpenAPI

## Overview
HTTP has become the universal protocol for delivering web APIs, largely due to its ubiquity, simplicity, and alignment with Internet standards.  
Most modern APIs are built on **HTTP** and use **JSON** for encoding request and response payloads.  
These conventions reflect the natural preferences of both API providers and consumers.  
**OpenAPI** builds on this foundation by providing a standardized way to describe HTTP-based APIs, particularly those following the **REST** architectural style.

---

## OpenAPI and HTTP
OpenAPI is designed primarily to describe HTTP-based APIs, mapping its components directly to HTTP concepts.  
This alignment allows OpenAPI to provide a familiar and intuitive framework for defining APIs.

### Mapping OpenAPI to HTTP

| HTTP Concept | OpenAPI Representation | Description |
|---------------|------------------------|-------------|
| **Uniform Resource Identifiers (URIs)** | **Paths Object** | URIs define the address of a resource. In OpenAPI, these are represented as paths within the `Paths Object`, containing one or more `Path Item Objects`. |
| **HTTP Methods** | **Operation Object** | Methods like `GET`, `POST`, `PUT`, and `DELETE` map to operations within a `Path Item Object`. Each defines how the API interacts with the resource. |
| **Parameters** | **Parameters Object** | Represents the various inputs an HTTP request can take, such as query parameters, headers, cookies, and path variables. |
| **Request Payloads** | **Request Body Object** | Defines the structure and content of data sent in HTTP requests (commonly in JSON). |
| **Response Codes** | **Responses Object** | Describes the expected HTTP response codes (`200`, `404`, etc.) and their meanings for each operation. |
| **Response Payloads** | **Response Object** | Details the structure and content type (e.g., JSON, XML) of responses returned by the API. |

This strong mapping between HTTP and OpenAPI makes it easy for developers to design, document, and consume RESTful APIs effectively.

---

## Supporting Technologies

OpenAPI relies on several key technologies to define and structure API descriptions:

- **JSON and YAML** – The OpenAPI document itself is a JSON object that can be serialized in either format.  
- **JSON Schema** – Provides the formal structure for describing request and response bodies, parameters, and headers via the `Schema Object`.  
- **Markdown (CommonMark)** – Enables adding readable text descriptions within the specification for human audiences.  

These supporting technologies provide a balance between **machine-readability** and **developer-friendly documentation**.

---

## Versions of OpenAPI

| Version | Year | Notes |
|----------|------|-------|
| **Swagger 2.0** | 2015 | Donated by SmartBear and rebranded as OpenAPI 2.0. |
| **OpenAPI 3.0** | 2017 | Major revision introducing reusable components and better JSON Schema alignment. |
| **OpenAPI 3.1** | 2021 | Latest stable version aligning fully with JSON Schema Draft 2020-12. |
| **OpenAPI 4.0 (Moonwalk)** | In Development | Planned for general release in 2024, focusing on simplification and new API paradigms. |

> Note: While object definitions vary slightly between versions, the mapping between HTTP and OpenAPI remains consistent.

---

## Structure of an OpenAPI Description

An OpenAPI document follows a structured layout defined by the **OpenAPI Specification**.  
Each component plays a distinct role in describing different aspects of the API.

### Core Objects in the Specification

1. **OpenAPI Object**  
   - The root of the document; defines the version of the specification and references other key objects.

2. **Info Object**  
   - Provides high-level metadata about the API, such as:
     - Title  
     - Summary  
     - Description  
     - License and Terms of Service  
     - Version (often following *Semantic Versioning*)  
   - Acts as the “identity” of the API description, ensuring consumers can align a versioned document to a specific API release.

3. **Paths Object**  
   - Lists all available API endpoints and their supported operations.
   - Paths is a Map that uses a URL to identify a given Path Item Object.
   - Each Path Item Object has one or more Operation Objects. These provide the HTTP request methods that are supported at the URL. Each Operation can be uniquely identified in a given OpenAPI description with the property operationId.
   - Each Operation, as well as identifying the supported HTTP methods, provides summary and description information and tags.
   - The Operations reference one or more parameters expressed as a Parameter Object and optionally a Request Body Object for HTTP methods that support sending a request payload.
   - Operations also implement a Responses Objects , which defines a map of possible HTTP return codes implemented by the Operation that may reference a Response Object. A default option can be provided as a catchall, as shown in the code snippet.
     

4. **Components Object**  
   - Contains reusable definitions for parameters, schemas, headers, responses, and security configurations.

---
Path Object - https://spec.openapis.org/oas/v3.1.0.html#paths-object
Operation Object - https://spec.openapis.org/oas/v3.1.0.html#operation-object
Parameter Object - https://spec.openapis.org/oas/v3.1.0.html#parameter-object
Request Body Object - https://spec.openapis.org/oas/latest.html#request-body-object
Response Object - https://spec.openapis.org/oas/v3.1.0.html#response-object
Security Schema Object - https://spec.openapis.org/oas/v3.1.0.html#security-scheme-object

---
## Key Takeaways
- HTTP is the foundational protocol for most modern APIs, and OpenAPI provides a standard way to describe them.  
- OpenAPI maps directly to HTTP semantics—URIs, methods, parameters, requests, and responses.  
- Supporting technologies like JSON, YAML, JSON Schema, and Markdown make the specification both readable and extensible.  
- The specification has evolved from Swagger 2.0 to OpenAPI 3.1, with version 4.0 (Moonwalk) in development.  
- The **Info Object**, **Paths Object**, and **Components Object** form the core structure of every OpenAPI document, defining how an API is described and versioned.

# Relationship Between Paths, Path Items and Operations

## Overview
Paths, Path Items, and Operations provide the binding between the URIs that are exposed, the methods supported at each URL, and the shape of the requests and responses for each operation.

## Object Relationship
The relationship between these objects can be described as follows:

- **OpenAPI Object**  
  Includes `openapi`, `info`, and `paths`.
  
- **Paths Object**  
  Connects to one or more endpoints such as `/endpoint1`, `/endpoint2`, `/endpoint3`.

- **Path Item Object**  
  Each path contains one or more HTTP methods such as `get`, `put`, `post`, and `delete`.

- **Operation Object**  
  Each method defines the details of an operation, including:  
  - `summary`  
  - `description`  
  - `requestBody`  
  - `responses`  
  - `operationId`

- **Response Object**  
  Connected to an Operation Object and defines the possible responses returned by the API.

## Reusability in OpenAPI 3.1
From version **3.1** of OpenAPI onward, a **Path Item Object** can be reused directly.  
This allows defining a combination of URL, HTTP method, parameters, and request/response bodies once and referencing it throughout the specification using the `pathItems` property in the **Components Object**.

This reusability supports:
- Consistency across APIs.  
- Simplified maintenance of shared endpoints.  
- Organization-wide templates (e.g., standard health-check endpoints).

This feature enables teams to efficiently reuse and standardize common API definitions across multiple services.


# Supported Security Requirements

## Overview
OpenAPI defines five supported security schemes using the **Security Scheme Object**, allowing APIs to describe and implement various authentication and authorization mechanisms.  
These schemes provide flexibility for securing APIs across different environments and use cases.

---

## 1. API Key (`apiKey`)
- Provides simple, coarse-grained credentials for identifying API consumers.  
- Can be passed in headers, query parameters, or cookies.  
- Easy to use but lacks strong security guarantees and standardization.  
- Still widely supported for basic authentication scenarios.

---

## 2. HTTP Authentication (`http`)
- References authentication mechanisms from the **IANA Authentication Scheme** registry.  
- Common types include:
  - **Basic Authentication:** Username and password encoded in headers.  
  - **Bearer Tokens:** Typically used with OAuth 2.0 for token-based access control.  
- Flexible but depends on secure transport (HTTPS).

---

## 3. Mutual TLS (`mutualTLS`)
- Enforces **two-way authentication** using x509 certificates between client and server.  
- Commonly used in **financial and enterprise-grade APIs** requiring high trust.  
- OpenAPI provides limited built-in metadata; implementers must specify certificate authorities and cipher details.

---

## 4. OAuth 2.0 (`oauth2`)
- Core standard for **delegated authorization** in the API economy.  
- Enables users to grant third-party applications controlled access without sharing credentials.  
- OpenAPI supports descriptions for key OAuth flows such as **Authorization Code**, **Client Credentials**, and **Implicit**.

---

## 5. OpenID Connect (`openIdConnect`)
- Extends OAuth 2.0 by adding **user identity verification**.  
- OpenAPI supports linking to **OpenID Connect Discovery metadata**, which can be programmatically parsed by clients.  
- Provides rich identity and authentication details for secure and automated integration.

---

## Key Takeaways
- OpenAPI supports a comprehensive range of security models, from simple API keys to advanced OAuth 2.0 and OpenID Connect frameworks.  
- The **Security Scheme Object** standardizes how security is described and integrated into API documentation.  
- Choosing the appropriate scheme depends on the API’s context, security requirements, and consumer trust model.



