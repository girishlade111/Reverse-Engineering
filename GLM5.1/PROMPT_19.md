# PROMPT_19.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_19: Authentication & Authorization Analysis (Optional)

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_19
- **Phase:** 2
- **Stage:** 9 of 10 (optional)
- **Dependencies:** ART-01 (PROMPT_01), ART-05 (PROMPT_05), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-15 (PROMPT_15), ART-16 (PROMPT_16).
- **Estimated Tokens:** 9000–15000 (when triggered) / 1500–2500 (when skipped)
- **Output Artifacts:** ART-19 (Doc) — Auth & Authorization Report (optional).
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Auth & Authorization Report artifact (ART-19) that identifies the subject repository's authentication model (session-based, token-based, OAuth, API-key, mTLS, SSO), reconstructs the authentication flow (login, token issuance, token validation, refresh, logout), identifies the authorization model (RBAC, ABAC, ACL, policy-based), identifies auth storage (cookies, headers, sessions), detects password hashing and MFA mechanisms, and documents auth-bypass risks for documentation (not exploitation) — when triggered by the presence of auth-related code; otherwise emit a `SKIPPED` completion record and proceed without producing ART-19.

---

## 3. When to Invoke

PROMPT_19 is OPTIONAL. Its dispatch is governed by the engagement's scope modifier (`MISSION.md` § 6) and by the trigger condition below.

### 3.1 Scope-Modifier Behavior

- **`SCOPE_FULL`** — PROMPT_19 is always dispatched; if the trigger condition does not fire, the prompt emits a `SKIPPED` completion record per § 3.3.
- **`SCOPE_CORE`** — PROMPT_19 is dispatched only if the trigger condition (§ 3.2) fires; if the trigger does not fire, the orchestrator skips PROMPT_19 entirely and records a `NOT_DISPATCHED` note in the engagement manifest.
- **`SCOPE_TRIAGE`** — PROMPT_19 is never dispatched.
- **`SCOPE_MODULE(target)`** — PROMPT_19 is dispatched only if the trigger condition fires within the target module's closure.

### 3.2 Trigger Condition

The trigger condition is satisfied when ANY of the following markers is detected in the in-scope source code or configuration:

- **Auth-library imports** — `passport`, `passport-*` strategies, `jsonwebtoken`, `jose`, `express-jwt`, `next-auth`, `lucia`, `auth.js`, `@auth/core`, `authlib`, `flask-login`, `flask-jwt-extended`, `django.contrib.auth`, `django-rest-framework-simplejwt`, `django-allauth`, `python-social-auth`, `fastapi-security`, `Spring Security` (`spring-boot-starter-security`), `Apache Shiro`, `Keycloak` adapters, `Nimbus JOSE`/`JWT`, `JWT` (Java), `devise`, `warden`, `omniauth`, `rails-api-auth`, `cancancan`, `pundit`, `Knock`, `Auth0` SDKs, `Clerk` SDKs, `Firebase Auth`, `AWS Cognito` SDKs, `Supabase Auth`, `Ory` SDKs, `Casbin`, `Oso`, `Microsoft.AspNetCore.Authentication.*`, `IdentityServer4`, `OpenIddict`, `ASP.NET Core Identity`.
- **Auth-marker identifiers** — case-insensitive identifier matches for `jwt`, `jwks`, `bcrypt`, `argon2`, `pbkdf2`, `scrypt`, `accessToken`, `refreshToken`, `idToken`, `sessionId`, `csrf`, `oauth`, `oidc`, `saml`, `ldap`, `totp`, `mfa`, `authn`, `authz`, `rbac`, `abac`, `acl`, `policy`, `permission`, `role`, `scope` in function/class/variable names.
- **Auth-schema files** — `oauth.yml`, `oauth.yaml`, `oidc.json`, `saml-metadata.xml`, `keycloak.json`, `cognito.json`, `casbin_model.conf`, `casbin_policy.csv`, `.well-known/openid-configuration`, `SAML` metadata files.
- **Auth-related API endpoints** — routes whose path matches `/login`, `/logout`, `/register`, `/signup`, `/auth`, `/token`, `/refresh`, `/oauth`, `/callback`, `/me`, `/profile`, `/password`, `/mfa`, `/verify`, `/permissions`, `/roles`.
- **Auth-related API auth requirements** — ART-15 records any API with `auth_requirement` other than `none` or `UNVERIFIED`.

### 3.3 Skipped Behavior

If the trigger condition does not fire under `SCOPE_FULL`, the prompt emits a `SKIPPED` completion record with the following fields:

```
COMPLETION_RECORD {
  prompt_id: PROMPT_19,
  status: "SKIPPED",
  artifacts_produced: [],
  quality_checks_passed: [],
  quality_checks_failed: [],
  open_questions: [],
  handoff_ready: true,
  notes: "Trigger condition not satisfied; no auth-related code detected. ART-19 not produced. Downstream consumers MUST treat ART-19 as ABSENT and not require it for handoff."
}
```

The orchestrator records `ART-19.status: NOT_PRODUCED` in the artifact registry. Downstream prompts that consume ART-19 (PROMPT_15, PROMPT_26) MUST degrade gracefully by treating ART-19 as `ABSENT` and recording the degradation in their own Open Questions.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-05 | Map | Entry-point catalog; auth bootstrap (passport initialization, Spring Security config) is recorded here. |
| ART-08 | Doc | Class catalog; auth-related classes (`User`, `Session`, `Token`, `Role`, `Permission`) are extracted from here. |
| ART-09 | Doc | Function catalog; auth-related functions (login handlers, token validators, password hashers) are detected by name and signature. |
| ART-15 | Doc | API catalog; auth-requirement field per API drives the authorization-model inference. |
| ART-16 | Doc | Middleware catalog; auth middleware (concern: auth) is the seed for auth-flow reconstruction. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22, R23, R33, R34 (auth-related authorization escalation). |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid sequence-diagram conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (`§ 4.5`) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). |

---

## 5. Instructions to AI Agent

1. Evaluate the trigger condition per § 3.2; IF not satisfied AND scope modifier is `SCOPE_FULL`, emit `SKIPPED` completion record per § 3.3 and halt. IF not satisfied AND scope modifier is `SCOPE_CORE` or `SCOPE_MODULE`, halt without emitting a completion record (the orchestrator already skipped dispatch). IF satisfied, continue.
2. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
3. Identify the authentication model per § 6.1 (session, token, OAuth, API-key, mTLS, SSO, hybrid).
4. Reconstruct the authentication flow per § 6.2 (login, token issuance, token validation, refresh, logout).
5. Identify the authorization model per § 6.3 (RBAC, ABAC, ACL, policy-based, hybrid).
6. Identify auth storage per § 6.4 (cookies, headers, sessions, server-side stores).
7. Detect password hashing per § 6.5 (algorithm, salt, work factor).
8. Detect MFA per § 6.6 (TOTP, SMS, email, hardware keys, biometric).
9. Detect auth-bypass risks per § 6.7 (for documentation, not exploitation).
10. Emit Mermaid sequence diagrams per § 6.8 for login, token-refresh, and logout flows.
11. Cross-check the auth catalog against ART-15's auth requirements per § 6.9; unaccounted auth-protected APIs are `CONTRADICTION` findings per R33.
12. Emit ART-19 per § 8 with full front-matter, per-flow sections, auth-storage catalog, hashing catalog, MFA catalog, bypass-risk register, traceability index, open questions.
13. Run the Quality Checks in § 9.
14. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Authentication-Model Identification

Identify the authentication model by inspecting auth-library usage and auth-storage patterns:

- **Session-based** — detected by session middleware (`express-session`, `cookie-session`, `django.contrib.sessions`, `rack-session`, `Spring Session`, `ASP.NET Session`). Sessions are server-side state keyed by a session ID in a cookie.
- **Token-based (JWT)** — detected by `jsonwebtoken`, `jose`, `jwt-go`, `pyjwt`, `jose4j`, `System.IdentityModel.Tokens.Jwt`. Tokens are self-contained, signed, and typically sent in `Authorization: Bearer <token>`.
- **Token-based (opaque)** — detected by opaque-token patterns: a random token string stored in a database or cache, validated by lookup. Detected by `tokens` table, `accessToken` columns, token-lookup middleware.
- **OAuth 2.0** — detected by `passport-oauth2`, `oauth2-proxy`, `Spring Security OAuth2`, `Authlib OAuth2`, `django-oauth-toolkit`, `Doorkeeper`, `IdentityServer4`, `OpenIddict`. OAuth flows: authorization-code, client-credentials, password, implicit (deprecated), device-code, refresh.
- **OIDC (OpenID Connect)** — detected by `openid-client`, `passport-openidconnect`, `oidc-go`, `mozilla-django-oidc`, `Spring Security Oauth2 Client`. OIDC is OAuth 2.0 + ID tokens.
- **API-key** — detected by `X-API-Key` header reads, `apiKey` columns, API-key middleware.
- **mTLS (mutual TLS)** — detected by TLS client-cert configuration (`requestCert: true`, `requireClientCert`, Spring `ClientCertAuth`, NGINX `ssl_verify_client on`).
- **SSO (SAML, SAML2)** — detected by `passport-saml`, `python3-saml`, `Spring Security SAML`, `ruby-saml`. SAML SSO has IdP-initiated and SP-initiated flows.
- **Basic auth** — detected by `Authorization: Basic <base64>` parsing.
- **Hybrid** — multiple models coexist (e.g., session-based for browser, JWT for API). Each model is recorded separately.

Each auth model records `auth_model_id` `AM-XX`, `kind`, `library`, `configuration_citation`, `token_kind` (jwt | opaque | session-id | api-key | client-cert | saml-assertion | NA), `external_idp: false|true` (for OAuth/OIDC/SAML).

### 6.2 Authentication-Flow Reconstruction

Reconstruct the authentication flow for every auth model. The flow consists of:

- **Login** — the endpoint and handler that authenticate credentials and issue a session/token. Detected by routes matching `/login`, `/token`, `/auth/signin`, `/oauth/token`. Records `login_endpoint` `A-XX` (from ART-15), `credential_validator_fn` `FN-XX`, `session_or_token_issuer_fn` `FN-XX`, `credential_storage` (the password-hash column or external IdP), `citation`.
- **Token issuance** — for token-based models, the function that creates and signs the token. Records `issuer_fn` `FN-XX`, `signing_algorithm` (HS256 | RS256 | ES256 | EdDSA | A128GCM | etc.), `signing_key_location` (env var | secret manager | JWKS), `claims` (standard: `iss`, `sub`, `aud`, `exp`, `iat`, `nbf`, `jti`; custom: list), `ttl`, `citation`.
- **Token validation** — the middleware or function that validates tokens on incoming requests. Detected by `express-jwt`, `passport-jwt`, `Spring Security OAuth2 Resource Server`, `AddJwtBearer` (.NET). Records `validator_fn` `FN-XX`, `validation_checks` (signature | expiry | issuer | audience | nonce), `jwks_url` (for RS256/ES256), `citation`.
- **Refresh** — the endpoint and handler that issue a new access token from a refresh token. Detected by routes matching `/refresh`, `/token/refresh`. Records `refresh_endpoint` `A-XX`, `refresh_token_validator_fn` `FN-XX`, `rotation_strategy` (one-time-use | reusable | sliding-window), `citation`.
- **Logout** — the endpoint and handler that invalidate the session/token. Detected by routes matching `/logout`, `/signout`. Records `logout_endpoint` `A-XX`, `invalidation_strategy` (server-side-session-delete | token-blocklist | client-side-clear-only), `citation`.

### 6.3 Authorization-Model Identification

Identify the authorization model:

- **RBAC (Role-Based Access Control)** — detected by `Role`/`Permission` entities, `@PreAuthorize("hasRole('ADMIN')")`, `@Roles('admin')` decorators, `role` columns, `user_role` join tables, `can? :manage, @user` (Rails Cancancan), `@requires_roles` decorators.
- **ABAC (Attribute-Based Access Control)** — detected by `@PreAuthorize` with attribute-based expressions (`@PreAuthorize("hasPermission(#entity, 'write')")`), OPA (Open Policy Agent) integration, AWS IAM conditions, XACML policies.
- **ACL (Access Control List)** — detected by `acl` tables, `canCanCan` ability definitions, `grant`/`deny` rules, `chmod`-style permission bits.
- **Policy-based** — detected by `Casbin`, `Oso`, `Cedar` (AWS), `Open Policy Agent (OPA)`, `AWS IAM policies`, `Google IAM policies`, custom policy engines. Each policy engine records `policy_files` (e.g., `casbin_policy.csv`), `policy_enforcement_fn` `FN-XX`.
- **Hybrid** — RBAC for coarse-grained access with ABAC for fine-grained access; ACL for resource-level with RBAC for role-level.

Each authorization model records `authz_model_id` `AZ-XX`, `kind`, `policy_engine` (when applicable), `policy_files` (when applicable), `enforcement_fn` `FN-XX`, `roles_catalog` (list of defined roles when RBAC), `permissions_catalog` (list of defined permissions), `citation`.

### 6.4 Auth-Storage Identification

Identify where auth state is stored:

- **Cookies** — `Set-Cookie` headers, `cookie-session`, `req.cookies.sessionId`, `HttpOnly`, `Secure`, `SameSite` attributes. Records `cookie_name`, `attributes` (HttpOnly | Secure | SameSite=Strict|Lax|None | Max-Age | Domain | Path).
- **Headers** — `Authorization: Bearer`, `X-API-Key`, `X-Auth-Token`. Records `header_name`, `format` (Bearer | Basic | raw).
- **Server-side sessions** — session stores: in-memory, Redis (`connect-redis`), Memcached, database (`connect-mongo`), file system. Records `session_store_kind`, `session_store_config_citation`.
- **Client-side storage** — `localStorage`, `sessionStorage`, `IndexedDB` (browser); `SharedPreferences` (Android); `Keychain` (iOS); `SecureStorage` (.NET). Records `storage_location`, `encryption_at_rest: true|false`.

### 6.5 Password-Hashing Detection

Detect password hashing:

- **bcrypt** — detected by `bcrypt.hash`, `bcrypt.compare`, `BCryptPasswordEncoder` (Spring), `bcrypt` (Python), `bcrypt-ruby`.
- **argon2** — detected by `argon2.hash`, `argon2.verify`, `Argon2PasswordEncoder` (Spring), `argon2-cffi` (Python), `argon2` (Rust).
- **pbkdf2** — detected by `crypto.pbkdf2`, `Pbkdf2PasswordEncoder`, `hashlib.pbkdf2_hmac`, `Rfc2898DeriveBytes`.
- **scrypt** — detected by `scrypt.hash`, `scrypt.verify`, `crypto.scryptSync`, `SCryptPasswordEncoder`.
- **Legacy / insecure** — `md5`, `sha1`, `sha256` (without salt or with insufficient iterations) are flagged `INSECURE_HASH` with severity MAJOR.

Each hashing mechanism records `hash_id` `HS-XX`, `algorithm`, `work_factor` (bcrypt cost | argon2 time/memory/parallelism | pbkdf2 iterations), `salt_strategy` (per-user random | global | none), `citation`.

### 6.6 MFA Detection

Detect Multi-Factor Authentication:

- **TOTP (Time-based One-Time Password)** — detected by `speakeasy`, `otplib`, `python-pyotp`, `GoogleAuthenticator`, `otpauth`. Records `totp_issuer`, `totp_digits`, `totp_period`.
- **SMS OTP** — detected by Twilio/Nexmo/SNS integration with OTP patterns. Records `sms_provider`, `otp_length`, `otp_ttl`.
- **Email OTP** — detected by `nodemailer`/`sendgrid` integration with OTP patterns. Records `email_otp_length`, `email_otp_ttl`.
- **Hardware keys (WebAuthn / FIDO2 / U2F)** — detected by `webauthn`, `fido2`, `@simplewebauthn/*`, `webauthn4j`. Records `relying_party_id`, `relying_party_name`, `attestation` (none | indirect | direct).
- **Biometric** — detected by `LocalAuthentication` (iOS), `BiometricPrompt` (Android), `WebAuthn` platform authenticators.
- **Recovery codes** — detected by `recovery_codes` table or column.

Each MFA mechanism records `mfa_id` `MF-XX`, `kind`, `enforcement_level` (required | optional | step-up), `enrollment_fn` `FN-XX`, `verification_fn` `FN-XX`, `citation`.

### 6.7 Auth-Bypass-Risk Detection

Detect auth-bypass risks for documentation purposes — these findings describe where the auth model could be misconfigured or where the implementation deviates from best practice. These findings are NOT exploitation instructions; they are documentation of risk for the End Consumer.

Risk categories:

- **Insecure token storage** — JWTs stored in `localStorage` (XSS-exposed) rather than `HttpOnly` cookies. Flagged with severity MAJOR.
- **Missing CSRF protection** — session-based auth without CSRF tokens. Flagged with severity MAJOR.
- **Weak signing key** — JWT signed with HS256 using a weak shared secret (length < 32 bytes) or with `algorithm: none`. Flagged with severity CRITICAL.
- **Missing token expiry** — tokens with no `exp` claim or `exp` set more than 24 hours in the future. Flagged with severity MAJOR.
- **Missing token validation** — token validators that skip signature validation, expiry check, or issuer check. Flagged with severity CRITICAL.
- **Insecure password hashing** — passwords hashed with MD5, SHA1, or unsalted SHA256. Flagged with severity CRITICAL.
- **Auth-bypass endpoints** — endpoints marked `no-auth-required` in ART-15 that handle sensitive operations (cross-reference ART-11's sensitive-flow register). Flagged with severity CRITICAL.
- **Missing rate-limiting on auth endpoints** — login/refresh endpoints without rate limits. Flagged with severity MAJOR.
- **Overly permissive CORS** — `Access-Control-Allow-Origin: *` combined with `Access-Control-Allow-Credentials: true`. Flagged with severity MAJOR.
- **Missing role checks** — admin-only operations without `@PreAuthorize` or equivalent. Flagged with severity MAJOR.

Each risk records `risk_id` `RK-XX`, `category`, `severity` (CRITICAL | MAJOR | MINOR | INFO), `description`, `affected_api_id` `A-XX` (when applicable), `citation`, `recommended_mitigation` (descriptive, not prescriptive per `MISSION.md` § 4 anti-goal).

### 6.8 Mermaid Sequence-Diagram Emission

Emit Mermaid `sequenceDiagram` diagrams per `OUTPUT_RULES.md` § 7:

- **Login flow** — `sequenceDiagram` showing client → login endpoint → credential validator → password hasher → token issuer → response. Each message carries a citation.
- **Token-refresh flow** — `sequenceDiagram` showing client → refresh endpoint → refresh-token validator → token issuer → response.
- **Logout flow** — `sequenceDiagram` showing client → logout endpoint → session/token invalidator → response.
- **Authorization decision flow** — `sequenceDiagram` showing request → auth middleware → token validator → policy engine → handler.
- **Auth-model overview** — `graph LR` showing every `AM-XX` auth model and the APIs it protects (cross-reference ART-15).

Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file.

### 6.9 Coverage Cross-Check

Cross-check the auth catalog against ART-15's auth requirements:

1. Compute `A_15_auth` = set of APIs in ART-15 with `auth_requirement != none`.
2. Compute `A_19_protected` = set of APIs in ART-19 inferred to be protected by the auth models cataloged.
3. Expected: `A_19_protected ⊇ A_15_auth`. APIs in `A_15_auth \ A_19_protected` are `COVERAGE_GAP` findings (auth-protected APIs that ART-19 did not associate with an auth model).
4. APIs in `A_19_protected \ A_15_auth` are APIs ART-19 protects that ART-15 marked `no-auth-required`; these are `CONTRADICTION` findings per R33 (typically auth-bypass-risk candidates per § 6.7).

---

## 7. Required Outputs

### ART-19 — Auth & Authorization Report

**Type:** Doc (optional).

**Acceptance Criteria (when produced):**

- AC-19.1: The artifact file exists at `<output_root>/artifacts/phase2/ART19_<engagement_id>_auth.md`.
- AC-19.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-19.3: The body contains: Executive Summary, Methodology, Auth Models, Auth Flows, Authorization Models, Auth Storage, Password Hashing, MFA, Auth-Bypass Risks, Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-19.4: Every auth model, flow, authorization model, storage, hashing mechanism, MFA mechanism, and risk cites its source.
- AC-19.5: Every Mermaid diagram is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-19.6: A `.mmd` sidecar file exists for every Mermaid block.
- AC-19.7: Every auth-bypass risk records its category, severity, citation, and recommended mitigation.
- AC-19.8: Coverage cross-check is recorded with no unresolved contradictions.

**Acceptance Criteria (when skipped):**

- AC-19.S1: A `SKIPPED` completion record is emitted with the note from § 3.3.
- AC-19.S2: No artifact file is produced; the artifact registry records `ART-19.status: NOT_PRODUCED`.

---

## 8. Output Templates

### 8.1 ART-19 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-19
artifact_type: Doc
producing_prompt: PROMPT_19
phase: 2
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
auth_models:
  - id: AM-01
    kind: session | token-jwt | token-opaque | oauth | oidc | api-key | mtls | sso-saml | basic | hybrid
    library: <name>
    configuration_citation: <file>:<line-range>
    token_kind: jwt | opaque | session-id | api-key | client-cert | saml-assertion | NA
    external_idp: false
auth_flows:
  - id: AF-01
    auth_model_id: AM-XX
    login:
      endpoint_id: A-XX
      credential_validator_fn: FN-XX
      session_or_token_issuer_fn: FN-XX
      credential_storage: <text>
      citation: <file>:<line-range>
    token_issuance:
      issuer_fn: FN-XX
      signing_algorithm: HS256 | RS256 | ES256 | EdDSA | A128GCM | NA
      signing_key_location: env-var | secret-manager | jwks | NA
      claims: [iss | sub | aud | exp | iat | nbf | jti | <custom>]
      ttl: <text>
      citation: <file>:<line-range>
    token_validation:
      validator_fn: FN-XX
      validation_checks: [signature | expiry | issuer | audience | nonce]
      jwks_url: <url> | NA
      citation: <file>:<line-range>
    refresh:
      endpoint_id: A-XX | NA
      refresh_token_validator_fn: FN-XX | NA
      rotation_strategy: one-time-use | reusable | sliding-window | NA
      citation: <file>:<line-range>
    logout:
      endpoint_id: A-XX
      invalidation_strategy: server-side-session-delete | token-blocklist | client-side-clear-only
      citation: <file>:<line-range>
authorization_models:
  - id: AZ-01
    kind: RBAC | ABAC | ACL | policy-based | hybrid
    policy_engine: <name> | NA
    policy_files: [<path>]
    enforcement_fn: FN-XX
    roles_catalog: [<role>]
    permissions_catalog: [<permission>]
    citation: <file>:<line-range>
auth_storage:
  - id: AS-01
    kind: cookie | header | server-session | client-storage
    storage_location: <text>
    cookie_name: <text> | NA
    cookie_attributes: [HttpOnly | Secure | SameSite=Strict | SameSite=Lax | SameSite=None | Max-Age | Domain | Path]
    header_name: <text> | NA
    format: Bearer | Basic | raw | NA
    session_store_kind: in-memory | redis | memcached | database | file-system | NA
    encryption_at_rest: true | false
    citation: <file>:<line-range>
password_hashing:
  - id: HS-01
    algorithm: bcrypt | argon2 | pbkdf2 | scrypt | md5 | sha1 | sha256 | other
    work_factor: <text>
    salt_strategy: per-user-random | global | none
    citation: <file>:<line-range>
    insecure_flag: true | false
mfa_mechanisms:
  - id: MF-01
    kind: totp | sms-otp | email-otp | webauthn | biometric | recovery-codes
    enforcement_level: required | optional | step-up
    enrollment_fn: FN-XX
    verification_fn: FN-XX
    citation: <file>:<line-range>
auth_bypass_risks:
  - id: RK-01
    category: insecure-token-storage | missing-csrf | weak-signing-key | missing-token-expiry | missing-token-validation | insecure-password-hashing | auth-bypass-endpoint | missing-rate-limit | permissive-cors | missing-role-check
    severity: CRITICAL | MAJOR | MINOR | INFO
    description: <text>
    affected_api_id: A-XX | NA
    citation: <file>:<line-range>
    recommended_mitigation: <text>
coverage_cross_check:
  apis_protected_by_art19: [A-XX]
  apis_with_auth_in_art15: [A-XX]
  coverage_gaps: [A-XX]
  catalog_only: [A-XX]
mermaid_sources:
  - diagram_id: D-01
    title: <text>
    sidecar_file: <relative-path>
    node_count: <int>
source_coverage:
  - path: <file_path>
    symbol_count: <int>
    line_range: <start-end>
open_questions:
  - id: OQ-01
    question: <text>
    blocking: true | false
traceability_index:
  - claim_id: C-01
    source: <file_path>:<line-range>
    symbol: <name>
sections:
  - id: S-01
    title: <string>
    claims: [C-XX]
---
```

### 8.2 ART-19 Body Skeleton

```markdown
# ART-19: Auth & Authorization Report

## 1. Executive Summary
## 2. Methodology
## 3. Auth Models
## 4. Auth Flows
   ### 4.1 AM-01: <name>
   #### Login Flow
   **Diagram D-01: Login Flow**
   ```mermaid
   sequenceDiagram
       participant C as Client
       participant L as LoginHandler
       participant V as CredentialValidator
       participant H as PasswordHasher
       participant I as TokenIssuer
       C->>L: POST /login (credentials) (file:line)
       L->>V: validate(credentials) (file:line)
       V->>H: verify(password, hash) (file:line)
       H-->>V: ok
       V-->>L: user
       L->>I: issue(user) (file:line)
       I-->>L: token
       L-->>C: 200 OK { token }
   ```
   #### Token Refresh Flow
   #### Logout Flow
## 5. Authorization Models
## 6. Auth Storage
## 7. Password Hashing
## 8. MFA
## 9. Auth-Bypass Risks
## 10. Coverage Cross-Check
## 11. Traceability Index
## 12. Open Questions
## 13. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks (when produced)

- **Q1. Coverage Check** — every auth-protected API in ART-15 is associated with an auth model in ART-19; threshold ≥ 0.90.
- **Q2. Citation Check** — ≥ 0.95 of auth models, flows, authorization models, storage, hashing, MFA, and risks cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no auth model in ART-19 contradicts ART-15's auth requirements or ART-16's auth middleware.
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED` auth model, signing key location, and policy enforcement has an Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.1 on a 5% sample of files yields the same auth model set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks (when produced)

- **Q-19.A. Auth-Model Evidence** — every `AM-XX` is backed by a `configuration_citation` evidencing the model (library import, configuration file, middleware registration).
- **Q-19.B. Flow Completeness** — every auth model has at least login and logout flows reconstructed; token-based models also have issuance, validation, and refresh flows.
- **Q-19.C. Hashing Insecurity** — every `insecure_flag: true` hashing mechanism has a corresponding `RK-XX` risk with severity `CRITICAL`.
- **Q-19.D. MFA Coherence** — every `MF-XX` MFA mechanism has both `enrollment_fn` and `verification_fn` or an Open Question.
- **Q-19.E. Bypass-Risk Coverage** — every category in § 6.7 has at least one check performed (either a finding is recorded or `category-not-applicable` is recorded with rationale).
- **Q-19.F. Mermaid Message Citation** — every message in the Mermaid sequence diagrams has a `file:line` citation.
- **Q-19.G. Sidecar Files** — every Mermaid block has a `.mmd` sidecar file.

### Skipped Checks (when skipped)

- **Q-19.S1. Trigger Not Satisfied** — the trigger condition (§ 3.2) is re-evaluated; if it now fires, the prompt is re-dispatched. If it does not fire, the `SKIPPED` record is conformant.
- **Q-19.S2. Downstream Degradation** — downstream prompts that consume ART-19 (PROMPT_15, PROMPT_26) record the degradation in their Open Questions.

---

## 10. Common Pitfalls

- Do not infer auth models from the presence of a `User` table; verify the actual auth-library usage and middleware per R22.
- Always record the signing algorithm for JWTs; `algorithm: none` is a CRITICAL finding, not a default.
- Do not conflate authentication with authorization; authentication verifies identity, authorization verifies permission, and the two have distinct models.
- Always record the policy files for policy-based authorization; an unspecified policy file leaves the authorization model unreviewable.
- Do not omit MFA because it is "optional"; optional MFA is still a finding and must be recorded.
- Always cross-check auth-protected APIs against ART-15; an auth-protected API not associated with an auth model is a `COVERAGE_GAP`.
- Do not record auth-bypass risks as exploitation instructions; the framework's anti-goal (`MISSION.md` § 4) forbids prescriptive recommendations, and the risk record is descriptive documentation only.
- Always record password hashing with work factor and salt strategy; an unspecified work factor leaves the security posture underspecified.
- Always cite the cookie attributes; missing `HttpOnly` or `Secure` flags are MAJOR findings.
- Do not infer `recommended_mitigation` from generic best practice; tailor the mitigation to the codebase's specific risk.
- Always emit `.mmd` sidecar files; PROMPT_25 re-renders diagrams from the sidecar source.
- Do not produce ART-19 when the trigger does not fire under `SCOPE_FULL`; the `SKIPPED` record is the conformant output.

---

## 11. Handoff Criteria

PROMPT_15 and PROMPT_26 consume ART-19 (when produced). Handoff requires ALL of:

- HC-19.1: ART-19 status is `REVIEWED` or `DRAFT` with orchestrator waiver, OR `SKIPPED` (when trigger does not fire under `SCOPE_FULL`).
- HC-19.2: Every auth model has at least login and logout flows.
- HC-19.3: Authorization models, auth storage, password hashing, and MFA are cataloged (or `NONE_DETECTED` with rationale).
- HC-19.4: Auth-bypass risks are recorded for every category in § 6.7 (or `category-not-applicable` with rationale).
- HC-19.5: Mermaid diagrams are emitted with `.mmd` sidecar files.
- HC-19.6: Coverage cross-check is recorded with no unresolved contradictions.
- HC-19.7: `repository_fingerprint_recheck` matches ART-01.
- HC-19.8: No `BLOCKING` open questions remain.

When ART-19 is `SKIPPED`, HC-19.2 through HC-19.6 are `NA`, and only HC-19.1, HC-19.7, and HC-19.8 apply.

---

## 12. Cross-References

- **Consumed by:** PROMPT_15 (API & Interface Documentation — uses ART-19 to validate auth requirements; degrades gracefully when ART-19 is `SKIPPED`), PROMPT_26 (Rebuild Guide — uses ART-19 as required content for the security runbook; degrades gracefully when `SKIPPED`).
- **Depends on:** ART-01 (PROMPT_01), ART-05 (PROMPT_05), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-15 (PROMPT_15), ART-16 (PROMPT_16).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33, R34 (authorization-override escalation when auth code is detected post-trigger).
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4, § 3.1, § 4, § 6, § 7.
- **Forward reference:** PROMPT_30 verifies that every `CRITICAL` auth-bypass risk in ART-19 is recorded as a finding in the QA report and that downstream consumers degrade gracefully when ART-19 is `SKIPPED`.

*End of PROMPT_19. Orchestrator may dispatch PROMPT_20 (if persistence code is detected) upon satisfaction of § 11, or skip to Phase 3 if both PROMPT_19 and PROMPT_20 are skipped.*
