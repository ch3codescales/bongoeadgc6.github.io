# Portfolio Site - Agent Guidelines

## Software Name Abstraction Policy

**All blog posts and content must use generic software/tool names instead of specific vendor product names.**

### Purpose
- Increase the content's longevity and broader applicability
- Avoid inadvertently promoting specific vendors
- Allow readers to map examples to their own tool choices
- Make content more resilient to tool changes or vendor outages

### Guidelines

When writing or editing blog posts, replace specific tool names with generic category names:

| Specific Tool | Generic Name(s) |
|---|---|
| the SCA tool | SCA tool, Supply Chain Analysis tool |
| the artifact repository | Artifact repository, artifact management solution |
| our Git platform | Git platform, source control platform |
| Docker | Container runtime, containerization platform |
| the container orchestration platform | Container orchestration platform |
| Datadog | Application monitoring solution, observability platform |
| Vault | Secrets management solution, key management system |
| Prometheus | Metrics collection system, monitoring system |

### Examples

**Before:**
> "the SCA tool is integrated into our CI/CD pipeline as a mandatory gate on every pull request."

**After:**
> "An SCA tool is integrated into our CI/CD pipeline as a mandatory gate on every pull request."

**Before:**
> "Our the artifact repository cache kept serving the malicious package long after npm pulled it."

**After:**
> "Our artifact repository cache kept serving the malicious package long after npm pulled it."

### When to Use Specific Names

Use specific tool names only when:
1. The tool's specific behavior, quirk, or limitation is essential to understanding the lesson
2. You're directly citing or crediting the tool's creators or documentation
3. You're discussing a specific vendor outage or incident as a case study (use it once for context, then generalize)

Example: "When the SCA tool experienced a service disruption, we learned that security tools can become single points of failure" — use the name once for context, then say "the SCA tool" thereafter.

### Enforcement

Before submitting blog posts for review:
- Search the content for vendor product names (use grep/rg to find them)
- Replace with the generic category name or description
- Update tags and metadata to reflect generic categories
- Verify no specific tool names appear in critical sentences

### Tag Policy

Tags should also use generic categories:
- `SCA Tool` instead of `the SCA tool`
- `Artifact Management` instead of `the artifact repository`
- `Git Platform` instead of `our Git platform`
- `Observability` instead of `Datadog`
