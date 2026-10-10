# Source and scope provenance

## Conversation inputs used

- Visible Jarvis discussion: AI Business Executive, business onboarding, dynamic teams/configurations, safe tools, backend controls, proactive briefings and natural voice.
- Explicit choice of Dynamic Agent Team Generation.
- Explicit choice of Voice-first plus Executive Dashboard.
- Jarvis PRD v0.1 created 2026-10-09 and read for this handoff. Its technical suggestions were proposals, not automatic user agreement.
- Cloud/CV conversation: Azure deployment, Docker, storage, managed database, identities/secrets, CI/CD, monitoring and later IaC were proposed to demonstrate practical cloud engineering.
- Packaging request of 2026-10-10: current scope, learning priority, text-first implementation, complete Markdown package, repository memory, verification and no first-step paid requirement.
- Final clarification: use only the discussion here; do not add or change product direction.

Some earlier conversation was supplied as truncated excerpts. This package does not infer decisions from omitted text.

## Local reference documents read

`Master_AI_Engineer_Roadmap(1).docx`: broad AI engineering learning order, concepts and progress states. All modules in that document were listed as not started.

`AI_Engineer_Project_Master_Context(1).docx`: conceptual understanding, one coherent evolving project, meaningful checks and experience with RAG/evaluation. The package does not modify those documents or declare their modules complete.

Original DOCX/PDF files and the CV are intentionally not bundled because they are unnecessary to repository continuation and may contain personal data. This package uses only relevant learning context without names, contact details or private business records.

## Organization versus new scope

Detailed field names, state enums, task IDs, synthetic fixtures and failure contracts are proposed engineering defaults written to make the requested handoff actionable. They introduce no additional product goal. The guest-question scenario is a worked example, not an agreed industry restriction. Azure is a proposed target, not an accepted provider selection.

## Implementation references

These are official documentation pointers for verifying the discussed stack at implementation time. They do not contribute new product requirements, establish a pinned version or claim a working deployment.

- [FastAPI container deployment](https://fastapi.tiangolo.com/deployment/docker/)
- [PostgreSQL row security](https://www.postgresql.org/docs/current/ddl-rowsecurity.html)
- [Azure Container Apps overview](https://learn.microsoft.com/en-us/azure/container-apps/overview)
- [Azure Container Apps jobs](https://learn.microsoft.com/en-us/azure/container-apps/jobs)
- [Azure managed identities](https://learn.microsoft.com/en-us/azure/container-apps/managed-identity)
- [Azure secret management](https://learn.microsoft.com/en-us/azure/container-apps/manage-secrets)
- [Azure GitHub Actions deployment](https://learn.microsoft.com/en-us/azure/container-apps/github-actions)
- [GitHub OIDC with Azure](https://docs.github.com/en/actions/how-tos/secure-your-work/security-harden-deployments/oidc-in-azure)
- [Azure log monitoring](https://learn.microsoft.com/en-us/azure/container-apps/log-monitoring)

Recheck relevant documentation, supported versions, service availability and pricing when implementing the corresponding task. Documentation availability is not evidence that credentials or resources exist.
