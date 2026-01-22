## 🤖🛡️ GitHub Workflow for AI Code Quality & Security Check

This workflow uses [Claude Code](https://docs.anthropic.com/en/docs/claude-code) via the official [Claude Code GitHub Action](https://github.com/anthropics/claude-code-action) to provide automated reviews of code changes for quality and security, posting AI-generated feedback directly on pull requests.

### ✨ Features

- **Drupal Coding Standards** - Enforces official Drupal coding standards from drupal.org
- **Zivtech Git Conventions** - Validates commit messages and branch naming
- **Security Analysis** - Identifies XSS, SQL injection, and access control issues
- **Accessibility Checks** - Reviews Twig templates for WCAG compliance
- **Interactive Mode** - Mention `@claude` in PR comments to ask follow-up questions

### Workflow Overview

- **Trigger:** Pull requests targeting `master` or `main`, or `@claude` mentions in PR comments
- **Analyzes:** Source code in `web/modules/custom/` and `web/themes/custom/` (configurable)
- **Excludes:** Dependencies, images, binaries, build outputs, and vendor code
- **Feedback:** Posts structured review comments with severity levels and code examples

---

### 📝 How to Add This Workflow to a Repository

1. **📄 Copy the Workflow**
    - Copy [.github/workflows/ai-code-quality-check.yml](.github/workflows/ai-code-quality-check.yml)
    - Save as `.github/workflows/ai-code-quality-check.yml` in your repository

2. **🔒 Set Up the API Key**
    - Add `ANTHROPIC_API_KEY` as an organization or repository secret
    - Get your API key from [console.anthropic.com](https://console.anthropic.com)
    - Go to **Settings → Secrets and variables → Actions → New repository secret**

3. **✅ Test the Workflow**
    - Open or update a PR targeting `master` or `main`
    - The AI-powered review runs automatically
    - Mention `@claude` in a PR comment for interactive follow-ups

---

### 🌳 Supported Trunk Branches

By default, this workflow runs on PRs targeting `master` or `main`:

```yaml
on:
  pull_request:
    branches:
      - master
      - main
  issue_comment:
    types: [created]
```

#### Changing the Trunk Branch

Update the `branches` list in your workflow YAML:

```yaml
on:
  pull_request:
    branches:
      - develop
      - staging
```

---

### 🌐 Customizing Reviewed Paths

By default, this workflow analyzes code in:
- `web/modules/custom/` (custom Drupal modules)
- `web/themes/custom/` (custom Drupal themes)

#### Override via Repository Variable

Set `CODE_REVIEW_PATHS` in **Settings → Secrets and variables → Actions → Variables**:

```
src/ custom/plugins/ lib/
```

Multiple paths are space-separated.

---

### 📦 Excluded Files

Set `CODE_REVIEW_FILE_EXCLUDE_REGEX` to exclude files from review. Default excludes:
- Images: `*.png`, `*.jpg`, `*.gif`, `*.svg`, `*.ico`
- Fonts: `*.woff`, `*.woff2`, `*.ttf`, `*.eot`
- Directories: `node_modules/`, `vendor/`, `dist/`, `build/`

Override by setting the variable at the repository level.

---

### 🏷️🔒 Variables & Secrets

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `ANTHROPIC_API_KEY` | Secret | ✅ Yes | [Anthropic API key](https://console.anthropic.com) |
| `CODE_REVIEW_PATHS` | Variable | No | Space-separated paths to review (default: `web/modules/custom/ web/themes/custom/`) |
| `CODE_REVIEW_FILE_EXCLUDE_REGEX` | Variable | No | Regex pattern for files to exclude |

#### Migration from OpenAI

If migrating from the previous OpenAI-based workflow:
- Replace `OPENAI_API_KEY` with `ANTHROPIC_API_KEY`
- Remove `OPENAI_CODE_REVIEW_PROMPT` and `OPENAI_CODE_REVIEW_MODEL` (no longer needed)
- Keep `CODE_REVIEW_PATHS` and `CODE_REVIEW_FILE_EXCLUDE_REGEX` (still supported)

---

### 💬 Interactive Mode

Mention `@claude` in any PR comment to interact with the AI reviewer:

```
@claude Can you explain the security concern in more detail?
```

```
@claude What's the best practice for caching this entity query?
```

The workflow responds directly in the PR conversation.

---

### 📋 Review Output Format

Claude structures reviews with:

| Section | Description |
|---------|-------------|
| **Summary** | Overview of changes and quality assessment |
| **Commit Messages** | ✅/❌ validation against Zivtech conventions |
| **Issues Found** | Categorized by severity (🔴 Critical, 🟠 Warning, 🟡 Suggestion) |
| **Code Examples** | Before/after snippets for each issue |
| **Accessibility** | WCAG concerns in Twig templates |
| **Positive Feedback** | Recognition of well-written code |

---

### 🔍 What Gets Reviewed

#### Drupal Coding Standards
- PHP formatting (2-space indent, `elseif`, trailing commas)
- Drupal-specific patterns (`\Drupal::`, `$this->t()`, render arrays)
- Security best practices (`Html::escape()`, parameterized queries)

#### Zivtech Git Conventions
- Commit message format: `TICKET-123: Description`
- Branch naming: `TICKET-123/short-description`
- Imperative mood, proper capitalization

#### Accessibility (WCAG)
- Disclosure widget patterns (heading wraps button)
- `aria-controls` / `id` pairing
- Proper escaping in Twig templates

---

### 🧰 How It Works

1. **Checkout** - Full git history for accurate diff analysis
2. **Claude Code Action** - Invokes Claude with Drupal + Zivtech standards
3. **Code Analysis** - Claude reads files, analyzes changes, checks commits
4. **Structured Review** - Posts formatted feedback as PR comment

Unlike the previous workflow, Claude Code can:
- Read related files for context (not just the diff)
- Understand the full codebase architecture
- Provide interactive follow-up responses

---

### 🔄 Comparison: Previous vs New Workflow

| Feature | OpenAI (Previous) | Claude Code (New) |
|---------|-------------------|-------------------|
| Lines of YAML | ~170 | ~140 |
| Interactive mode | ❌ | ✅ `@claude` mentions |
| Context awareness | Diff + changed files only | Full repo access |
| Custom prompts | Via org variable | Embedded in workflow |
| File exploration | ❌ | ✅ Can read related files |
| Tool access | ❌ | ✅ Bash, Glob, Grep, Read |

---

### ℹ️ References

- [Claude Code GitHub Action](https://github.com/anthropics/claude-code-action)
- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Anthropic Console](https://console.anthropic.com) (API keys)
- [Drupal Coding Standards](https://project.pages.drupalcode.org/coding_standards/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
