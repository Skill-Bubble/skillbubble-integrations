<p align="center">
  <img src="./assets/logo.svg" alt="SkillBubble" width="80" />
</p>

# SkillBubble Integrations

Connect SkillBubble to your AI assistant or development workflow so your skill profile stays current as you build.

There are two ways to do this — pick the one that fits your situation.

---

## Option 1 — AI assistant (recommended)

Connect your personal AI assistant (Kiro, Claude, Copilot) to your SkillBubble profile via the MCP server. Your assistant can then read your profile, suggest skill updates based on what you're building, and keep your profile current as you work — all driven by you, in conversation.

**This is the primary integration.** It's personal, intentional, and you stay in control.

→ [Set up your AI assistant](#ai-assistant-setup)

---

## Option 2 — GitHub Sync

Connect your GitHub account in Settings and SkillBubble will automatically analyse your personal contribution history — commits, languages, repo activity — and generate skill suggestions in your inbox.

This is the recommended approach for automatic profile updates. It's cleaner than a CI/CD pipeline because SkillBubble pulls your data directly, scoped to you, with no keys in shared repos.

→ [Learn about GitHub Sync](#github-sync)

---

## ⚠️ Pipeline integrations — read this first

The GitHub Actions workflows in this repo let you trigger skill suggestions on push or PR. **They are designed for personal, single-developer repositories only.**

**Do not add a SkillBubble API key to a shared team repository.** If you do:
- Every push by every team member will trigger suggestions against *your* profile
- Your colleagues' work will appear as your skill activity
- You'll get flooded with irrelevant suggestions

If you want automatic updates from your work in a shared repo, use **GitHub Sync** (Option 2) instead — it reads only your personal contributions, not the whole team's.

If you have a legitimate single-user repo use case, the pipeline workflows are documented below.

---

## AI assistant setup

### Prerequisites

- A SkillBubble account — [sign up free](https://www.skillbubble.com)
- A Pro subscription for write and AI tools
- An API key — generate one in [Settings → Integrations](https://www.skillbubble.com/settings/integrations)

### 1. Get an API key

Sign in → [Settings → Integrations](https://www.skillbubble.com/settings/integrations) → **Create API key**.

### 2. Add to your client

Pick the config for your AI assistant from [`mcp-config/`](./mcp-config/):

| File | Client |
|------|--------|
| [`kiro.json`](./mcp-config/kiro.json) | Kiro IDE (`~/.kiro/settings/mcp.json`) |
| [`claude.json`](./mcp-config/claude.json) | Claude Desktop / Claude Code |
| [`copilot.json`](./mcp-config/copilot.json) | GitHub Copilot (VS Code — `.vscode/mcp.json`) |

Replace `sk-sb-your-api-key-here` with your key from Settings.

### 3. Try it

Ask your AI assistant:

> "Show me my SkillBubble profile — what are my top-rated skills?"

---

## What to ask your AI assistant

Once connected, your AI assistant has full access to your skill profile. Here are some prompts to get started:

**Profile review**
```
Show me my SkillBubble profile. What are my strongest skills?
```

```
Summarise my top skills for a CV profile summary. Keep it to 3 sentences.
```

```
What skills am I missing compared to a senior TypeScript engineer?
```

**Keeping your profile current**
```
I just finished building a React dashboard with Recharts and React Query.
Use suggestSkills with that context and update my profile.
```

```
Read my package.json and suggest skills for the libraries I'm actively using.
Then call createSkillRating for each relevant one.
```

```
Run: git diff main --stat
Then: git diff main -- '*.ts' '*.tsx' | head -300
What did I build in this branch? Suggest and apply skill updates.
```

**Gap analysis**
```
Call getSkillProfile, then run git log --oneline -20.
Compare what I've been building against my profile and suggest updates for anything I'm missing.
```

---

## Available tools

| Tool | Tier | Description |
|------|------|-------------|
| `getSkillProfile` | Free | Read skills with optional filtering by category, rating, or date |
| `getCertifications` | Free | List certifications, filter by issuer or active/expired status |
| `getFrameworks` | Free | View competency frameworks and progress |
| `getSkillHistory` | Free | See rating changes over time |
| `searchSkills` | Free | Search by name or category |
| `createSkillRating` | Pro | Add or update a skill rating |
| `updateSkillRating` | Pro | Update an existing rating by skill key |
| `createCertification` | Pro | Add a certification record |
| `createSkillPackage` | Pro | Create a custom skill package |
| `suggestSkills` | Pro | AI-powered skill suggestions from a context string — non-destructive |
| `suggestSkillPackage` | Pro | Suggest a skill package from a goal — non-destructive |

> `suggestSkills` and `suggestSkillPackage` return recommendations only — they never modify your profile automatically.

---

## GitHub Sync

GitHub Sync connects your GitHub account to SkillBubble via OAuth. Once connected, SkillBubble periodically analyses your personal contribution history — languages, commit activity, repo topics — and generates skill suggestions in your inbox.

**Why this is better than a pipeline:**
- Scoped to your contributions only, not the whole team's
- No API key in a shared repo
- Runs automatically, no workflow files to maintain
- Suggestions land in your inbox for review, same as MCP suggestions

You'll find it in [Settings → Integrations](https://www.skillbubble.com/settings/integrations).

> **On a team?** GitHub Sync is for personal profile updates based on your own contributions. If you want team-wide skill analysis — gap reports, sprint planning suggestions, assignment recommendations — that's coming in **Team AI Automation** (business subscriptions). [Learn more](#team-ai-automation-coming-soon).

---

## GitHub Actions (single-user repos only)

> ⚠️ **Personal use only.** See the [pipeline warning](#️-pipeline-integrations---read-this-first) above before using these.
>
> **Looking for team-wide automation?** Pipeline workflows are the wrong tool for that. Check out [Team AI Automation](#team-ai-automation-coming-soon) — it's built for exactly that use case, without the credential risks.

Ready-to-use workflows in [`github-actions/`](./github-actions/):

| Workflow | Trigger |
|----------|---------|
| [`on-merge.yml`](./github-actions/on-merge.yml) | Push to `main` |
| [`on-pr.yml`](./github-actions/on-pr.yml) | Pull request open/update |
| [`dependency-scan.yml`](./github-actions/dependency-scan.yml) | Manual or dependency file change |

**Setup:** Add these secrets to your repo (Settings → Secrets → Actions):

| Secret | Value |
|--------|-------|
| `SKILLBUBBLE_API_KEY` | Your personal API key from SkillBubble Settings |
| `SKILLBUBBLE_MCP_ENDPOINT` | `https://mcp.skillbubble.com/mcp` |

Or use the Action directly:

```yaml
- uses: Skill-Bubble/skillbubble-integrations@v1
  with:
    api_key: ${{ secrets.SKILLBUBBLE_API_KEY }}
    mcp_endpoint: ${{ secrets.SKILLBUBBLE_MCP_ENDPOINT }}
```

---

## Team AI Automation *(coming soon — business subscriptions)*

Team AI Automation brings skill intelligence directly into SkillBubble for your whole team — no MCP configuration required by individual members.

A company admin activates a shared Automation Account for the organisation. Team members opt in to share their skill data, and SkillBubble runs AI-driven analysis on a schedule and in response to key events:

- **Sprint planning** — "Your team has a gap in Terraform. 3 members recently upskilled — good time to tackle that infrastructure backlog."
- **Feature planning** — Describe a new feature and get assignment recommendations based on who has the strongest relevant skills.
- **Weekly gap reports** — Aggregated skill gap and upskill opportunity reports for team owners, with no individual ratings exposed.
- **New member onboarding** — Analysis runs automatically when someone joins the team.

Suggestions land in each member's existing Suggestions Inbox, labelled "Team AI", using the same accept/reject flow they already know.

**This is the right answer for teams** — not pipeline workflows with shared API keys. Available as part of the business subscription tier.

---

## Data and security

- All traffic is encrypted via HTTPS / TLS 1.2+
- API keys are scoped — read-only keys cannot call write tools
- Write tools respect your **MCP Trust Level** in Settings: in `ask` mode (default), write calls create pending suggestions for review rather than applying changes directly
- Revoke any key at any time in [Settings → Integrations](https://www.skillbubble.com/settings/integrations)

**Prompt injection risk:** LLMs can be manipulated by malicious content in files or commits. Keep the default `ask` trust level so all write calls require your review before being applied.

See [`SECURITY.md`](./SECURITY.md) for full details.

---

## Support

- Email: [hello@skillbubble.com](mailto:hello@skillbubble.com)
- Settings: [www.skillbubble.com/settings/integrations](https://www.skillbubble.com/settings/integrations)

---

## Contributing

This repository is maintained by the SkillBubble team. External contributions are not accepted at this time.

---

## Disclaimer

MCP clients can read and write to your SkillBubble profile with your API key's permissions. Keep the default `ask` trust level, review suggestions before accepting, and revoke keys you no longer use.
