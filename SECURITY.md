# Security Policy

## Supported versions

| Version | Supported |
|---------|-----------|
| `@v1` (latest) | ✅ |
| Older tags | ❌ |

Always pin to `@v1` or a specific commit SHA in production workflows.

## Reporting a vulnerability

**Please do not open a public GitHub issue for security vulnerabilities.**

Report security issues by emailing **hello@skillbubble.com**. You should receive a response within 48 hours. If the issue is confirmed we will release a patch as soon as possible.

Please include:
- A description of the vulnerability
- Steps to reproduce
- Potential impact
- Any suggested fix (optional)

## Workflow security best practices

### Pin action versions

```yaml
# Good — pinned to a specific SHA
- uses: Skill-Bubble/skillbubble-integrations@f2c01a4

# Acceptable — pinned to a major version tag
- uses: Skill-Bubble/skillbubble-integrations@v1

# Avoid — floating ref
- uses: Skill-Bubble/skillbubble-integrations@main
```

### Use secrets, never hardcode credentials

```yaml
# Good
-H "Authorization: ${{ secrets.SKILLBUBBLE_API_KEY }}"

# Never do this
-H "Authorization: sk-sb-abc123..."
```

### Limit secret scope

- Create separate API keys per repo or environment
- Revoke keys you no longer use in [Settings → Integrations](https://www.skillbubble.com/settings/integrations)
- Use read-only keys where write access is not needed

### Restrict workflow permissions

Add this to your workflow to limit the default `GITHUB_TOKEN` permissions:

```yaml
permissions:
  contents: read
```

### Review suggestions before accepting

SkillBubble's default trust level is `ask` — write tool calls create pending suggestions in your inbox rather than applying changes directly. Keep this setting unless you have a specific reason to use `auto_accept`.
