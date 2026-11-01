# Authentication & authorisation

Identity touches every request and is one of the first things an enterprise customer's security team will probe, so interviewers use it to test whether you can tell who a caller is from what they may do, and whether you know how each mechanism fails. Senior backend, Solutions Architect and Forward Deployed Engineer roles ask most, because integrating with a customer's identity provider is often the first real task on a deployment.

> Related: [Security & compliance](security-and-compliance.md) · [Multi-tenant SaaS](multi-tenant-saas.md) · [API design & microservices](api-design-and-microservices.md) · [Cloud, DevOps & Integration](../topics/08-cloud-devops-integration/README.md)

---

## A. Fundamentals — must answer crisply
- [ ] Authentication vs. authorisation: what does each answer, and why does mixing them up cause real bugs?
- [ ] What problem does OAuth 2.0 solve, and why is it not an authentication protocol on its own?
- [ ] Walk through the authorization code flow step by step. What does each party see, and why is there a code exchange at all?
- [ ] What does PKCE protect against, and why is it now recommended for all clients rather than only mobile apps? *(verify)*
- [ ] What does OIDC add to OAuth 2.0? What is the difference between an ID token and an access token, and who is each one for?
- [ ] SAML vs. OIDC: how do they differ, and why does SAML still appear in so many enterprise deployments?
- [ ] What is inside a JWT, what does the signature prove, and what does it not prove?
- [ ] JWT vs. opaque tokens: how does validation differ, and what are the trade-offs for revocation and latency?
- [ ] Server-side sessions vs. stateless tokens: when would you choose each for a web application?
- [ ] RBAC vs. ABAC vs. ReBAC: give a one-line definition and one requirement that fits each.

## B. Working depth
- [ ] Which claims must an API validate on every JWT, and how do you handle key rotation through JWKS? What do `alg` confusion attacks exploit?
- [ ] How do client credentials and the device code flow differ from the authorization code flow, and when is each the right choice? *(verify)*
- [ ] How does refresh token rotation work, how is reuse detected, and how do you avoid logging out a legitimate client that retries on a poor network?
- [ ] Session cookies: what do `HttpOnly`, `Secure` and `SameSite` each protect against, and when do you still need CSRF protection?
- [ ] Where should a single-page app keep its tokens? Compare browser storage, in-memory tokens and a backend-for-frontend. *(verify)*
- [ ] How do you design API keys: format, hashed storage, scopes, last-used tracking, rotation with overlap and leak detection?
- [ ] Walk through an SP-initiated SAML login. What must the service provider validate, and what are XML signature wrapping attacks?
- [ ] What does SCIM do that SSO does not, and what happens to a user's sessions and tokens when they are deprovisioned in the identity provider?
- [ ] SSO for enterprise customers: how do you handle domain verification, just-in-time provisioning, account linking and the login page for a user whose company has enforced SSO?
- [ ] Where do you enforce authorisation: gateway, service or database? How do you avoid checks scattered across the codebase?

## C. Senior / Principal depth (trade-offs, failure modes, scale, cost)
- [ ] A self-contained JWT has been stolen and has an hour left. What are your revocation options, and what does each cost in latency and complexity?
- [ ] When does RBAC stop working (role explosion), and how would you migrate to ABAC or ReBAC without rewriting every check?
- [ ] Zanzibar-style systems such as OpenFGA or SpiceDB: how do relationship tuples work, and what is the "new enemy" consistency problem? *(verify)*
- [ ] Policy engines such as OPA or Cedar vs. checks in application code: where does the logic live, and how do you test and audit it? *(verify)*
- [ ] How do you filter a list endpoint by permissions when a user can see 50,000 of 10 million records?
- [ ] How do you establish service-to-service identity: mTLS, SPIFFE, cloud workload identity and signed tokens? How do you retire long-lived shared secrets? *(verify)*
- [ ] How should a user's identity propagate through a chain of services, and how do token exchange and audience restriction prevent confused deputy problems? *(verify)*
- [ ] Design support-staff impersonation: approval, time limits, visibility to the customer, restricted actions, and how the audit log records both identities.
- [ ] What must an audit log record for authentication and authorisation events, and how do you make it tamper-evident and searchable without logging secrets?
- [ ] Build vs. buy for identity: when would you use Auth0, Cognito, Entra ID or Keycloak, and what does lock-in look like, including cost at scale? *(verify)*
- [ ] Step-up authentication and passkeys: when do you ask for more proof, and how does WebAuthn resist phishing where one-time codes do not? *(verify)*

## D. Scenario & design questions (whiteboard / take-home style)
- [ ] An engineer finds that changing the ID in a URL shows another customer's invoice. Walk through the fix, how you find similar bugs, and how you determine what was exposed.
- [ ] Design authentication and authorisation for a B2B SaaS product with per-organisation SSO, SCIM, custom roles and API keys.
- [ ] A customer's SAML login suddenly fails for all users on a Monday morning. How do you diagnose it, including certificate expiry and clock skew?
- [ ] An employee was removed from the customer's identity provider, but could still use the product for two days. Explain how, and redesign it.
- [ ] Your signing key may have been leaked. Describe the incident response and how you rotate without logging everyone out.
- [ ] Design permissions for a document system with folders, inheritance, sharing links and external guests.
- [ ] An AI agent or third-party integration must act for a user with limited scope. Design the delegation, consent, token lifetime and audit trail.

## E. Hands-on checks (things to be able to DO)
- [ ] Implement the authorization code flow with PKCE against a local identity provider such as Keycloak, without a high-level auth library.
- [ ] Validate a JWT by hand: decode it, fetch the JWKS, check the signature and claims, and show rejected tokens for each failure.
- [ ] Build refresh token rotation with reuse detection, and write a test proving that a replayed token revokes the token family.
- [ ] Add SP-initiated SAML or OIDC SSO to a small app, and inspect the assertion or tokens in browser developer tools.
- [ ] Model the same permissions in plain RBAC tables and in a ReBAC tool such as OpenFGA, then compare the queries and the code.
- [ ] Implement API keys with hashed storage, scopes and audit entries, and show that the raw key cannot be recovered from the database.

---

**Good answers show:**
- A clean separation of authentication, authorisation and session management, with precise use of terms such as ID token and access token.
- Thinking in attacks: token theft, replay, CSRF, confused deputy and broken object-level access, with a specific defence for each.
- Honest revocation trade-offs rather than treating stateless tokens as free.
- Enterprise reality: SSO per customer, SCIM deprovisioning, certificate rotation, clock skew and a support path when login breaks.
- An authorisation model that fits the product's requirements, with a migration path as they grow.
- Audit and least privilege built in, including for internal staff, service identities and impersonation.

**Prove it:** Build a small B2B notes app with two tenants: OIDC login with PKCE through Keycloak, per-tenant SSO settings, SCIM deprovisioning that kills live sessions, refresh rotation with reuse detection, scoped API keys and audited support impersonation. Model sharing with ReBAC, then write a test suite that attempts cross-tenant access, token replay and privilege escalation, and publish what it caught.
