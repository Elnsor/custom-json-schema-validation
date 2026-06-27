# GitHub Actions Complete Reference Guide

Quick reference for all popular GitHub Actions with their `with:` parameters.

---

## 📋 Table of Contents

- [Official GitHub Actions](#official-github-actions)
- [Setup Actions](#setup-actions)
- [Build & Push Actions](#build--push-actions)
- [Testing & Quality](#testing-&-quality)
- [Deployment Actions](#deployment-actions)
- [Utilities & Notifications](#utilities-&-notifications)

---

## Official GitHub Actions

### actions/checkout

**Description:** Check out your repository code

**Basic Usage:**
```yaml
- uses: actions/checkout@v4
  with:
    ref: main                    # Branch, tag, or SHA to checkout
    fetch-depth: 0               # 0 = all history, default = 1
    token: ${{ secrets.GITHUB_TOKEN }}
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `repository` | string | current repo | Repository name with owner (e.g., owner/repo) |
| `ref` | string | event ref | Branch, tag, or SHA to checkout |
| `token` | string | GITHUB_TOKEN | PAT for authentication |
| `ssh-key` | string | - | SSH key for authentication |
| `ssh-known-hosts` | string | - | Known hosts file content |
| `ssh-strict` | boolean | true | Enable strict host key checking |
| `persist-credentials` | boolean | true | Configure git credentials |
| `clean` | boolean | true | Run git clean -ffdx before fetch |
| `fetch-depth` | number | 1 | Number of commits to fetch (0 = all) |
| `lfs` | boolean | false | Download Git-LFS files |
| `submodules` | string | false | Checkout submodules: true, false, or recursive |
| `set-safe-directory` | boolean | true | Add repo as safe directory in git config |

**Examples:**
```yaml
# Checkout specific branch
- uses: actions/checkout@v4
  with:
    ref: develop

# Checkout with full history (for versioning)
- uses: actions/checkout@v4
  with:
    fetch-depth: 0

# Checkout with submodules
- uses: actions/checkout@v4
  with:
    submodules: recursive
    fetch-depth: 0

# Checkout with SSH
- uses: actions/checkout@v4
  with:
    ssh-key: ${{ secrets.SSH_PRIVATE_KEY }}
```

---

## Setup Actions

### actions/setup-node

**Description:** Setup Node.js environment with optional caching

**Basic Usage:**
```yaml
- uses: actions/setup-node@v4
  with:
    node-version: 18
    cache: npm                   # Cache npm dependencies
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `node-version` | string | - | Version spec (e.g., 18, 18.x, 18.17.0, >=18.0.0) |
| `node-version-file` | string | - | File with version spec (.nvmrc, .node-version) |
| `registry-url` | string | - | NPM registry URL for authentication |
| `scope` | string | - | Scope for authenticating against scoped registries |
| `cache` | string | - | Cache manager: npm, yarn, or pnpm |
| `cache-dependency-path` | string | - | Path to dependency file(s) with wildcard support |
| `architecture` | string | x64 | CPU architecture: x64 or x86 |
| `check-latest` | boolean | false | Check for latest available version |
| `token` | string | GITHUB_TOKEN | Token for downloading Node distributions |

**Examples:**
```yaml
# Setup Node 18 with npm cache
- uses: actions/setup-node@v4
  with:
    node-version: 18
    cache: npm

# Setup with specific lock file
- uses: actions/setup-node@v4
  with:
    node-version: 18
    cache: npm
    cache-dependency-path: '**/package-lock.json'

# Setup multiple versions (matrix)
- uses: actions/setup-node@v4
  with:
    node-version: ${{ matrix.node-version }}
    cache: npm

# Setup with private registry
- uses: actions/setup-node@v4
  with:
    node-version: 18
    registry-url: 'https://registry.npmjs.org'
    scope: '@myorg'
```

### actions/setup-python

**Description:** Setup Python environment with optional caching

**Basic Usage:**
```yaml
- uses: actions/setup-python@v4
  with:
    python-version: '3.11'
    cache: pip                   # Cache pip dependencies
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `python-version` | string | - | Python version (e.g., 3.11, 3.x, pypy3.10) |
| `python-version-file` | string | - | File with version spec (.python-version, pyproject.toml) |
| `cache` | string | - | Cache manager: pip, pipenv, or poetry |
| `cache-dependency-path` | string | - | Path to dependency file(s) with wildcard support |
| `architecture` | string | x64 | CPU architecture: x64 or x86 |
| `token` | string | GITHUB_TOKEN | Token for downloading Python distributions |
| `update-environment` | boolean | true | Update PATH and PYTHONPATH |
| `allow-prereleases` | boolean | false | Allow pre-release and development versions |

**Examples:**
```yaml
# Setup Python 3.11 with pip cache
- uses: actions/setup-python@v4
  with:
    python-version: '3.11'
    cache: pip

# Setup with poetry
- uses: actions/setup-python@v4
  with:
    python-version: '3.11'
    cache: poetry

# Setup from version file
- uses: actions/setup-python@v4
  with:
    python-version-file: '.python-version'
    cache: pip

# Multiple versions (matrix)
- uses: actions/setup-python@v4
  with:
    python-version: ${{ matrix.python-version }}
    cache: pip
```

### actions/setup-java

**Description:** Setup Java environment

**Basic Usage:**
```yaml
- uses: actions/setup-java@v3
  with:
    java-version: '17'
    distribution: 'temurin'      # Adoptium Temurin
```

**All Parameters:**

| Parameter | Type | Options | Description |
| :--- | :--- | :--- | :--- |
| `java-version` | string | - | Java version (e.g., 11, 17, 21) |
| `java-package` | string | jdk, jre | Package type |
| `distribution` | string | temurin, zulu, adopt, corretto | JDK distribution |
| `architecture` | string | x64, x86, aarch64 | CPU architecture |
| `cache` | string | gradle, maven | Dependency cache manager |
| `check-latest` | boolean | - | Check for latest available version |

### actions/setup-go

**Description:** Setup Go environment

**Basic Usage:**
```yaml
- uses: actions/setup-go@v4
  with:
    go-version: '1.20'
    cache: true                  # Cache Go modules
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `go-version` | string | - | Go version (e.g., 1.20, 1.20.x) |
| `go-version-file` | string | - | File with version spec (go.mod) |
| `cache` | boolean | true | Cache Go modules and build cache |
| `cache-dependency-path` | string | - | Path to dependency file with wildcard support |
| `token` | string | GITHUB_TOKEN | Token for downloading Go |
| `check-latest` | boolean | false | Check for latest available version |

---

## Build & Push Actions

### docker/build-push-action

**Description:** Build and push Docker images

**Basic Usage:**
```yaml
- uses: docker/build-push-action@v5
  with:
    context: .
    push: true
    tags: myrepo/myimage:latest
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `context` | string | . | Build context (directory) |
| `file` | string | Dockerfile | Path to Dockerfile |
| `push` | boolean | false | Push image to registry after build |
| `tags` | string | - | Image tags (comma-separated or newline-separated) |
| `build-args` | string | - | Build arguments (key=value pairs) |
| `target` | string | - | Target stage in multi-stage build |
| `no-cache` | boolean | false | Do not use cache when building |
| `platforms` | string | - | Target platforms (e.g., linux/amd64,linux/arm64) |
| `load` | boolean | false | Load image to Docker instead of pushing |
| `outputs` | string | - | Output destinations (Docker output format) |
| `cache-from` | string | - | External cache sources |
| `cache-to` | string | - | Cache destinations |
| `secrets` | string | - | Secrets passed to build (--secret key=value) |
| `labels` | string | - | Image labels |

**Examples:**
```yaml
# Build and push to Docker Hub
- uses: docker/build-push-action@v5
  with:
    context: .
    push: true
    tags: ${{ secrets.DOCKER_USERNAME }}/myapp:latest

# Build for multiple platforms
- uses: docker/build-push-action@v5
  with:
    context: .
    push: true
    platforms: linux/amd64,linux/arm64
    tags: myrepo/myimage:latest

# Build with build arguments
- uses: docker/build-push-action@v5
  with:
    context: .
    push: true
    tags: myrepo/myimage:latest
    build-args: |
      BUILD_DATE=$(date -u +'%Y-%m-%dT%H:%M:%SZ')
      VCS_REF=${{ github.sha }}

# Load locally for testing
- uses: docker/build-push-action@v5
  with:
    context: .
    load: true
    tags: myimage:test
```

### docker/login-action

**Description:** Log in to Docker registries

**Basic Usage:**
```yaml
- uses: docker/login-action@v3
  with:
    username: \${{ secrets.DOCKER_USERNAME }}
    password: \${{ secrets.DOCKER_PASSWORD }}
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `registry` | string | docker.io | Docker registry URL |
| `username` | string | - | Registry username |
| `password` | string | - | Registry password/token |
| `logout` | boolean | true | Log out from registry at end of job |

**Examples:**
```yaml
# Docker Hub
- uses: docker/login-action@v3
  with:
    username: \${{ secrets.DOCKERHUB_USERNAME }}
    password: \${{ secrets.DOCKERHUB_TOKEN }}

# GitHub Container Registry
- uses: docker/login-action@v3
  with:
    registry: ghcr.io
    username: \${{ github.actor }}
    password: \${{ secrets.GITHUB_TOKEN }}

# Private registry
- uses: docker/login-action@v3
  with:
    registry: registry.example.com
    username: \${{ secrets.REGISTRY_USERNAME }}
    password: \${{ secrets.REGISTRY_PASSWORD }}
```

---

## Testing & Quality

### actions/cache

**Description:** Cache dependencies and build outputs

**Basic Usage:**
```yaml
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key:  runner.os -npm-{{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      \${{ runner.os }}-npm-
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `path` | string | - | Paths to cache (can use wildcards) |
| `key` | string | - | Primary cache key |
| `restore-keys` | string | - | Ordered list of alternative keys (one per line) |
| `upload-chunk-size` | number | - | Chunk size for upload in bytes |
| `fail-on-cache-miss` | boolean | false | Fail if cache not found |
| `lookup-only` | boolean | false | Only check if cache exists, don't download |
| `enableCrossOsSymlink` | boolean | false | Enable cross-OS symbolic links |

**Examples:**
```yaml
# Cache npm dependencies
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key:  runner.os -npm-{{ hashFiles('**/package-lock.json') }}
    restore-keys: \${{ runner.os }}-npm-

# Cache pip dependencies
- uses: actions/cache@v4
  with:
    path: ~/.cache/pip
    key:  runner.os -pip-{{ hashFiles('**/requirements.txt') }}
    restore-keys: \${{ runner.os }}-pip-

# Cache Go modules
- uses: actions/cache@v4
  with:
    path: ~/go/pkg/mod
    key:  runner.os -go-{{ hashFiles('**/go.sum') }}
    restore-keys: \${{ runner.os }}-go-

# Cache Maven
- uses: actions/cache@v4
  with:
    path: ~/.m2/repository
    key:  runner.os -maven-{{ hashFiles('**/pom.xml') }}
    restore-keys: \${{ runner.os }}-maven-

# Multiple paths
- uses: actions/cache@v4
  with:
    path: |
      ~/.npm
      ~/.cache
    key:  runner.os -multi-{{ github.sha }}
```

### codecov/codecov-action

**Description:** Upload code coverage reports to Codecov

**Basic Usage:**
```yaml
- uses: codecov/codecov-action@v3
  with:
    file: ./coverage/coverage.xml
    flags: unittests
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `file` | string | - | Path to coverage file |
| `files` | string | - | Multiple coverage files (comma/newline separated) |
| `flags` | string | - | Flag to group coverage metrics |
| `name` | string | - | Custom name for the upload |
| `fail_ci_if_error` | boolean | false | Fail CI if Codecov upload errors |
| `verbose` | boolean | false | Enable verbose logging |
| `token` | string | - | Codecov repository token |
| `slug` | string | - | Repository slug |
| `commit_parent` | string | - | Parent commit SHA |
| `pr` | string | - | Pull request number |
| `branch` | string | - | Branch name |

**Examples:**
```yaml
# Upload coverage from pytest
- uses: codecov/codecov-action@v3
  with:
    file: ./coverage.xml
    flags: unittests
    name: codecov-umbrella

# Upload multiple coverage files
- uses: codecov/codecov-action@v3
  with:
    files: ./coverage1.xml,./coverage2.xml
    flags: unittests,integration

# With fail on error
- uses: codecov/codecov-action@v3
  with:
    file: ./coverage.xml
    fail_ci_if_error: true
    token: \${{ secrets.CODECOV_TOKEN }}
```

---

## Artifact Management

### actions/upload-artifact

**Description:** Upload build artifacts

**Basic Usage:**
```yaml
- uses: actions/upload-artifact@v4
  with:
    name: build-output
    path: ./dist
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `name` | string | artifact | Artifact name |
| `path` | string | - | File(s) or directory to upload |
| `if-no-files-found` | string | warn | Behavior if no files: warn, error, ignore |
| `retention-days` | number | repo default | Days to retain artifact (0 = default, -1 = indefinite) |
| `compression-level` | number | 6 | Compression level (0-9) |
| `overwrite` | boolean | false | Overwrite if artifact with same name exists |

**Examples:**
```yaml
# Upload build directory
- uses: actions/upload-artifact@v4
  with:
    name: build
    path: ./dist

# Upload with retention
- uses: actions/upload-artifact@v4
  with:
    name: build
    path: ./dist
    retention-days: 30

# Upload multiple patterns
- uses: actions/upload-artifact@v4
  with:
    name: test-results
    path: |
      ./test-results/**/*.xml
      ./coverage/**/*.html
    if-no-files-found: error
```

### actions/download-artifact

**Description:** Download build artifacts from previous jobs

**Basic Usage:**
```yaml
- uses: actions/download-artifact@v4
  with:
    name: build-output
    path: ./downloaded-artifacts
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `name` | string | - | Artifact name to download |
| `path` | string | ./ | Destination directory |
| `github-token` | string | GITHUB_TOKEN | Token for authentication |
| `run-id` | string | - | Run ID to download from (default: current run) |

**Examples:**
```yaml
# Download artifact
- uses: actions/download-artifact@v4
  with:
    name: build
    path: ./artifacts

# Download all artifacts
- uses: actions/download-artifact@v4
  with:
    path: ./all-artifacts
```
---

## Deployment Actions

### actions/deploy-pages

**Description:** Deploy to GitHub Pages

**Basic Usage:**
```yaml
- uses: actions/deploy-pages@v3
  with:
    artifact-id: github-pages
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `artifact-id` | string | - | Artifact ID from upload step |
| `token` | string | GITHUB_TOKEN | GitHub token for deployment |
| `environment-url` | string | - | Deployment environment URL |

### aws-actions/configure-aws-credentials

**Description:** Configure AWS credentials

**Basic Usage:**
```yaml
- uses: aws-actions/configure-aws-credentials@v2
  with:
    role-to-assume: arn:aws:iam::ACCOUNT_ID:role/ROLE_NAME
    aws-region: us-east-1
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `role-to-assume` | string | - | ARN of role to assume (OIDC) |
| `aws-access-key-id` | string | - | AWS access key ID |
| `aws-secret-access-key` | string | - | AWS secret access key |
| `aws-region` | string | - | AWS region |
| `role-duration-seconds` | number | 3600 | Duration for assumed role |
| `role-session-name` | string | GitHubActions | Session name for assumed role |

### azure/login

**Description:** Log in to Azure

**Basic Usage:**
```yaml
- uses: azure/login@v1
  with:
    creds: \${{ secrets.AZURE_CREDENTIALS }}
```

---

## Utilities & Notifications

### actions/github-script

**Description:** Run GitHub API scripts

**Basic Usage:**
```yaml
- uses: actions/github-script@v7
  with:
    script: |
      console.log('Running script');
      return await github.rest.repos.get({
        owner: context.repo.owner,
        repo: context.repo.repo,
      });
```

**All Parameters:**

| Parameter | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `script` | string | - | JavaScript code to execute |
| `github-token` | string | GITHUB_TOKEN | GitHub token |
| `debug` | boolean | false | Enable debug logging |
| `result-encoding` | string | string | How to encode result: string or json |
| `retries` | number | 0 | Number of retries on failure |

### slackapi/slack-github-action

**Description:** Send messages to Slack

**Basic Usage:**
```yaml
- uses: slackapi/slack-github-action@v1
  with:
    webhook-url: \${{ secrets.SLACK_WEBHOOK }}
    payload: |
      {
        "text": "GitHub Action build result: \${{ job.status }}"
      }
```

### 8398a7/action-slack

**Description:** Send Slack notifications

**Basic Usage:**
```yaml
- uses: 8398a7/action-slack@v3
  if: always()
  with:
    status: \${{ job.status }}
    webhook_url: \${{ secrets.SLACK_WEBHOOK }}
```

---

## Common Patterns

### Pattern 1: Node.js CI/CD
```yaml
name: Node CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-node@v4
        with:
          node-version: 18
          cache: npm
      
      - run: npm ci
      - run: npm run test
      - run: npm run build
      
      - uses: actions/upload-artifact@v4
        with:
          name: dist
          path: ./dist
```

### Pattern 2: Python Testing
```yaml
name: Python Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ['3.9', '3.10', '3.11']
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-python@v4
        with:
          python-version: \${{ matrix.python-version }}
          cache: pip
      
      - run: pip install -r requirements.txt
      - run: pytest
      
      - uses: codecov/codecov-action@v3
        with:
          file: ./coverage.xml
```

### Pattern 3: Docker Build & Push
```yaml
name: Build & Push Docker
on:
  push:
    branches: [main]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: docker/login-action@v3
        with:
          username: \${{ secrets.DOCKERHUB_USERNAME }}
          password: \${{ secrets.DOCKERHUB_TOKEN }}
      
      - uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: \${{ secrets.DOCKERHUB_USERNAME }}/myapp:latest
          cache-from: type=gha
          cache-to: type=gha,mode=max
```

### Pattern 4: Deploy to GitHub Pages
```yaml
name: Deploy to Pages
on:
  push:
    branches: [main]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci && npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: github-pages
          path: ./build
  
  deploy:
    needs: build
    runs-on: ubuntu-latest
    permissions:
      pages: write
      id-token: write
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: github-pages
      - uses: actions/deploy-pages@v3
```

---

## Quick Tips

- **Always pin action versions**: Use `@v4` not `@main` for stability.
- **Use caching**: Speed up workflows by caching dependencies.
- **Use matrix strategy**: Test multiple versions simultaneously.
- **Secrets management**: Store sensitive data as repository secrets.
- **Check action docs**: Each action on GitHub Marketplace has detailed docs.
- **Reuse workflows**: Use `workflow_call` to create reusable workflows.
- **Use conditional steps**: `if: always()`, `if: failure()`, etc.

