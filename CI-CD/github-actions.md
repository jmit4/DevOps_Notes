# GitHub Actions

Notes on building CI/CD pipelines with GitHub Actions.

---

## Key Concepts

| Term | Description |
|---|---|
| **Workflow** | Automated process defined in a YAML file under `.github/workflows/` |
| **Event** | Trigger that starts a workflow (push, pull_request, schedule, etc.) |
| **Job** | A group of steps that run on the same runner |
| **Step** | A single task within a job (shell command or Action) |
| **Action** | A reusable unit of code (from Marketplace or custom) |
| **Runner** | Machine that executes jobs (GitHub-hosted or self-hosted) |
| **Secret** | Encrypted environment variable stored in repository settings |

---

## Workflow File Structure

```yaml
# .github/workflows/ci.yml
name: CI

on:                            # Triggers
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build:                       # Job ID
    runs-on: ubuntu-latest     # Runner

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```

---

## Common Triggers

```yaml
on:
  push:                          # Any push
  push:
    branches: [main]             # Push to specific branches
    tags: ['v*']                 # Push of tags matching pattern
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 2 * * *'          # Nightly at 02:00 UTC
  workflow_dispatch:             # Manual trigger (adds "Run workflow" button)
  workflow_call:                 # Can be called by other workflows
```

---

## Environment Variables & Secrets

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    env:
      NODE_ENV: production       # Workflow-level env var

    steps:
      - name: Use secret
        env:
          API_KEY: ${{ secrets.MY_API_KEY }}   # Repository secret
        run: echo "API key is set: ${#API_KEY} chars"
```

---

## Matrix Builds

Run the same job across multiple configurations:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [18, 20, 22]
        os: [ubuntu-latest, windows-latest]
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm test
```

---

## Caching Dependencies

```yaml
- name: Cache node_modules
  uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

---

## Build & Push Docker Image

```yaml
- name: Log in to Docker Hub
  uses: docker/login-action@v3
  with:
    username: ${{ secrets.DOCKER_USERNAME }}
    password: ${{ secrets.DOCKER_PASSWORD }}

- name: Build and push
  uses: docker/build-push-action@v5
  with:
    context: .
    push: true
    tags: myuser/myapp:${{ github.sha }}
```

---

## Job Dependencies

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: npm test

  deploy:
    needs: test                  # Only run after test passes
    runs-on: ubuntu-latest
    steps:
      - run: ./deploy.sh
```

---

## Reusable Workflows

```yaml
# Called workflow (.github/workflows/deploy.yml)
on:
  workflow_call:
    inputs:
      environment:
        required: true
        type: string
    secrets:
      DEPLOY_KEY:
        required: true

# Caller workflow
jobs:
  deploy:
    uses: ./.github/workflows/deploy.yml
    with:
      environment: production
    secrets:
      DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
```

---

## Useful Contexts

```yaml
${{ github.sha }}           # Full commit SHA
${{ github.ref_name }}      # Branch or tag name
${{ github.actor }}         # Username that triggered the workflow
${{ github.repository }}    # owner/repo
${{ runner.os }}            # Linux | Windows | macOS
${{ env.MY_VAR }}           # Environment variable
```
