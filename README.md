# Poligin Community Policy Templates Hub

Welcome to the **Poligin Community Policy Templates Hub**! This directory contains curated, production-ready policy templates that can be directly imported into Poligin through the Poligin Web Dashboard or the REST API.

---

## 📁 Repository Structure

```tree
templates/
├── basic/
│   ├── 01-read-only-auditor.json              # Read-only audit & compliance officer access
│   └── 02-standard-operator.json              # Standard devops workspace file management
├── intermediate/
│   ├── 03-zero-trust-credential-guardrail.json # Deny override blocking credential/secret exfiltration
│   └── 04-mcp-tool-sandbox.json               # Sandbox restricting AI agent MCP tool calls
├── advanced/
│   └── 05-ai-prompt-risk-and-dynamic-dsl.json # Dynamic context evaluation with Fast-Path DSL
└── catalog/
    └── poligin-community-starter-pack.json     # All-in-one bundle containing all 5 templates
```

---

## 🚀 How to Import into Poligin

### Option 1: One-Click Public GitHub Raw Import (Dashboard)
1. Open the **Poligin Dashboard** at `http://localhost:3000` (or your deployed instance).
2. Navigate to **Policy Governance** -> **Policy Templates**.
3. Click **"Import Templates"** in the top action bar.
4. Select the **"From Public URL / GitHub"** tab.
5. Paste any public raw GitHub link, for example:
   ```url
   https://raw.githubusercontent.com/<user>/poligin-templates/main/catalog/poligin-community-starter-pack.json
   ```
6. Click **"Fetch & Preview"**, review the imported templates, and click **"Import Templates"**.

### Option 2: Copy & Paste JSON
1. Click on any `.json` file in this repository (e.g., `03-zero-trust-credential-guardrail.json`).
2. Copy the JSON content.
3. In the Poligin Dashboard -> **Policy Templates** -> **Import Templates**, select **"Paste JSON"**.
4. Paste the JSON and click **"Import Templates"**.

### Option 3: REST API Import (`curl`)
You can automate template importing via CI/CD pipelines or shell scripts:

```bash
# Import a bundle
curl -X POST "http://localhost:8181/v1/tenants/acme/templates/import?overwrite=true" \
  -H "Authorization: Bearer <POLIGIN_API_TOKEN>" \
  -H "Content-Type: application/json" \
  -d @templates/catalog/poligin-community-starter-pack.json
```

---

## 🛠️ Creating Your Own Policy Template

Each template complies with the `poligin/v1` schema:

```json
{
  "$schema": "https://poligin.dev/schemas/policy-template-v1.json",
  "schema_version": "poligin/v1",
  "kind": "PolicyTemplate",
  "template_key": "my-custom-guardrail",
  "name": "My Custom Guardrail",
  "description": "Explains what this template controls",
  "roles": ["developer"],
  "groups": ["engineering"],
  "actions": ["read", "write"],
  "resources": ["file:workspace/*"],
  "effect": "allow",
  "priority": 100,
  "conditions": "contains(actor.roles, \"developer\")"
}
```

### Supported Effects:
- `"allow"`: Permissive rule granting the specified actions on resources.
- `"deny"`: Restrictive guardrail overriding permissive rules when evaluated.
