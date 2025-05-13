# GitHub Workflows 🚀

This repository contains automated workflows used by Zivtech to improve engineering processes, code quality, and security.  
**All workflows in this repository are documented in this README.** Please update this file when new workflows are added.

> **Organization Variables:**  
> All `OPENAI_*` variables and secrets are set at the organization level by default. Individual projects may override these settings if necessary.

---

## AI Code Quality & Security Check 🤖🛡️

This workflow uses [OpenAI](https://platform.openai.com/docs/api-reference) models to provide automated reviews of code changes for quality and security, posting AI-generated feedback directly on pull requests related to custom Drupal modules and themes.

### Workflow Overview

- **Trigger:** Pull requests whose base (target) branch is the defined **trunk branch** (default: `master`).
- **Analyzes:** Source code in `web/modules/custom/*` and `web/themes/custom/*`.
- **Excludes:** Noise files such as dependencies, images, binaries, archives, VCS metadata, and generated assets.
- **Feedback:** Posts the OpenAI analysis as a PR comment for team review.

### Supported Trunk Branches (How to Specify PR Targets)

By **default**, this workflow only runs on PRs where the target (base) branch is `master`.

```yaml
on:
  pull_request:
    branches:
      - master
```

#### Changing the Trunk Branch

If your repository’s trunk branch is named something other than `master` (such as `main`, `develop`, or another convention), update the `branches` list under the `pull_request` trigger in your workflow YAML:

**For a different single trunk (e.g. `main`):**

```yaml
on:
  pull_request:
    branches:
      - main
```

**For multiple trunks:**

```yaml
on:
  pull_request:
    branches:
      - main
      - develop
```

- Only PRs targeting one of the specified trunks will trigger the review.
- Edit this in `.github/workflows/ai-code-quality.yml` under `on.pull_request.branches`.
- Commit and push the update for changes to take effect.

---

### How to Add This Workflow to a Repository 📝

1. **Copy the Workflow 📄**
    - Save the YAML file as `.github/workflows/ai-code-quality.yml`.

2. **Set Up Secrets & Variables 🔒🏷️**
    - `OPENAI_API_KEY`, `OPENAI_CODE_REVIEW_PROMPT`, and `OPENAI_CODE_REVIEW_MODEL` are **already configured organization-wide**.
    - You may override them for your repository in **Settings → Secrets and variables → Actions**.

3. **Test the Workflow ✅**
    - Open or update a PR targeting the configured trunk branch.
    - The AI-powered review runs automatically.

---

### Variables & Secrets 🏷️🔒

| Name                        | Set at                 | Description                                                                                                                                      |
|-----------------------------|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| `OPENAI_API_KEY`            | Org Secret             | [OpenAI API key](https://platform.openai.com/account/api-keys)                                                                                   |
| `OPENAI_CODE_REVIEW_PROMPT` | Org Variable           | Prompt for code reviews sent to OpenAI                                                                                                           |
| `OPENAI_CODE_REVIEW_MODEL`  | Org Variable           | OpenAI model name (e.g. `gpt-4o`, `gpt-4`). Check out [Models](https://platform.openai.com/docs/models) to compare the various available models. |

By default, **no further action is needed**, but you can override these settings per project if needed in **Settings → Secrets and variables → Actions**.

> **Security Note 🔒💡:**  
> For security best practices, ensure your `OPENAI_API_KEY` is created with strictly limited permissions—**it should only have "Write" access for Model capabilities** (used to chat/completions endpoints). Do not grant broader permissions that your workflow does not require.
---

### Excluded Files & Folders 📦

The workflow ignores files and directories not relevant to code review, including:

- `vendor/`, `node_modules/`, `bower_components/` (dependencies)
- `dist/`, `build/`, `out/` (build outputs)
- `.git/`, `.github/`, `.gitlab/`, `.circleci/` (VCS metadata)
- `coverage/`, `reports/` (coverage, reports)
- Binaries, archives: (`*.zip`, `*.tar`, `*.exe`, `*.dll`, etc.)
- Images, media, fonts
- Office files: (`*.pdf`, `*.docx`, etc.)
- Minified/generated files: (`*.min.js`, `*.pyc`, `*.map`)
- OS/config files: (`.env`, `.DS_Store`, `Thumbs.db`)
- Lockfiles and manifests (e.g. `composer.lock`)

You can adjust these in the exclusion logic within the workflow YAML if your project has additional needs.

---

### Workflow Process 🧰

1. **Checkout repo** using [actions/checkout](https://github.com/actions/checkout) with full git history for accurate diffs.
2. **Identify changed files** in the supported directories.
3. **Filter** files using comprehensive exclusion logic.
4. **Bundle diff and content** as JSON.
5. **Send to OpenAI** according to org prompt/model settings.
6. **Post** the review comment back on the pull request using [GitHub CLI](https://cli.github.com/).

---

### References ℹ️

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Actions Variables](https://docs.github.com/en/actions/learn-github-actions/variables)
- [GitHub Actions Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
- [OpenAI API Reference](https://platform.openai.com/docs/api-reference)
- [Workflow Triggers](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#pull_request)

---

## Adding More Workflows 🧰

Additional workflows should be added under `.github/workflows/` and documented in a new section below, including:

- **Workflow name and summary**
- **Triggers**
- **Required secrets/variables**
- **How to use/customize**
- **References, if applicable**

---

*This documentation and configuration is intended for use within Zivtech. Update this README whenever adding or changing workflows to ensure consistency and clarity.*
