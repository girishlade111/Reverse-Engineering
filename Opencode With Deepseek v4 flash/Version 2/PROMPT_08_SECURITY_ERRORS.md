========================================================================
PROMPT 08: SECURITY AND ERROR ANALYSIS
========================================================================
Phase 8: Security, Error Handling, and Resilience Analysis
Enterprise Reverse Engineering Prompt Framework

========================================================================
OBJECTIVES
========================================================================

After completing this phase, you will have:
1. Complete security architecture documentation
2. All error handling patterns and strategies documented
3. Retry, fallback, and resilience patterns mapped
4. Input validation and sanitization audit
5. Dependency vulnerability assessment
6. Secure data handling practices audit
7. Logging, monitoring, and observability documentation
8. Incident response mechanisms documented

========================================================================
INPUTS
========================================================================

- API_CATALOG.md (from Phase 7)
- THIRD_PARTY_INTEGRATIONS.md (from Phase 7)
- AUTH_ARCHITECTURE.md (from Phase 7)
- ALGORITHM_CATALOG.md (from Phase 5)
- DECISION_TREES.md (from Phase 5)
- DATA_FLOW_DIAGRAMS.md (from Phase 4)
- VALIDATION_MATRIX.md (from Phase 4)
- All repository files

========================================================================
ACTIVITIES
========================================================================

ACTIVITY 8.1: ERROR HANDLING ARCHITECTURE

8.1.1. Identify error handling patterns:
    - Try/catch/finally blocks
    - Result types (Ok/Err, Either, etc.)
    - Error monads (Maybe, Either, etc.)
    - Exception hierarchies
    - Error middleware (Express, ASP.NET, etc.)
    - Global error handlers
    - Domain-specific error types

8.1.2. For each error handling pattern:
    - Location in codebase
    - Pattern type
    - What errors it handles
    - How errors are propagated
    - How errors are logged
    - How errors are presented to users
    - How errors are presented to operators

8.1.3. Create an error catalog:
    - Error type/class name
    - Error code (if any)
    - HTTP status code (if applicable)
    - Error message template
    - When this error occurs
    - Who handles it
    - User-visible message
    - Log level

8.1.4. Generate error flow diagrams showing:
    - Where errors originate
    - How they propagate through the system
    - Where they are caught and handled
    - What the user/operator sees

ACTIVITY 8.2: RETRY AND RESILIENCE PATTERNS

8.2.1. Identify all retry mechanisms:
    - Retry loops with backoff
    - Exponential backoff implementations
    - Jitter strategies
    - Retry budgets and limits
    - Circuit breaker implementations
    - Bulkhead/isolation patterns
    - Timeout configurations
    - Fallback mechanisms

8.2.2. For each resilience mechanism:
    - Location in codebase
    - Trigger conditions
    - Retry count and strategy
    - Backoff algorithm
    - Fallback behavior
    - Failure threshold
    - Half-open/recovery strategy
    - Monitoring/alerting

ACTIVITY 8.3: INPUT VALIDATION AND SANITIZATION AUDIT

8.3.1. Audit all input validation:
    - API input validation
    - Form validation
    - File upload validation
    - Database input sanitization
    - Command injection prevention
    - Path traversal prevention
    - XSS prevention
    - CSRF prevention
    - SQL injection prevention
    - NoSQL injection prevention
    - LDAP injection prevention
    - Header injection prevention

8.3.2. For each validation point:
    - Location
    - What is validated
    - Validation rules
    - Sanitization applied
    - Bypass mechanisms (if any)
    - Testing coverage

8.3.3. Identify missing validation:
    - Entry points without validation
    - Trust assumptions about input
    - Internal data crossing trust boundaries

ACTIVITY 8.4: SECURITY ARCHITECTURE REVIEW

8.4.1. Audit security mechanisms:
    - Authentication implementation
    - Authorization enforcement
    - Session management
    - TLS/SSL configuration
    - Secret management
    - Encryption at rest
    - Encryption in transit
    - Key management
    - Certificate management
    - Security headers (CSP, HSTS, X-Frame-Options, etc.)
    - CORS configuration
    - Rate limiting
    - Brute force protection

8.4.2. For each security mechanism:
    - Implementation location
    - Configuration
    - Strength/adequacy assessment
    - Known bypasses or weaknesses
    - Testing coverage

8.4.3. Identify potential security gaps:
    - Hardcoded credentials
    - Insecure direct object references (IDOR)
    - Missing access controls
    - Insecure deserialization
    - Server-side request forgery (SSRF)
    - Path traversal
    - Mass assignment
    - Open redirects
    - Security misconfiguration
    - Known vulnerable dependencies

ACTIVITY 8.5: SECURE DATA HANDLING

8.5.1. Identify sensitive data handling:
    - Personally Identifiable Information (PII)
    - Authentication credentials
    - API keys and secrets
    - Financial data
    - Health data
    - Session tokens
    - Encryption keys

8.5.2. For each sensitive data type:
    - Where it enters the system
    - How it is processed
    - How it is stored
    - How it is transmitted
    - How it is logged
    - How it is disposed
    - Access controls applied
    - Encryption applied
    - Data retention/deletion policies

ACTIVITY 8.6: LOGGING AND MONITORING

8.6.1. Identify logging infrastructure:
    - Logging framework and configuration
    - Log levels and their usage
    - Log format and structure
    - Log output destinations
    - Log rotation and retention
    - Structured vs. unstructured logging
    - Correlation IDs or trace IDs

8.6.2. Identify monitoring infrastructure:
    - Metrics collection (Prometheus, StatsD, etc.)
    - Health check endpoints
    - Alerting rules and thresholds
    - Dashboards and visualization
    - Distributed tracing
    - Error tracking (Sentry, etc.)
    - APM tools

8.6.3. For each observability component:
    - Implementation location
    - Configuration
    - What is measured/tracked
    - Alert thresholds
    - Escalation paths

ACTIVITY 8.7: DEPENDENCY VULNERABILITY ASSESSMENT

8.7.1. For each external dependency:
    - Check for known vulnerabilities (CVEs)
    - Check current version vs. latest
    - Check for unmaintained/abandoned packages
    - Check license compatibility
    - Check for deprecated APIs in use

8.7.2. Document:
    - Dependency name and version
    - Known vulnerabilities
    - Severity of vulnerabilities
    - Remediation options
    - Business impact of upgrading

ACTIVITY 8.8: INCIDENT RESPONSE MECHANISMS

8.8.1. Identify incident response capabilities:
    - Error alerting
    - Automated rollback mechanisms
    - Feature flag/disable mechanisms
    - Kill switches
    - Rate limit escalation
    - Emergency access procedures
    - Incident documentation templates

8.8.2. For each mechanism:
    - Location
    - Activation procedure
    - Expected outcome
    - Testing/documentation status

========================================================================
ANALYSIS METHODOLOGY
========================================================================

This phase requires SecurityScan methodology:

Read the code with a security mindset:
1. TRUST NO INPUT: Assume all external input is malicious.
2. FOLLOW THE DATA: Trace sensitive data through the system.
3. CHECK EVERY BOUNDARY: Every interface is an attack surface.
4. VERIFY ACCESS: Every operation should check authorization.
5. CHECK ERROR HANDLING: Errors often leak information or
   bypass security.
6. ASSESS DEFAULT CONFIGURATION: Defaults should be secure.

For each finding:
- Document the evidence from the code
- State the security implication
- Provide the location (file:line)
- Note the severity (critical, high, medium, low, info)

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT 8.1: ERROR_HANDLING_ARCHITECTURE.md
- Error handling pattern inventory
- Error catalog with codes and messages
- Error flow diagrams (Mermaid)
- Global error handler documentation

ARTIFACT 8.2: RESILIENCE_PATTERNS.md
- Retry strategy documentation
- Circuit breaker documentation
- Timeout and fallback patterns
- Mermaid resilience diagrams

ARTIFACT 8.3: VALIDATION_AUDIT.md
- Validation point inventory
- Validation rules documentation
- Missing validation report
- Sanitization audit

ARTIFACT 8.4: SECURITY_AUDIT.md
- Security architecture documentation
- Mechanism assessment
- Vulnerability findings
- Security gap analysis

ARTIFACT 8.5: DATA_HANDLING_AUDIT.md
- Sensitive data inventory
- Data flow and handling documentation
- Encryption and access control documentation

ARTIFACT 8.6: OBSERVABILITY.md
- Logging infrastructure documentation
- Monitoring infrastructure documentation
- Alerting rules documentation
- Dashboard descriptions

ARTIFACT 8.7: DEPENDENCY_VULNERABILITIES.md
- Vulnerability assessment per dependency
- Severity and remediation
- Priority recommendations

ARTIFACT 8.8: INCIDENT_RESPONSE.md
- Incident response mechanisms
- Activation procedures
- Testing/documentation status

========================================================================
QUALITY GATES
========================================================================

Before completing this phase, verify:

[ ] All error handling patterns are identified and documented.
[ ] Error catalog covers all system error types.
[ ] All retry/resilience mechanisms are documented.
[ ] Input validation audit is complete.
[ ] Security architecture is fully documented.
[ ] Sensitive data handling is audited.
[ ] Observability infrastructure is documented.
[ ] Dependency vulnerabilities are assessed.
[ ] Incident response mechanisms are documented.
[ ] Findings are labeled with severity and evidence.
[ ] Artifacts meet quality standards (score >= 4.0).

========================================================================
OUTPUTS TO NEXT PHASE
========================================================================

Pass to Phase 9:
- All artifacts from this phase

========================================================================
END OF PROMPT 08
========================================================================
