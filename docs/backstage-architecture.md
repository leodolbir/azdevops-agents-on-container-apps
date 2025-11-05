# Backstage-Based API Developer Portal Architecture

## Overview
- Leverage Backstage as the core developer portal platform delivering catalog, documentation, search, and extensibility.
- Containerize Backstage frontend/backend services and deploy to Azure Kubernetes Service (AKS) or Azure Container Apps using Bicep-managed infrastructure.
- Persist portal metadata in Azure Database for PostgreSQL; store generated documentation assets in Azure Blob Storage; integrate with Azure Active Directory (Duende IdentityServer) for authentication and Keycloak for authorization policy enforcement.
- Treat the `api-contracts` Git monorepo as the system of record for OpenAPI/AsyncAPI specifications, reusable components, lint rules, and generator configs.

## High-Level Architecture
- **Backstage Frontend (React)** served via NGINX/Ingress with SSL termination in Azure Front Door or Application Gateway.
- **Backstage Backend** running Node.js/Express with plugins for catalog, scaffolder, Azure DevOps, linting, and mock proxy orchestration.
- **Metadata Stores**: PostgreSQL for Backstage catalog and plugin state; Redis cache for session management; Blob Storage for TechDocs.
- **Integrations**:
  - Azure DevOps Repos, Pipelines, Boards for source control, automation, and work tracking.
  - Duende IdentityServer for OIDC sign-in; Keycloak as authorization service referencing user/team scopes.
  - Azure API Management or other gateways supplying deployment status, metrics, and traffic policies.
  - Spectral/Redocly CLI for contract linting; OpenAPI Generator for client SDK creation.
  - Stoplight Prism/WireMock containers for contract-driven mock proxies.

## Catalog & Entity Modeling
- Register each API spec as a Backstage `API` entity pointing to its OpenAPI/AsyncAPI document in the monorepo.
- Use catalog annotations to surface owner team, lifecycle stage, deployment environments, linked pipelines, Duende/Keycloak configurations, known consumers, and support contacts.
- Model reusable schema libraries as `Component` entities and reference them across APIs to promote consistency.
- Maintain ownership and escalation metadata via Backstage Group entities synchronized with Keycloak/IdentityServer.

## Workflow Integration
- **Contract-First Scaffolding**: Backstage Scaffolder templates bootstrap repositories or branches pre-populated with approved lint configs, shared schemas, and pipeline YAML.
- **Linting & Compliance**: Custom backend plugin executes Spectral rules on spec updates, presenting pass/fail badges and remediation guidance directly within the Backstage UI.
- **Design → Test → Version Flow**: Scaffolder actions provision mock proxies (Prism/WireMock) in Azure Container Apps per branch, capture validation results, and orchestrate semantic version tagging.
- **Pipeline Automation**: Azure DevOps pipeline triggers exposed in Backstage regenerate SDKs, server stubs, and documentation; results displayed through the Azure DevOps plugin.
- **Governance & Lifecycle**: Scorecards and policy checks ensure every API has owner metadata, SLAs, deprecation plans, and validated contracts before publication.

## Out-of-the-Box Advantages Aligned with Requirements
- Backstage **Catalog & Discovery** provides searchable, taggable API listings without custom code.
- **TechDocs & API Docs** plugins render Markdown, OpenAPI, and AsyncAPI documentation, fulfilling interactive documentation requirements.
- Built-in **Scaffolder** accelerates contract-first templates and onboarding workflows.
- **Software Catalog** drives ownership visibility, team contacts, and lifecycle metadata natively.
- Existing **Azure DevOps**, **Auth**, and **RBAC** plugins shorten time-to-integrate with Gordian’s pipelines and identity providers.
- **Search** and **Home** plugins enhance discoverability and onboarding experiences immediately.

## Extensions & Custom Plugins
- **Contract Linting Plugin**: Wrap Spectral/Redocly CLI to enforce best practices, display issues, and gate approvals.
- **Mock Proxy Orchestrator**: Trigger deployment of spec-driven proxies, manage lifecycle, and surface endpoints.
- **Compliance Dashboards**: Expose metrics for lint compliance, version coverage, consumer adoption, and SLA conformance.
- **Access Request Workflow**: Integrate with Keycloak/Duende to submit, approve, and track consumer access per API.

## Deployment Considerations
- Use GitHub Actions or Azure Pipelines to build Backstage containers, push to Azure Container Registry, and deploy via Helm/Flux or Bicep templates.
- Configure health probes, autoscaling policies, and centralized logging (Azure Monitor/Application Insights).
- Secure secret storage with Azure Key Vault, injecting credentials at runtime via Managed Identity.
- Set up CI/CD to refresh catalog entities from the monorepo automatically (e.g., scheduled sync job or Git trigger).

## Rollout Plan
- Phase 1: Stand up Backstage core, integrate identity, import existing API specs, enable TechDocs.
- Phase 2: Implement linting plugin, scaffolder templates, and Azure DevOps pipeline triggers; pilot with one API team.
- Phase 3: Add mock proxy automation, compliance dashboards, and consumer access workflows; onboard remaining teams.
- Phase 4: Optimize observability, analytics, and community features; formalize governance board for lint rule updates.
