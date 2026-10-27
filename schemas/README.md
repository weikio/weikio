# Schemas

Each file is the JSON schema of one document kind in Weik.io 2026.2.1. Weik.io serves the same schemas at `GET /api/v1/schemas/{kind}`, and `weikio validate` checks every document against the schema of its kind.

An editor that runs the YAML language server checks a file as you type and marks a misspelled field before `weikio apply` refuses it. Point the file at the schema of its kind with a comment on its first line:

```yaml
# yaml-language-server: $schema=https://raw.githubusercontent.com/weikio/weikio/2026.2.1/schemas/variable.json
apiVersion: weik.io/v1
kind: variable
metadata: { name: erp-url, project: finance, environment: production }
spec: { value: https://erp.example.internal }
```

[Document kinds](https://weik.io/docs/reference/kinds) describes every kind and its fields.
