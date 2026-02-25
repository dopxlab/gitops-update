# 🚀 GitOps YAML Tag Update Action

Update a YAML key (e.g., `image.tag`) in a remote repository using **GitHub App authentication**.

✔ No Personal Access Token (PAT)
✔ No username/password
✔ Short-lived secure tokens
✔ Works with GitHub.com & GitHub Enterprise Server
✔ Private repository support
✔ GitOps & ArgoCD ready

---

## ✨ Features

* 🔐 GitHub App authentication (secure & enterprise-grade)
* 🌍 Dynamic host detection (GitHub.com & GHES)
* 📝 YAML update using `yq`
* 🔄 Safe commit (skips if no changes)
* 🚀 Direct push to target branch
* 🛡 Scoped repository permissions

---

## 📦 Inputs

| Input             | Required | Description                          |
| ----------------- | -------- | ------------------------------------ |
| `app_id`          | ✅        | GitHub App ID                        |
| `installation_id` | ✅        | GitHub App Installation ID           |
| `private_key`     | ✅        | GitHub App Private Key (PEM content) |
| `repository`      | ✅        | Target repository (`org/repo`)       |
| `branch`          | ❌        | Target branch (default: `main`)      |
| `file_path`       | ✅        | Path to YAML file                    |
| `yaml_key`        | ✅        | YAML key (example: `image.tag`)      |
| `new_value`       | ✅        | New value for YAML key               |

---

## 🛠 Example Usage

```yaml
name: GitOps Update

on:
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - name: Update Tag
        uses: your-org/gitops-update-action@v1
        with:
          app_id: ${{ secrets.APP_ID }}
          installation_id: ${{ secrets.INSTALLATION_ID }}
          private_key: ${{ secrets.APP_PRIVATE_KEY }}
          repository: my-org/gitops-repo
          branch: main
          file_path: helm/values.yaml
          yaml_key: image.tag
          new_value: v1.2.3
```

---

# 🔐 How Authentication Works

This action uses a **GitHub App** instead of:

❌ Personal Access Tokens
❌ Stored credentials
❌ Long-lived tokens

Flow:

1. Generate JWT (signed with GitHub App private key)
2. Exchange JWT for installation token
3. Clone target repo
4. Update YAML
5. Commit & push

Installation tokens:

* ⏳ Valid for 1 hour
* 🔒 Scoped only to selected repositories

---

# 🏗 Setup Guide – Create GitHub App

### 1️⃣ Create GitHub App

Go to:

```
https://github.com/settings/apps
```

Click **New GitHub App**

---

### 2️⃣ Configure Permissions

Set:

| Permission | Access       |
| ---------- | ------------ |
| Contents   | Read & Write |
| Metadata   | Read         |

Save.

---

### 3️⃣ Install the App

* Install to your organization or user
* Select target repository

---

### 4️⃣ Generate Private Key

Inside the GitHub App:

* Generate Private Key
* Download `.pem` file
* Copy contents into GitHub Secret

---

### 5️⃣ Add Secrets to Workflow Repo

Go to:

Repository → Settings → Secrets → Actions

Add:

* `APP_ID`
* `INSTALLATION_ID`
* `APP_PRIVATE_KEY` (paste full .pem content)

---

# 🌍 GitHub Enterprise Support

This action automatically detects:

* `GITHUB_SERVER_URL`
* `GITHUB_API_URL`

So it works for:

* ✅ github.com
* ✅ GitHub Enterprise Server
* ✅ Internal private GitHub domains

No hardcoded `github.com`.

---

# 🧠 YAML Example

Before:

```yaml
image:
  repository: my-app
  tag: v1.0.0
```

After:

```yaml
image:
  repository: my-app
  tag: v1.2.3
```

---

# 🔄 GitOps Use Case

Typical flow:

CI Build →
Update image tag in GitOps repo →
Push →
ArgoCD auto-sync →
Deploy

Fully GitOps compliant.

---

# 🛡 Security Benefits

| GitHub App             | Personal Access Token |
| ---------------------- | --------------------- |
| Short-lived token      | Often long-lived      |
| Repo scoped            | User scoped           |
| Easy rotation          | Harder to rotate      |
| Enterprise recommended | Not ideal             |

---

# 🚀 Roadmap (Optional Extensions)

* Pull Request mode instead of direct push
* Multi-file update
* JSON support
* Multi-environment matrix support
* Helm intelligent patching

---

# 📜 License

MIT License

---

# ⭐ Support

If this action helps you, please consider giving it a ⭐ on GitHub.
Joby