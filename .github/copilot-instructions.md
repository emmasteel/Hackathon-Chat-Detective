# GitHub Copilot Instructions for Hackathon Chat Detective

## Project Overview

**Chat Detective** is an agentic AI tool designed for Microsoft Hackathon 2025 to assist law enforcement and intelligence teams in analysing chat transcripts. The application surfaces threats, identifies key entities, and provides actionable insights through a secure, audit-ready interface suitable for sensitive operational environments.

## Technology Stack

### Backend
- **Python 3.11+** with **FastAPI** for high-performance async APIs
- **Azure Functions** for hosting **FastAPI**
- **Azure AI Services** for AI capabilities
- **Azure Key Vault** for secrets management

### Frontend
- **TypeScript** with **React 18+**
- **Material-UI** or **Fluent UI** for consistent Microsoft ecosystem integration
- **React Query** for server state management
- **React Hook Form** with **Zod** validation

### Infrastructure
- **Azure Container Apps** for serverless deployment
- **Azure Database for PostgreSQL** with encryption at rest
- **Azure Application Insights** for monitoring and audit trails
- **Azure Entra ID** for identity and access management

## Core Principles

### Security-First Development
- **Zero Trust Architecture**: Assume no implicit trust, verify everything
- **Principle of Least Privilege**: Grant minimal necessary permissions
- **Defence in Depth**: Multiple layers of security controls
- **Secure by Default**: Security controls enabled from the start
- **Data Classification**: Handle sensitive law enforcement data appropriately

### DevSecOps Integration
- **Shift-Left Security**: Integrate security testing early in development
- **Infrastructure as Code**: Use Azure Bicep/ARM templates
- **Automated Security Scanning**: SAST, DAST, and dependency scanning
- **Audit Logging**: Comprehensive logging for compliance and forensics
- **Secrets Management**: Never commit secrets, use Azure Key Vault

### Clean Architecture Patterns
- **Hexagonal Architecture**: Separate business logic from external concerns
- **Domain-Driven Design**: Model the law enforcement and intelligence domain
- **CQRS Pattern**: Separate read and write operations for complex queries
- **Repository Pattern**: Abstract data access with clear interfaces
- **Dependency Injection**: Use FastAPI's built-in DI container

## Coding Standards

### Australian English
- Use Australian English spelling throughout (e.g., "analyse" not "analyze", "colour" not "color")
- Documentation and comments should follow Australian conventions
- Error messages and user-facing text in Australian English

### Code Style
- **Python**: Follow PEP 8, use `black` formatter, `ruff` linter
- **TypeScript**: Follow strict TypeScript config, use Prettier, ESLint
- **Comments**: Focus on *why* not *what*, explain business context
- **Naming**: Use domain-specific terminology from law enforcement context

### Error Handling
- **Fail-Fast Principle**: Detect errors early and fail explicitly
- **Structured Errors**: Use custom exception classes with error codes
- **Audit Trail**: Log all errors with correlation IDs for tracing
- **User-Friendly Messages**: Translate technical errors for end users

## Human-in-the-Loop Patterns

### Approval Workflows
- **Critical Decision Points**: Require human approval for high-impact actions
- **Confidence Scoring**: Surface AI confidence levels to users
- **Review Queues**: Implement approval workflows for sensitive operations
- **Escalation Paths**: Clear escalation when AI confidence is low

### User Interface Design
- **Progressive Disclosure**: Show complexity gradually based on user needs
- **Explanable AI**: Provide clear explanations for AI decisions
- **Feedback Loops**: Enable users to correct and improve AI outputs
- **Audit Trails**: Show decision history and human interventions

## Security Guidelines

### Authentication & Authorisation
```python
# Use Azure AD integration with role-based access
@requires_role("analyst")
@audit_action("view_transcript")
async def get_transcript(transcript_id: str, user: User = Depends(get_current_user)):
    # Implementation with proper authorisation checks
```

### Data Handling
- **Encryption**: Encrypt sensitive data at rest and in transit
- **Data Retention**: Implement appropriate retention policies
- **Anonymisation**: Support data anonymisation for training/testing
- **Access Logging**: Log all data access with user and timestamp

### Input Validation
```python
# Always validate and sanitise input data
class TranscriptAnalysisRequest(BaseModel):
    transcript_text: str = Field(..., min_length=1, max_length=100000)
    analysis_type: AnalysisType
    sensitivity_level: SensitivityLevel
    
    @validator('transcript_text')
    def sanitise_transcript(cls, v):
        return sanitise_sensitive_content(v)
```

## AI Best Practices

### Model Integration
- **Model Versioning**: Track and version ML models properly
- **A/B Testing**: Support gradual rollout of model improvements
- **Fallback Mechanisms**: Graceful degradation when AI services fail
- **Bias Monitoring**: Regular assessment of model and prompt bias and fairness

### Data Processing
- **Streaming Processing**: Handle large chat transcripts efficiently
- **Caching Strategy**: Cache expensive AI operations appropriately
- **Rate Limiting**: Protect against abuse of AI services
- **Cost Optimisation**: Monitor and optimise Azure Cognitive Services usage

## Monitoring & Observability

### Logging Strategy
```python
# Use structured logging with correlation IDs
logger.info(
    "Analysis completed",
    extra={
        "correlation_id": correlation_id,
        "user_id": user.id,
        "transcript_id": transcript_id,
        "analysis_duration_ms": duration,
        "threat_level": result.threat_level
    }
)
```

### Metrics & Alerts
- **Performance Metrics**: Response times, throughput, error rates
- **Security Metrics**: Failed authentication attempts, privilege escalations
- **Business Metrics**: Analysis accuracy, user engagement, case outcomes
- **Operational Metrics**: Resource usage, cost tracking, availability

## Testing Strategy

### Security Testing
- **SAST Integration**: Static analysis in CI/CD pipeline
- **Penetration Testing**: Regular security assessments
- **Dependency Scanning**: Monitor for known vulnerabilities
- **Secrets Scanning**: Prevent secret leakage in code

### Functional Testing
```python
# Example test for critical security functionality
@pytest.mark.security
async def test_unauthorised_access_blocked():
    """Ensure unauthorised users cannot access sensitive data."""
    response = await client.get("/api/transcripts/sensitive", headers=no_auth_headers)
    assert response.status_code == 401
    assert "correlation_id" in response.json()
```

## Deployment Guidelines

### Infrastructure
- **Blue-Green Deployments**: Zero-downtime deployments
- **Feature Flags**: Safe rollout of new capabilities
- **Disaster Recovery**: Multi-region backup and recovery procedures
- **Compliance**: Ensure deployments meet regulatory requirements

### Configuration Management
- **Environment Separation**: Clear separation of dev/test/prod environments
- **Secret Rotation**: Regular rotation of credentials and keys
- **Configuration Validation**: Validate configuration before deployment
- **Rollback Procedures**: Quick rollback mechanisms for issues

## Compliance & Audit

### Regulatory Compliance
- **Data Protection**: GDPR, Privacy Act compliance where applicable
- **Law Enforcement Standards**: Adhere to relevant law enforcement data standards
- **Audit Requirements**: Maintain comprehensive audit trails
- **Evidence Chain**: Preserve integrity of evidence for legal proceedings

### Documentation Requirements
- **Decision Logs**: Document significant architectural and security decisions
- **Runbooks**: Operational procedures for common scenarios
- **Incident Response**: Clear procedures for security incidents
- **Change Management**: Track and approve all changes to production systems

## Anti-Patterns to Avoid

### Code Quality
- **Avoid Boilerplate**: Use code generation and abstractions wisely
- **No Magic Numbers**: Use named constants with business meaning
- **Avoid Deep Nesting**: Keep code readable with early returns
- **No Hardcoded Secrets**: Always use secure configuration management

### Security Anti-Patterns
- **No Client-Side Security**: Never rely on client-side validation alone
- **Avoid Over-Privileged Access**: Grant minimal necessary permissions
- **No Logging Sensitive Data**: Sanitise logs of PII and sensitive content
- **Avoid Custom Crypto**: Use established cryptographic libraries

---

*This configuration ensures GitHub Copilot suggestions align with the secure, audit-ready requirements of law enforcement and intelligence applications while maintaining clean, maintainable code architecture.*
