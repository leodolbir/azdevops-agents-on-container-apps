# API Portal Requirements

## Vision & Scope
- One-stop Azure DevOps–aligned experience where Gordian teams design or import APIs, enforce architecture guardrails, and publish discoverable, governed endpoints.
- Covers contract-first design, code-first onboarding, linting, lifecycle governance, and Azure DevOps pipeline automation for .NET, Python, and Angular ecosystems.
- Reduces integration friction, accelerates consumer onboarding, modernizes Cost Data API delivery, and showcases Gordian’s API maturity to partners.

## Stakeholders & Users
- **Platform Team**: Owns portal roadmap, lint policies, environment automation, and integrations.
- **API Producers**: Fourteen product teams designing, documenting, testing, and publishing APIs.
- **API Consumers**: Internal/external developers needing discovery, auth details, examples, and SDKs.
- **Security & Compliance**: Oversees authentication, authorization, audit trails, and policy adherence.
- **Support & Ops**: Maintains ownership records, incident logs, and version rollout communications.

## Core Capabilities
- **Catalog & Discovery**: Searchable index with domain tags, lifecycle state, deployment targets, auth requirements, usage examples, and known consumers.
- **Contract Design Workspace**: OpenAPI/AsyncAPI editor with live linting, reusable schema libraries, policy templates, and approval workflow for contract-first development.
- **Code-First Import & Alignment**: Spec ingestion from repos/pipelines, lint gap analysis, remediation guidance, and synchronization with existing services.
- **Design → Test → Version Flow**: Mock/auto-proxy spin-up, scenario validation, semantic versioning, changelog capture, and readiness gating before publication.
- **Git Integration**: Branching support, commit hooks, pull-request visualization, traceability to Azure Boards, and history diffing of specifications.
- **Pipeline Automation**: Trigger Azure DevOps workflows to run OpenAPI Generator, produce SDKs/docs, publish artifacts, and notify stakeholders.
- **Identity & Access**: Duende IdentityServer for authentication, Keycloak-backed authorization roles/scopes, token request samples, and access request handling.
- **Governance & Lifecycle**: Ownership matrix, SLA metadata, deprecation notices, consumer impact tracking, and automated compliance checks.

## Supporting Features
- **Best-Practice Playbooks**: Embedded architecture guides, service templates, reusable security and validation patterns, and policy change logs.
- **Consumer Enablement**: Self-service access requests, approval routing, SLA acknowledgment tracking, and onboarding tutorials.
- **Community & Support**: FAQs, announcement feed, Q&A threads, incident summaries, and feedback capture.
- **Documentation Enhancements**: Interactive consoles, language snippets, Postman collection downloads, and usage analytics overlays.

## Non-Functional Requirements
- **Security**: Enforce OAuth2/OIDC, RBAC, encrypted storage, audit trails, and compliance with SOC2/ISO standards.
- **Scalability & Availability**: Highly available Azure deployment (AKS/Container Apps) sized for concurrent authoring/testing sessions.
- **Performance**: Catalog search under one second, lint feedback under two seconds, mock environment provisioning under ten seconds.
- **Extensibility**: Pluggable rule engine, modular generator support, and configurable policy packs for future frameworks.
- **Observability**: Structured logging, metrics dashboards, alerting hooks, and integration with existing APM tools.
- **User Experience**: Responsive Angular UI aligned with Gordian branding, WCAG 2.1 AA accessibility, and intuitive navigation.

## Integrations & Dependencies
- **Azure DevOps**: Repos, pipelines, Boards, and optional wiki migration for legacy documentation.
- **CI/CD Quality Gates**: Pre-commit hooks, PR lint checks, and release validations tied to portal rules.
- **Identity Services**: Duende IdentityServer for login flows and Keycloak for authorization decisions.
- **API Gateways & Monitoring**: Azure API Management or equivalent for deployment status, metrics, and traffic policies.
- **Secrets & Configuration**: Azure Key Vault for securing mock/test credentials and pipeline tokens.
- **Communications**: Teams/Email notifications for lifecycle events, approvals, and incident broadcasts.

## Implementation Considerations
- **Architecture**: Angular SPA front-end, .NET/Python backend services, modular rule engine, containerized deployment managed via Bicep.
- **Data Model**: Entities for APIs, versions, environments, policies, consumers, owners, artifacts, and compliance records.
- **Governance**: Change board for lint rules, policy rollout procedures, and contract-first adoption milestones.
- **Migration Strategy**: Phase plan—catalog existing APIs, remediate lint issues, onboard teams to contract-first practices.
- **Success Metrics**: Share of APIs with validated contracts, lint compliance rate, consumer satisfaction, onboarding time reductions, SDK generation cadence.

## Risks & Mitigations
- **Adoption Resistance**: Provide hands-on training, starter templates, integration with existing workflows, and incentive KPIs.
- **Specification Debt**: Implement remediation backlog, autofix suggestions, and prioritized refactoring sprints.
- **Identity Integration Complexity**: Run PoC for Duende/Keycloak flows, document reference implementations, and stage rollouts.
- **Pipeline Load**: Cache generator artifacts, allow incremental rebuilds, and load-test burst scenarios.

## Next Steps
- Validate these requirements with platform, security, and product teams.
- Break the epic into MVP scope, features, and user stories with acceptance criteria.
- Draft technical design, integration diagrams, and environment plan.
- Prototype linting pipeline and Azure DevOps automation for a pilot API.
- Define training schedule, onboarding milestones, support model, and success reporting.
