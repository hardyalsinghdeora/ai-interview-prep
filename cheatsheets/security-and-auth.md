# Security and Auth Cheatsheet

Covers authentication and authorisation protocols, access models, the OWASP Top 10, encryption, key and secret management, zero trust and compliance terms.

> Related: [Security and compliance skill](../skills/security-and-compliance.md) · [Cloud, DevOps and integration](../topics/08-cloud-devops-integration/README.md) · [api-design.md](api-design.md) · [cloud-services.md](cloud-services.md) · [networking-and-web.md](networking-and-web.md) · [billing-and-payments.md](billing-and-payments.md)

## Core concepts

| Term | What it means |
|---|---|
| **Authentication** | Proving who someone or something is, usually abbreviated as AuthN. |
| **Authorisation** | Deciding what an authenticated identity may do, usually abbreviated as AuthZ. |
| **MFA** (Multi-Factor Authentication) | Requires two or more factor types, such as password and device, greatly reducing account takeover. |
| **Passkey** | A phishing-resistant sign-in credential based on public-key cryptography and FIDO2 standards, replacing passwords. |
| **SSO** (Single Sign-On) | One login grants access to many applications, improving usability and centralising control. |
| **IdP** (Identity Provider) | The system that authenticates users and issues identity assertions, such as Okta or Microsoft Entra ID. |
| **SP** (Service Provider) | The application that relies on an identity provider's assertion to grant access. |
| **SCIM** (System for Cross-domain Identity Management) | A standard for provisioning and removing users across applications automatically. |
| **Session** | Server-tracked login state, usually referenced by a cookie, which is easy to revoke. |

## OAuth and OIDC

| Term | What it means |
|---|---|
| **OAuth 2.0** (Open Authorisation 2.0) | A delegated authorisation framework that grants apps limited access without sharing the user's password. |
| **OIDC** (OpenID Connect) | An identity layer on OAuth 2.0 that adds login and a standard ID token. |
| **Resource owner** | The user or entity who can grant access to the protected data. |
| **OAuth client** | The application requesting access on behalf of the user or itself. |
| **Authorisation server** | Authenticates the user and issues tokens to the client. |
| **Resource server** | The API holding protected data, which accepts and validates access tokens. |
| **Scope** | A named permission limiting what an access token can do, supporting least privilege. |
| **Access token** | A short-lived credential presented to an API; it is meant for the API rather than the client. |
| **Refresh token** | A longer-lived credential used to get new access tokens, so it must be stored securely and rotated. |
| **ID token** (identity token) | An OIDC JSON Web Token describing the authenticated user, intended for the client rather than for APIs. |
| **Authorization code flow** | The client swaps a one-time code for tokens at the back end, the standard flow for user login. |
| **PKCE** (Proof Key for Code Exchange) | Binds a code to the requesting client, preventing interception; recommended for all clients using the code flow. |
| **Client credentials flow** | A service authenticates as itself with no user present, used for machine-to-machine calls. |
| **Device authorisation flow** | Lets input-limited devices such as TVs get tokens, with the user approving on another device. |

## SAML and tokens

| Term | What it means |
|---|---|
| **SAML** (Security Assertion Markup Language) | An XML-based standard for enterprise single sign-on, passing signed assertions from identity to service provider. |
| **SAML assertion** (Security Assertion Markup Language assertion) | A signed XML statement about a user's identity and attributes, whose validation must be done carefully. |
| **JWT** (JSON Web Token) | A compact signed token of claims that verifies without a database lookup, but is hard to revoke early. |
| **JWT claim** (JSON Web Token claim) | A field in the token payload, such as `sub`, `exp` or `aud`; check expiry, audience and issuer. |
| **JWS** (JSON Web Signature) | The signed form of a JWT: readable by anyone, tamper-evident, but not encrypted. |
| **JWE** (JSON Web Encryption) | The encrypted form, used when token contents must stay confidential. |
| **JWKS** (JSON Web Key Set) | A published set of public keys used to verify token signatures, which enables key rotation. |
| **`alg: none` attack** | Accepting unsigned tokens through a weak library setup; always enforce an allowed algorithm list. |

| Protocol | Purpose | Format | Typical use |
|---|---|---|---|
| OAuth 2.0 | Delegated authorisation | Tokens (format not fixed) | API access |
| OIDC | Authentication on OAuth | JWT ID token | Modern app login |
| SAML 2.0 | Authentication and SSO | XML assertion | Enterprise SSO |

| Sessions vs JWTs | Server sessions | JWTs |
|---|---|---|
| State | On the server | In the token |
| Revocation | Easy | Hard; use short expiry |
| Scaling | Needs a shared store | Verifies locally |

## Access models

| Term | What it means |
|---|---|
| **RBAC** (Role-Based Access Control) | Permissions attach to roles assigned to users, which is simple but can lead to role explosion. |
| **ABAC** (Attribute-Based Access Control) | Decisions use attributes of user, resource and context, offering finer control but harder auditing. |
| **ReBAC** (Relationship-Based Access Control) | Decisions follow relationships, such as document editor, as in Google's Zanzibar model. |
| **Least privilege** | Granting only the minimum access needed, which limits damage from mistakes or compromise. |
| **Separation of duties** | Splitting sensitive actions between people, which reduces fraud and error. |
| **Zero trust** | Never trust by network location: verify identity and device on each request, and assume breach. |
| **Defence in depth** | Layered controls, so that one failure does not expose the whole system. |
| **Microsegmentation** | Fine-grained network isolation between workloads, which limits lateral movement after a breach. |
| **mTLS** (mutual Transport Layer Security) | Both client and server present certificates, commonly used between services for identity and encryption. |

## OWASP Top 10

The 2025 edition from the OWASP (Open Worldwide Application Security Project) *(verify)*. The 2021 list still appears in many materials.

| Term | What it means |
|---|---|
| **A01 Broken Access Control** | Users act beyond their permissions, such as reading others' records; now includes server-side request forgery. |
| **A02 Security Misconfiguration** | Unsafe defaults, open storage or verbose errors, often caused by inconsistent hardening. |
| **A03 Software Supply Chain Failures** | Compromised or vulnerable dependencies, build systems or distribution, extending the older outdated-components category. |
| **A04 Cryptographic Failures** | Weak or missing encryption that exposes sensitive data. |
| **A05 Injection** | Untrusted input interpreted as commands, such as SQL injection or cross-site scripting; use parameterised queries and encoding. |
| **A06 Insecure Design** | Missing security controls at the design stage, which cannot be fixed by implementation alone. |
| **A07 Authentication Failures** | Weak login, credential stuffing or poor session handling that allows account takeover. |
| **A08 Software or Data Integrity Failures** | Trusting code or data without verification, such as unsafe deserialisation or unsigned updates. |
| **A09 Security Logging and Alerting Failures** | Too little logging or alerting, so that attacks go unnoticed. |
| **A10 Mishandling of Exceptional Conditions** | Poor error handling that fails open, leaks details or leaves inconsistent state. |

## Common attacks

| Term | What it means |
|---|---|
| **XSS** (Cross-Site Scripting) | Injected script runs in a victim's browser; prevent with output encoding and a Content Security Policy. |
| **CSRF** (Cross-Site Request Forgery) | Tricks a logged-in browser into sending an unwanted request; prevent with tokens and SameSite cookies. |
| **SSRF** (Server-Side Request Forgery) | Makes a server call attacker-chosen URLs, often to reach internal systems or cloud metadata. |
| **IDOR** (Insecure Direct Object Reference) | Changing an identifier exposes another user's data because access checks are missing. |
| **SQL injection** (Structured Query Language injection) | Input changes a database query's meaning; parameterised queries prevent it. |

## Encryption

| Term | What it means |
|---|---|
| **Symmetric encryption** | One shared key encrypts and decrypts; it is fast, so it is used for bulk data. |
| **Asymmetric encryption** | Public and private key pairs, used for key exchange and signatures; slower than symmetric encryption. |
| **AES** (Advanced Encryption Standard) | The dominant symmetric cipher; AES-256-GCM provides both encryption and integrity. |
| **RSA** (Rivest-Shamir-Adleman) | A long-established asymmetric algorithm used for signatures and key exchange. |
| **ECC** (Elliptic Curve Cryptography) | Asymmetric cryptography with smaller keys than RSA for comparable strength. |
| **Hashing** | A one-way fingerprint of data, used for integrity; it cannot be reversed with a key. |
| **Password hashing** | Deliberately slow, salted hashing such as Argon2 or bcrypt, which resists brute-force attacks. |
| **Salt** | Random data added per password, which defeats precomputed tables and identical-hash matching. |
| **HMAC** (Hash-based Message Authentication Code) | A keyed hash proving integrity and authenticity, commonly used for webhook signatures. |
| **Digital signature** | A private-key proof of origin and integrity, verifiable by anyone holding the public key. |
| **TLS** (Transport Layer Security) | Encrypts and authenticates network connections; version 1.3 is the modern default. |
| **PFS** (Perfect Forward Secrecy) | Session keys are temporary, so a later key leak cannot decrypt recorded traffic. |
| **Encryption at rest** | Protects stored data if disks or backups are exposed. |
| **Encryption in transit** | Protects data moving across networks from interception and tampering. |
| **Envelope encryption** | Data is encrypted with a data key, which a master key encrypts, allowing efficient rotation and scale. |
| **Tokenisation** | Replaces sensitive values with meaningless tokens, reducing systems in compliance scope. |

## Keys and secrets

| Term | What it means |
|---|---|
| **KMS** (Key Management Service) | A managed service that creates, stores and controls keys, with audit logs and access policies. |
| **HSM** (Hardware Security Module) | Tamper-resistant hardware that protects keys, often required for strict compliance needs. |
| **Key rotation** | Replacing keys on a schedule, which limits the exposure from any one key. |
| **BYOK** (Bring Your Own Key) | Importing or controlling your own key in a cloud service, giving greater customer control. |
| **Secrets manager** | A central, audited store for credentials, with access control and rotation support. |
| **Short-lived credentials** | Tokens that expire quickly, which sharply limits the value of a leaked credential. |
| **Secret scanning** | Automatically detecting committed credentials; any exposed secret must be rotated, not merely deleted. |

## Compliance

| Term | What it means |
|---|---|
| **SOC 2** (System and Organization Controls 2) | An attestation on controls against criteria for security, availability, processing integrity, confidentiality and privacy *(verify)*. |
| **SOC 2 Type I and Type II** (System and Organization Controls 2) | Type I checks control design at a point in time; Type II tests effectiveness over a period. |
| **ISO 27001** (International Organization for Standardization 27001) | A certifiable standard for an information security management system; current edition 2022 *(verify)*. |
| **GDPR** (General Data Protection Regulation) | EU privacy law with fines up to 4% of global turnover or EUR 20m, whichever is higher *(verify)*. |
| **Data controller and processor** | GDPR roles: the controller decides why data is used, the processor handles data on instructions. |
| **HIPAA** (Health Insurance Portability and Accountability Act) | US law protecting health data held by covered entities and their business associates *(verify)*. |
| **PHI** (Protected Health Information) | Identifiable health information regulated under HIPAA. |
| **BAA** (Business Associate Agreement) | A required HIPAA contract with vendors who handle protected health information. |
| **PCI DSS** (Payment Card Industry Data Security Standard) | Security rules for handling card data; current version 4.0.1 *(verify)*. |
| **DPDP Act** (Digital Personal Data Protection Act, 2023) | India's data protection law; Rules notified November 2025, with phased compliance over about 18 months *(verify)*. |
| **Data Fiduciary and Data Principal** | DPDP terms: the fiduciary decides purposes for data, and the principal is the person it concerns. |
| **DPDP penalties** (Digital Personal Data Protection penalties) | Set by the Data Protection Board of India, up to ₹250 crore for serious security failures *(verify)*. |
| **PII** (Personally Identifiable Information) | Data that can identify a person, which usually needs minimising, protecting and retention limits. |
| **Pseudonymisation** | Replacing identifiers so data needs extra information to re-identify; under GDPR it is still personal data. |
| **Anonymisation** | Irreversibly removing identification, which places data outside GDPR if truly achieved. |
