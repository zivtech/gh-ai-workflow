## 🤖🛡️ GitHub Workflow for AI Code Quality & Security Check

This workflow uses [Claude Code](https://docs.anthropic.com/en/docs/claude-code) to provide automated code reviews with **false-positive filtering**, posting feedback directly on pull requests.

### ✨ Features

- **False-Positive Filtering** - Multi-angle review with confidence scoring (only 75+ reported)
- **Drupal Coding Standards** - Checks for common violations
- **Zivtech Git Conventions** - Validates commit message format
- **Security Checks** - Flags XSS, SQL injection, access issues
- **Interactive Mode** - Mention `@claude` in PR comments for follow-ups

---

### 🎯 How False-Positive Filtering Works

Inspired by Claude Code's code-review skill, the workflow:

1. **Reviews from 5 angles**:
   - CLAUDE.md / Drupal standards compliance
   - Obvious bugs (shallow scan)
   - Git history context
   - Commit conventions
   - Security issues

2. **Scores each issue 0-100**:
   - 0: False positive
   - 25: Can't verify
   - 50: Real but nitpicky
   - 75: Verified, impacts functionality
   - 100: Definitely real

3. **Filters out issues under 75**

4. **Ignores known false positives**:
   - Pre-existing issues
   - Linter/typechecker catches
   - Intentional changes
   - Lines not modified in PR

---

### 📝 Setup

1. **Copy the workflow** to `.github/workflows/ai-code-quality-check.yml`

2. **Add secrets** as repository or org secrets:
   - `ANTHROPIC_API_KEY` - Your Anthropic API key
   - `ZIVTECH_SKILLS_DEPLOY_KEY` - SSH deploy key for `zivtech/zivtech-claude-skills` (read access)

3. **Create the deploy key** (one-time setup):
   ```bash
   # Generate a new SSH key pair
   ssh-keygen -t ed25519 -f zivtech-skills-deploy -N "" -C "zivtech-skills-deploy-key"

   # Add the PUBLIC key to zivtech/zivtech-claude-skills as a deploy key
   # GitHub → zivtech-claude-skills → Settings → Deploy keys → Add

   # Add the PRIVATE key as a secret to your org/repo
   # GitHub → Your Repo → Settings → Secrets → ZIVTECH_SKILLS_DEPLOY_KEY
   cat zivtech-skills-deploy  # Copy this as the secret value
   ```

4. **Test** - Open a PR targeting `master` or `main`

---

### 🎯 Installed Skills

The workflow automatically installs these Claude skills:

| Skill | Source | Purpose |
|-------|--------|---------|
| **drupal-coding-standards** | `zivtech/zivtech-claude-skills` | Drupal PHP, Twig, JS, CSS standards |
| **zivtech-development-workflow** | `zivtech/zivtech-claude-skills` | Git branch/commit conventions |
| **drupal-security** | `madsnorgaard/agent-resources` | Security vulnerability checks |

Skills are read from `~/.claude/skills/` during each review.

---

### 🌐 Reviewed Paths

By default reviews files in:
- `web/modules/custom/`
- `web/themes/custom/`

Edit the prompt in the workflow to customize paths.

---

### 💬 Interactive Mode

Mention `@claude` in PR comments for follow-ups:

```
@claude Can you explain this issue in more detail?
```

---

### 📋 Review Output

**If issues found:**
```
### Code Review

Found 2 issues:

1. Missing leading backslash on Drupal:: call (Drupal standards)
   https://github.com/org/repo/blob/abc123/file.php#L42

2. Commit message missing ticket number (Zivtech conventions)

---
🤖 Generated with Claude Code
```

**If no issues:**
```
### Code Review

No issues found. Checked for bugs, Drupal standards, and commit conventions.

🤖 Generated with Claude Code
```

---

### 🔍 What Gets Checked

| Category | Checks |
|----------|--------|
| **Drupal Standards** | 2-space indent, `elseif`, `\Drupal::`, `$this->t()`, `Html::escape()` |
| **Zivtech Conventions** | `TICKET-123: Description` format, imperative mood |
| **Security** | XSS, SQL injection, access controls |
| **Bugs** | Logic errors, null checks, edge cases |

---

### 🔄 Migration from OpenAI

- Replace `OPENAI_API_KEY` with `ANTHROPIC_API_KEY`
- Remove `OPENAI_CODE_REVIEW_PROMPT` and `OPENAI_CODE_REVIEW_MODEL`

---

### ℹ️ References

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code)
- [Anthropic Console](https://console.anthropic.com)
- [Drupal Coding Standards](https://project.pages.drupalcode.org/coding_standards/)
