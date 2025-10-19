# JadeSec – Security Gate Action for CI/CD

**JadeSec – a security gate solution for CI/CD pipelines.**  
This GitHub Action sends the **full GitHub workflow context** to an external API for validation before allowing deployments or critical workflow steps.

It is designed to help teams enforce security, compliance, and custom validation rules before sensitive operations.

---

## 🔒 Features

- Sends full GitHub context including:
  - Repository name & owner
  - Event type (`push`, `pull_request`, `workflow_dispatch`, etc.)
  - Branch, tag, and commit SHA
  - Actor (who triggered the workflow)
  - Pull request information (number, title, target branch)
  - Release info (tag name, draft/release)
- Receives a JSON response from your API:
  - `{ "status": "ok" }` → workflow continues
  - Any other response → workflow fails
- Easy to integrate into any workflow
- Masks API keys to keep secrets safe in logs

---

## ⚙️ Inputs

| Input | Required | Description |
|-------|----------|-------------|
| `api_url` | ✅ | The endpoint of the external Gate API that validates the workflow. |
| `api_key` | ✅ | API key for authentication. **Pass via GitHub Secrets**. |

---

## 🧪 Example Usage

```yaml
name: Deploy Workflow
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: JadeSec Gate Validation
        uses: jadeSec/agent-gate@v1
        with:
          api_url: "https://gate.example.com/validate"
          api_key: ${{ secrets.JADESEC_GATE_KEY }}

      - name: Deploy
        run: |
          echo "🚀 Deployment authorized by JadeSec Gate!"
```