# GitHub Actions: Complete Beginner to Advanced Guide

**Version:** 1.0  
**Last Updated:** June 2, 2026  
**Repository:** https://github.com/annamthej/GitHubAction-Notes

---

## Table of Contents

1. [01 — Introduction to GitHub Actions](#01--introduction-to-github-actions)
   - What is GitHub Actions?
   - What is CI/CD?
   - Why Use GitHub Actions?
   - How GitHub Actions Works
   - Understanding YAML
   - Where Workflows Live
   - Hands-On: Creating Your First Workflow

2. [02 — GitHub Actions Fundamentals (Beginner Level)](#02--github-actions-fundamentals-beginner-level)
   - Understanding Workflow Structure
   - Triggers (Events)
   - Jobs
   - Runners
   - Steps
   - Marketplace Actions
   - Environment Variables
   - Secrets
   - Artifacts
   - Caching
   - Matrix Builds
   - Practical Exercise: Build a Real CI Workflow

3. [03 — Building CI Pipelines](#03--building-ci-pipelines)
   - What is a CI Pipeline?
   - Best Practices for CI
   - Real CI Pipeline Structure
   - Creating the Workflow File
   - Breaking Down Each Part
   - Job Dependencies
   - Common CI Pipeline Add-Ons
   - Troubleshooting Tips

4. [04 — Docker Integration (Intermediate Level)](#04--docker-integration-intermediate-level)
   - Why Use Docker in CI/CD?
   - Preparing Your Repository
   - GitHub Actions Setup for Docker Builds
   - Building a Docker Image (CI)
   - Adding Docker Layer Caching
   - Pushing Docker Images to Registries
   - GitHub Container Registry (GHCR)
   - Azure Container Registry (ACR)
   - Multi-Architecture Build
   - Tagging Strategies
   - Docker Image Scanning

5. [05 — CD Pipelines (Advanced)](#05--cd-pipelines-advanced)
   - What is Continuous Deployment?
   - Triggering Deployment After CI
   - Deployment Environments
   - Creating a Deployment Workflow
   - Releasing New Versions
   - Deployment Strategies
   - Using Secrets & Environment Config
   - Deployment Job Structure
   - Real Example: Container Deployment
   - Deployment Rollback Strategies

---

# 01 — Introduction to GitHub Actions

## 1.1 What is GitHub Actions?

GitHub Actions is a **CI/CD automation platform built directly into GitHub**.
It allows you to automate tasks such as:

* Running tests automatically
* Building source code
* Packaging applications into Docker images
* Deploying to servers, cloud platforms, Kubernetes clusters
* Running scheduled jobs (cron)
* Managing releases
* Security scanning and code quality checks

Actions are defined using **YAML workflow files** under:

```
.github/workflows/
```

Example:

```
.github/workflows/ci.yml
```

Everything happens **inside your GitHub repository**, with no external tools required.

---

## 1.2 What is CI/CD?

### CI — Continuous Integration

The process of **integrating code frequently** and verifying it automatically by:

* Running tests
* Validating code quality
* Checking security vulnerabilities
* Building artifacts

A CI pipeline ensures code works before merging to the main branch.

---

### CD — Continuous Delivery/Deployment

The process of **automatically releasing tested code** to environments such as:

* Development
* Staging
* Production

CD can deploy to:

* Virtual machines
* Docker containers
* Kubernetes clusters
* Cloud platforms (Azure, AWS, GCP)

GitHub Actions supports **both Delivery and Deployment** depending on how automated you want to be.

---

## 1.3 Why Use GitHub Actions? (Benefits)

### 1. Integrated with GitHub

No need for external CI servers like Jenkins or Azure DevOps — everything stays in your repo.

### 2. Fully automatable

Any repetitive task can be automated:
tests → build → docker image → deploy → notify → monitor

### 3. Cloud hosted runners

GitHub provides runners like:

* `ubuntu-latest`
* `windows-latest`
* `macos-latest`

No setup required.

### 4. Extensible through Marketplace

Over **25,000+ ready-made actions**, including:

* Setup Node/Python/Java/.NET
* Build Docker images
* Login to Azure/AWS/GCP
* Deploy to Kubernetes
* Code scanning

### 5. Supports advanced CI/CD

* Matrix builds
* Job parallelization
* Caching
* Reusable workflows
* Deployment environments
* Approvals
* OIDC authentication

### 6. Secure

* GitHub Secrets
* Environment protections
* Fine-grained permissions
* Encrypted logs

All perfect for enterprise use.

---

## 1.4 How GitHub Actions Works (Concept Overview)

### Workflow

A complete automation pipeline defined in a `.yml` file.

Example:

```yaml
name: CI Pipeline
on: push
jobs:
  build:
    runs-on: ubuntu-latest
```

---

### Event

A trigger that starts a workflow, such as:

* `push`
* `pull_request`
* `release`
* `workflow_dispatch`
* `schedule`

---

### Job

A collection of steps executed on a runner.

* Jobs can run in **parallel**
* Or run **in sequence** using `needs:`

---

### Step

An individual automation command:

* Running a script (`npm test`)
* Calling an action (`actions/checkout`)
* Logging into Azure
* Building a Docker image

Steps run **inside the same machine**.

---

### Runner

The actual machine where jobs run.

Types:

1. **GitHub-hosted runners** (free)

   * Ubuntu, Windows, macOS

2. **Self-hosted runners**

   * You provide your own server/VM
   * Useful for:

     * Private networks
     * GPU workloads
     * Long-running jobs
     * Custom hardware

---

## 1.5 Understanding YAML (for Workflows)

GitHub uses simple YAML syntax.

### Basics:

```yaml
key: value
list:
  - item1
  - item2
jobs:
  build:
    runs-on: ubuntu-latest
```

### Indentation

* Must use **spaces**, not tabs
* 2 spaces is recommended

### Interpolations:

```yaml
${{ github.sha }}
${{ env.IMAGE_TAG }}
${{ secrets.AZURE_CREDENTIALS }}
```

---

## 1.6 Where Workflows Live in a Repo

All workflows are stored in:

```
.github/workflows/
```

Examples:

```
ci.yml
build.yml
deploy.yml
docker-publish.yml
```

You can have unlimited workflows.

---

## 1.8 — Hands-On: Creating Your First GitHub Actions Workflow

This section gives you a **practical, step-by-step exercise** to create your very first workflow.
You will learn how to:

* Create the `.github/workflows/` directory
* Add your first workflow file
* Trigger a workflow on `push`
* Run simple commands
* View workflow runs

This is the foundation for all advanced CI/CD work later.

---

### ✅ Step 1 — Create the Workflow Folder

Inside your repository, create:

```
.github/workflows/
```

The folder must be exactly that name (all lowercase).

---

### ✅ Step 2 — Create Your First Workflow File

Create:

```
.github/workflows/hello.yml
```

Paste this minimal workflow:

```yaml
name: Hello GitHub Actions

on:
  push:
    branches:
      - main
      - master

jobs:
  hello-job:
    runs-on: ubuntu-latest

    steps:
      - name: Print a greeting
        run: echo "Hello from GitHub Actions!"

      - name: Show current time
        run: date

      - name: Show repository files
        run: ls -la
```

---

### ✅ Step 3 — Commit and Push

Commit and push your file:

```
git add .github/workflows/hello.yml
git commit -m "Add first GitHub Actions workflow"
git push
```

As soon as you push, your workflow **automatically triggers**.

---

### ✅ Step 4 — Check the Workflow Run

1. Go to your GitHub repository
2. Click **Actions** (top menu)
3. You'll see a new workflow named:
   **Hello GitHub Actions**
4. Open it to see detailed logs for each step

You should see:

```
Hello from GitHub Actions!
Sun Nov 12 10:33:21 UTC 2025
(total file list...)
```

This confirms your runner executed your commands successfully.

---

### ✅ Step 5 — Modify a Workflow and Trigger Again

Try updating:

```yaml
- name: Print workflow context
  run: echo "Triggered by: ${{ github.event_name }}"
```

Push again.
You'll now see which event triggered the workflow (push, PR, etc).

---

### 🎉 What You Have Learned

By completing section **1.8**, you now know:

* How to create workflow YAML files
* Where to store them
* How events trigger workflows
* How jobs and steps work
* How to read output logs

---

# 02 — GitHub Actions Fundamentals (Beginner Level)

## 2.1 Understanding the Structure of a Workflow

A GitHub Actions workflow is a YAML file with 4 main parts:

1. **name** – What the workflow is called
2. **on** – What triggers it
3. **jobs** – What tasks it runs
4. **steps** – Commands/actions inside each job

Example structure:

```yaml
name: Example Workflow

on: push

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Step 1
        run: echo "Hello"
```

Everything we learn next expands on these core parts.

---

## 2.2 Triggers (Events)

Triggers define **when** your workflow runs.

### Common triggers:

| Trigger             | Description                   |
| ------------------- | ----------------------------- |
| `push`              | Runs when someone pushes code |
| `pull_request`      | Runs on PR creation/update    |
| `schedule`          | Cron-based scheduled jobs     |
| `workflow_dispatch` | Manual run button             |
| `release`           | When creating GitHub releases |
| `workflow_call`     | Called by another workflow    |

### Example: Run on push to main only

```yaml
on:
  push:
    branches:
      - main
```

### Example: Run on schedule (every day at 2 AM)

```yaml
on:
  schedule:
    - cron: "0 2 * * *"
```

### Example: Manual trigger

```yaml
on:
  workflow_dispatch:
```

---

## 2.3 Jobs

Jobs group related work and run **on runners**.

### Basic job example:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building..."
```

### Key points:

* Each workflow can have **multiple jobs**
* Jobs run in **parallel** by default
* Use `needs:` to control job order

Example:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Testing..."

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - run: echo "Build only after tests"
```

---

## 2.4 Runners

A **runner** is the machine that executes your jobs.

GitHub provides 3 main types of hosted runners:

| Runner           | OS      |
| ---------------- | ------- |
| `ubuntu-latest`  | Linux   |
| `windows-latest` | Windows |
| `macos-latest`   | Mac     |

---

### Self-Hosted Runners

You can register your own machine to run jobs:

* Your own server
* A cloud VM
* A Kubernetes pod
* A GPU machine

Useful when:

* You need custom dependencies
* Your builds take long
* You want full control

---

## 2.5 Steps

Steps run inside the job in order.

Two types:

### 1. run: → Direct shell command

```yaml
steps:
  - name: Print branch
    run: echo "Branch: $GITHUB_REF"
```

### 2. uses: → Pre-built action

```yaml
steps:
  - name: Check out code
    uses: actions/checkout@v4
```

---

## 2.6 Using Marketplace Actions

GitHub Actions Marketplace has thousands of prebuilt actions.

### Example: Setting up Node.js

```yaml
- uses: actions/setup-node@v4
  with:
    node-version: 18
```

### Example: Uploading artifacts

```yaml
- uses: actions/upload-artifact@v4
  with:
    name: build-output
    path: dist/
```

---

## 2.7 Environment Variables

Two forms:

### 1. `env:` block

```yaml
env:
  NODE_ENV: production
```

### 2. Using inside steps

```yaml
run: echo "Current env: $NODE_ENV"
```

### Built-in environment variables

GitHub provides many defaults:

* `${{ github.sha }}`
* `${{ github.ref }}`
* `${{ github.actor }}`
* `${{ github.event_name }}`

Example:

```yaml
run: echo "Commit SHA: ${{ github.sha }}"
```

---

## 2.8 Secrets

Secrets store sensitive values like:

* API keys
* Cloud access credentials
* Tokens
* Passwords

Add them at:

**GitHub Repo → Settings → Secrets and Variables → Actions**

Use in workflow:

```yaml
run: echo ${{ secrets.MY_API_KEY }}
```

Secrets are **masked in logs**.

---

## 2.9 Artifacts

Artifacts let you store files generated during workflow execution.

### Upload:

```yaml
- name: Upload build files
  uses: actions/upload-artifact@v4
  with:
    name: my-artifact
    path: dist/
```

### Download:

```yaml
- uses: actions/download-artifact@v4
  with:
    name: my-artifact
```

Useful for:

* Build outputs
* Test reports
* Docker image tar files
* Log bundles

---

## 2.10 Caching (Huge Performance Boost)

Caching dependencies saves build time.

Example: cache npm dependencies

```yaml
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-npm-${{ hashFiles('package-lock.json') }}
```

Also works for:

* pip
* Maven
* Gradle
* Docker layers
* Yarn

---

## 2.11 Matrix Builds (Multi-Version Testing)

Matrix lets you run the **same job on multiple environments**.

Example: test on Node versions 16, 18, 20:

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [16, 18, 20]

    steps:
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
      - run: npm test
```

You can also matrix **OS × version**:

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest, macos-latest]
    python: [3.9, 3.10, 3.11]
```

---

## 2.12 Summary of Chapter 02

You now understand:

* How workflows are structured
* Events and triggers
* Jobs and runners
* Steps and marketplace actions
* Environment variables and secrets
* Artifacts
* Caching for faster builds
* Matrix builds (multi-version testing)

---

## Practical Exercise: Build a Real CI Workflow

Perfect — here is a **full hands-on practical exercise** based on everything you learned in **Chapter 02**.
By the end of this exercise, you will build a complete beginner-level CI workflow that:

* Checks out code
* Sets up a runtime environment
* Installs dependencies
* Uses caching
* Runs tests
* Stores test reports as artifacts
* Demonstrates matrix builds
* Uses secrets
* Uses environment variables

This is the foundation for the advanced CI/CD pipelines later.

---

### 🧪 Practical Exercise: Build a Real CI Workflow

We will create a workflow called **"CI Pipeline"** that includes:

1. Trigger on push & pull request
2. Run tests on **Node 18 & 20** (matrix)
3. Cache Node dependencies
4. Run tests
5. Upload test results
6. Use environment variables
7. Access a secret

This workflow applies equally for Node/Python/Java — you can adapt it easily.

---

### ✅ Step 1 — Create the Workflow File

Create:

```
.github/workflows/ci.yml
```

Add this full workflow:

---

### 🚀 Full CI Workflow Example (Complete)

```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

env:
  APP_ENV: "testing"
  LOG_LEVEL: "debug"

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node: [18, 20]

    steps:
      # Step 1: Checkout the code
      - name: Checkout repository
        uses: actions/checkout@v4

      # Step 2: Use Node (matrix version)
      - name: Setup Node ${{ matrix.node }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}

      # Step 3: Cache dependencies
      - name: Cache npm dependencies
        uses: actions/cache@v4
        with:
          path: ~/.npm
          key: ${{ runner.os }}-npm-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-npm-

      # Step 4: Install dependencies
      - name: Install dependencies
        run: npm ci

      # Step 5: Print environment variables
      - name: Print env vars
        run: |
          echo "Environment: $APP_ENV"
          echo "Log level: $LOG_LEVEL"

      # Step 6: Use a secret (create SECRET_MESSAGE manually)
      - name: Print a secret (masked)
        run: echo "Secret message is ${{ secrets.SECRET_MESSAGE }}"

      # Step 7: Run tests
      - name: Run tests
        run: npm test --if-present

      # Step 8: Upload test results
      - name: Upload test reports
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: test-results-${{ matrix.node }}
          path: |
            ./tests/results
            ./coverage
```

---

### 🚀 What This Workflow Demonstrates

#### ✔️ 1. Triggers

Runs on push & PR into main.

#### ✔️ 2. Environment Variables

Defined globally:

```yaml
env:
  APP_ENV: "testing"
```

#### ✔️ 3. Secrets Usage

Uses a secret named `SECRET_MESSAGE`:

```yaml
echo ${{ secrets.SECRET_MESSAGE }}
```

#### ✔️ 4. Matrix Builds

Runs the job twice:

* Node 18
* Node 20

#### ✔️ 5. Caching

Caches npm dependencies:

```yaml
uses: actions/cache@v4
```

#### ✔️ 6. Artifact Storage

Uploads test results per Node version.

#### ✔️ 7. Installing Dependencies and Running Tests

Standard CI tasks.

---

### ✔️ Step 2 — Create a Secret

Go to:

**GitHub Repo → Settings → Secrets and Variables → Actions → New Repository Secret**

Name:

```
SECRET_MESSAGE
```

Value:

```
hello-from-secrets
```

---

### ✔️ Step 3 — Trigger the Workflow

Do any of these:

* Push a commit
* Re-run an existing failed job
* Open a PR to main

---

### ✔️ Step 4 — Open the Results

Go to the **Actions** tab.

You will see:

* 2 matrices: Node 18, Node 20
* Logs for each step
* Artifacts at the bottom
* Secrets masked in logs
* Cache hit/miss

This is your first *real* CI pipeline.

---

### 🎯 What You Have Learned in This Exercise

You now practiced:

* Creating workflow files
* Using steps and actions
* Caching dependencies
* Using matrix builds
* Working with environment variables
* Using GitHub Secrets securely
* Uploading and viewing artifacts
* Understanding logs and build output

This knowledge is essential before we build:

* Docker CI
* Docker image pushing
* Kubernetes deployment
* Azure integration

---

### Nodejs Code Example

Sure! Here is a **simple Node.js project** you can use for GitHub Actions, CI/CD, Docker, or any tutorial.

I'll give you:

✔ Folder structure
✔ package.json
✔ index.js
✔ a simple test
✔ .gitignore

---

#### 📁 Project Structure

```
simple-node-app/
│
├── package.json
├── index.js
├── .gitignore
└── test/
    └── app.test.js
```

---

#### 📦 package.json

```json
{
  "name": "simple-node-app",
  "version": "1.0.0",
  "description": "A simple Node.js app for GitHub Actions demo",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "jest"
  },
  "author": "Your Name",
  "license": "MIT",
  "devDependencies": {
    "jest": "^29.7.0"
  }
}
```

---

#### 🟩 index.js

```js
function sum(a, b) {
  return a + b;
}

console.log("App is running… Sum(2,3) =", sum(2, 3));

module.exports = sum;
```

---

#### 🧪 test/app.test.js

```js
const sum = require('../index');

test('adds 2 + 3 = 5', () => {
  expect(sum(2, 3)).toBe(5);
});
```

---

#### 📝 .gitignore

```
node_modules/
npm-debug.log
.env
```

---

#### 🚀 Want GitHub Actions workflow too?

Here's a simple workflow for CI:

```yaml
name: Node.js CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 18

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

---

# 03 — Building CI Pipelines

## 3.1 What is a CI Pipeline?

A **Continuous Integration (CI) pipeline** automatically verifies your code whenever you push or open a pull request.

A typical CI workflow includes:

1. **Code checkout**
2. **Dependency installation**
3. **Build the source code**
4. **Lint & format checks**
5. **Unit tests**
6. **Integration tests**
7. **Code quality tools**
8. **Artifact packaging** (optional)
9. **Caching for speed**

The CI pipeline ensures that **every commit is safe** before merging into main.

---

## 3.2 Best Practices for CI

Before building the pipeline, understand these core best practices:

### ✔️ Keep CI fast (goal: < 10 minutes)

* Use caching
* Use matrix builds only when needed
* Split heavy tests into separate jobs

### ✔️ Fail fast

Tests should be the very first step after installing deps.

### ✔️ Run CI on pull requests

Never allow untested code to merge.

### ✔️ Upload artifacts

* Test results
* Coverage
* Build outputs

### ✔️ Run multiple runtime versions

Test your app on multiple Node/Python/Java versions.

### ✔️ Use linting + formatting

Avoid code-quality issues early.

---

## 3.3 A Real CI Pipeline Structure

We will build a CI pipeline with these jobs:

| Job                   | Purpose                                      |
| --------------------- | -------------------------------------------- |
| **lint**              | Check formatting & code style                |
| **test**              | Run unit tests using matrix builds           |
| **build**             | Build the application and generate artifacts |
| **integration-tests** | Run slower integration tests                 |
| **report**            | Aggregate results / upload artifacts         |

The pipeline should look like this:

```
lint → test → build → integration-tests → report
     └─────────── dependencies ────────────┘
```

---

## 3.4 Creating the Workflow File

Create:

```
.github/workflows/ci.yml
```

---

## 3.5 Full CI Pipeline (Professional Example)

This workflow includes everything:

```yaml
name: CI Pipeline

on:
  pull_request:
  push:
    branches: [ main ]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint --if-present

  test:
    runs-on: ubuntu-latest
    needs: lint
    strategy:
      matrix:
        node: [18, 20]

    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}

      - name: Cache dependencies
        uses: actions/cache@v4
        with:
          path: ~/.npm
          key: ${{ runner.os }}-npm-${{ hashFiles('package-lock.json') }}

      - name: Install dependencies
        run: npm ci

      - name: Run unit tests
        run: npm test --if-present

      - name: Upload test coverage
        uses: actions/upload-artifact@v4
        with:
          name: coverage-${{ matrix.node }}
          path: coverage/

  build:
    runs-on: ubuntu-latest
    needs: test

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20

      - name: Install dependencies
        run: npm ci

      - name: Build project
        run: npm run build --if-present

      - name: Upload build artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/

  integration-tests:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - uses: actions/checkout@v4

      - name: Run integration tests
        run: npm run test:integration --if-present
        env:
          API_URL: ${{ secrets.API_URL }}
          API_KEY: ${{ secrets.API_KEY }}

      - name: Upload integration test logs
        uses: actions/upload-artifact@v4
        with:
          name: integration-logs
          path: logs/

  report:
    runs-on: ubuntu-latest
    needs: [build, test, integration-tests]

    steps:
      - name: Final summary
        run: echo "CI pipeline completed successfully."
```

---

## 3.6 Breaking Down Each Part

### (1) Lint Job

Keeps code formatting standards clean.

### (2) Test Job With Matrix

Tests code on Node 18 & 20 simultaneously.

### (3) Build Job

Builds the production-ready version.

### (4) Integration Tests

Runs more expensive tests after build artifacts are produced.

### (5) Report Job

Aggregates and finishes the pipeline.

---

## 3.7 Adding Job Dependencies

You control the job order with:

```yaml
needs: lint
```

Multiple dependencies:

```yaml
needs: [test, build]
```

This creates a **directed workflow**, ensuring jobs run in the right order.

---

## 3.8 Common CI Pipeline Add-Ons

You can extend CI with:

### ✔️ Code scanning

```yaml
uses: github/codeql-action/init@v3
```

### ✔️ Dependency scanning

```yaml
uses: snyk/actions/node@master
```

### ✔️ Automatic tagging

```yaml
run: git tag v1.${{ github.run_number }}
```

### ✔️ Slack/Teams notifications

```yaml
uses: slackapi/slack-github-action@v1.24
```

---

## 3.9 CI Pipeline Troubleshooting Tips

| Problem                 | Solution                              |
| ----------------------- | ------------------------------------- |
| Cache not working       | Ensure correct hash key               |
| Job failing at checkout | Missing permissions                   |
| Secrets not available   | Ensure repo → Settings → Secrets      |
| Test artifacts empty    | Check test output path                |
| Matrix job slow         | Use narrower versions/parallelization |

---

## 3.10 Summary of Chapter 03

In this chapter, you learned how to build:

* A real production-grade CI pipeline
* Multiple jobs with dependencies
* Matrix testing (Node versions)
* Caching for faster builds
* Linting, testing, building
* Artifacts and logs storage
* Integration testing
* Reporting jobs

You now have a **solid CI backbone**, which we'll extend into Docker, CD, Kubernetes, and Azure pipelines.

---

# 04 — Docker Integration (Intermediate Level)

## 4.1 Why Use Docker in CI/CD Pipelines?

Docker helps you:

* Package your application + dependencies
* Create consistent environments across dev/staging/prod
* Build immutable, versioned images
* Deploy the same container to Kubernetes, Azure, or any cloud

GitHub Actions makes this easy by automating:

* Build
* Test
* Tag
* Push
* Scan (optional)

---

## 4.2 Preparing Your Repository

### Dockerfile Example (Node.js)

```dockerfile
# Stage 1: Builder
FROM node:20-alpine AS builder
WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

# Stage 2: Production image
FROM node:20-alpine AS prod
WORKDIR /app

COPY --from=builder /app/dist ./dist
COPY package*.json ./

RUN npm ci --only=production

CMD ["node", "dist/index.js"]
```

This is an optimized multi-stage Dockerfile:

* Faster builds
* Smaller image size
* Best practice for CI/CD pipelines

---

## 4.3 GitHub Actions Setup for Docker Builds

### Use the official Docker GitHub Actions:

| Purpose                    | Action                       |
| -------------------------- | ---------------------------- |
| Set up QEMU for cross-arch | `docker/setup-qemu-action`   |
| Set up Buildx              | `docker/setup-buildx-action` |
| Login to registry          | `docker/login-action`        |
| Build & push image         | `docker/build-push-action`   |

---

## 4.4 Building a Docker Image (CI)

Create file:

```
.github/workflows/docker-ci.yml
```

Add:

```yaml
name: Docker Build (CI)

on:
  push:
    branches: [ main ]
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3

      - name: Build Docker image
        uses: docker/build-push-action@v5
        with:
          context: .
          file: ./Dockerfile
          platforms: linux/amd64
          push: false
          tags: myapp:ci
```

This builds your Docker image **without pushing it** — great for CI testing.

---

## 4.5 Adding Docker Layer Caching (Huge Speed Gain)

Docker builds can be slow. Use GitHub cache backend:

```yaml
with:
  cache-from: type=gha
  cache-to: type=gha,mode=max
```

Updated example:

```yaml
      - name: Build Docker image with cache
        uses: docker/build-push-action@v5
        with:
          context: .
          file: ./Dockerfile
          platforms: linux/amd64
          push: false
          tags: myapp:ci
          cache-from: type=gha
          cache-to: type=gha,mode=max
```

This can **cut build time by 50–70%**.

---

## 4.6 Pushing Docker Images to Registries

You can push Docker images to:

* GitHub Container Registry (GHCR)
* Docker Hub
* Azure Container Registry (ACR)
* AWS ECR
* GCP GAR

Here we cover all major registries.

---

## 4.7 Push Image to GitHub Container Registry (GHCR)

### Step 1 — Add secret

Create a secret:

```
GHCR_TOKEN
```

Value: GitHub PAT with `write:packages`

### Step 2 — Add workflow

```yaml
name: Build and Push to GHCR

on:
  push:
    branches: [ main ]
    tags:
      - "v*"

env:
  IMAGE_NAME: ghcr.io/${{ github.repository }}/myapp

jobs:
  push-image:
    runs-on: ubuntu-latest
    permissions:
      packages: write
      contents: read

    steps:
      - uses: actions/checkout@v4

      - uses: docker/setup-buildx-action@v3

      - name: Login to GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GHCR_TOKEN }}

      - name: Build & Push
        uses: docker/build-push-action@v5
        with:
          push: true
          platforms: linux/amd64
          tags: |
            ${{ env.IMAGE_NAME }}:latest
            ${{ env.IMAGE_NAME }}:${{ github.sha }}
            ${{ env.IMAGE_NAME }}:${{ github.ref_name }}
```

---

## 4.8 Push Image to Azure Container Registry (ACR)

You need:

* ACR name
* OIDC permission OR Service Principal

### Recommended: OIDC (no passwords!)

Create:

```
AZURE_CLIENT_ID
AZURE_TENANT_ID
AZURE_SUBSCRIPTION_ID
```

Workflow:

```yaml
name: Build and Push to ACR

on:
  push:
    branches: [ main ]

env:
  ACR_NAME: myregistry.azurecr.io
  IMAGE_NAME: myapp

jobs:
  push-acr:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read

    steps:
      - uses: actions/checkout@v4

      - name: Azure Login
        uses: azure/login@v2
        with:
          client-id: ${{ secrets.AZURE_CLIENT_ID }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
          subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}

      - name: Login to ACR
        run: az acr login --name ${{ env.ACR_NAME }}

      - uses: docker/setup-buildx-action@v3

      - name: Build & Push Image to ACR
        uses: docker/build-push-action@v5
        with:
          push: true
          tags: |
            ${{ env.ACR_NAME }}/${{ env.IMAGE_NAME }}:latest
            ${{ env.ACR_NAME }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
```

---

## 4.9 Multi-Architecture Build (amd64 + arm64)

Supports:

* Intel/AMD servers
* Apple Silicon (M1/M2)
* Raspberry Pi

Add:

```yaml
platforms: linux/amd64,linux/arm64
```

Example:

```yaml
      - name: Build multi-arch image
        uses: docker/build-push-action@v5
        with:
          platforms: linux/amd64,linux/arm64
          push: true
          tags: myapp:latest
```

---

## 4.10 Tagging Strategies (CI/CD Standard)

Use multiple tag formats:

| Tag format    | Purpose                  |
| ------------- | ------------------------ |
| `latest`      | Default production image |
| `v1.0.0`      | Release version          |
| `sha-commit`  | Immutable build          |
| `branch-name` | Dev/staging environment  |
| `pr-123`      | PR previews              |

Example:

```yaml
tags: |
  myapp:latest
  myapp:${{ github.sha }}
  myapp:${{ github.ref_name }}
```

---

## 4.11 Docker Image Scanning (Security)

Optional but highly recommended:

### Use Trivy

```yaml
- name: Scan image
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: myapp:latest
```

---

## 4.12 Summary of Chapter 04

You now understand how to:

* Write optimized Dockerfiles
* Build Docker images in GitHub Actions
* Use Docker Buildx and caching
* Push images to GHCR, DockerHub, and ACR
* Authenticate using OIDC
* Perform multi-architecture builds
* Tag Docker images properly
* Scan images for vulnerabilities

This knowledge is required for the next chapter:
**Deploying to Kubernetes (AKS)**.

---

# 05 — CD Pipelines (Advanced)

## 5.1 What is Continuous Deployment?

CD is the automated process of deploying your **tested, verified, stable** build to your environments:

* Development
* QA / Testing
* Staging
* Production

A CD pipeline typically includes:

1. Downloading build artifacts
2. Using secrets / environment variables
3. Running deployment scripts (kubectl, helm, SSH, Azure CLI, etc.)
4. Switching versions
5. Health checks
6. Rollback if deployment fails

---

## 5.2 Triggering Deployment Only After CI Passes

You should **never deploy directly** from the push event.
CD must depend on CI.

Example:

```
CI → Build Docker → Push image → CD
```

The CD pipeline "listens" for:

* Successful CI
* Tagged release
* Manual approval

---

## 5.3 Deployment Environments in GitHub

GitHub provides managed environments:

* `dev`
* `staging`
* `prod`

Each environment can have:

* Environment-specific secrets
* Protection rules
* Required reviewers
* Wait timers
* Custom secrets per environment

### Example environment secrets:

| Environment | Secrets      |
| ----------- | ------------ |
| dev         | DB_CONN_DEV  |
| staging     | DB_CONN_STG  |
| prod        | DB_CONN_PROD |

This allows **safe deployments** depending on environment.

---

## 5.4 Creating a Deployment Workflow

Create file:

```
.github/workflows/deploy.yml
```

### Step 1 — Set up the trigger

Deploy only when a new release tag is pushed:

```yaml
on:
  push:
    tags:
      - "v*.*.*"
```

This means:

* Push a Git tag `v1.0.0`
* A release deployment begins automatically

---

## 5.5 Basic Deployment Template

Here is the simplest CD pipeline that downloads the build artifact and deploys it.

```yaml
name: Deploy App

on:
  push:
    tags:
      - "v*"

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: prod
      url: https://myapp.com

    steps:
      - uses: actions/checkout@v4

      - uses: actions/download-artifact@v4
        with:
          name: build-output

      - name: Deploy
        run: |
          echo "Deploying version ${{ github.ref_name }}"
```

This shows the concept, but we will move to real deployment soon (Kubernetes in Chapter 6).

---

## 5.6 Releasing New Versions from GitHub Actions

CI should produce versions automatically:

### Step → Tag version → Deploy

```yaml
- name: Create release
  uses: softprops/action-gh-release@v2
  with:
    tag_name: v1.${{ github.run_number }}
```

This automatically triggers your CD workflow.

---

## 5.7 Deployment Strategies

### ✔️ Strategy 1: Branch-Based

* Merge to `dev` → deploy to development
* Merge to `staging` → deploy to staging
* Merge to `main` → deploy to production

```yaml
on:
  push:
    branches:
      - dev
      - staging
      - main
```

### ✔️ Strategy 2: Tag-Based

* Push `v1.0.0` → deploy to production
* Push `v1.0.0-rc1` → deploy to staging

### ✔️ Strategy 3: Manual Deployment (workflow_dispatch)

For controlled releases:

```yaml
on:
  workflow_dispatch:
    inputs:
      version:
        type: string
```

You run it manually from the GitHub UI.

---

## 5.8 Using Secrets & Environment-Specific Config

GitHub separates secrets by environment:

* Only `dev` jobs can use `dev` secrets
* Only `prod` jobs can use `prod` secrets

Example:

```yaml
env:
  DB_URL: ${{ secrets.DB_URL }}
  API_KEY: ${{ secrets.API_KEY }}
```

---

## 5.9 Deployment Job Structure (Recommended)

A real deployment job should:

1. **Checkout code**
2. **Download build artifacts**
3. **Authenticate with cloud provider**
4. **Deploy container** (Docker, Kubernetes, Azure)
5. **Run health checks**
6. **Rollback on failure**

Example rollback logic:

```yaml
- name: Health check
  run: curl -f https://myapp.com/health || exit 1
```

If this step fails → GitHub Actions marks job failed → triggers rollback workflow.

---

## 5.10 Real Example: Container Deployment

Below is a CD job that:

* Pulls Docker image from registry
* Runs container on a VM / self-hosted server

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: SSH to Server and Deploy
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SERVER_KEY }}
          script: |
            docker pull myapp:latest
            docker stop app || true
            docker rm app || true
            docker run -d --name app -p 80:80 myapp:latest
```

This is simple VM-based CD.

The next chapter takes this further to **full Kubernetes deployment**.

---

## 5.11 Deployment Rollback Strategies

### ✔️ Automatic rollback

Deployments should fail if health check fails.

### ✔️ Manual rollback

Trigger a rollback workflow:

```yaml
on:
  workflow_dispatch:
    inputs:
      version:
```

### ✔️ Kubernetes rollback (built-in)

```sh
kubectl rollout undo deployment/myapp
```

---

## 5.12 Summary of Chapter 05

You learned:

* CD pipeline fundamentals
* GitHub Environments (dev/stage/prod)
* Manual approvals
* Release-based deployments
* Branch-based deployments
* Using artifacts in deployment
* Using secrets securely
* Real deployment examples
* Rollback strategy basics

---

# Next Steps

This comprehensive guide covers:

✅ Chapter 1: GitHub Actions Introduction  
✅ Chapter 2: Fundamentals & Practical Exercises  
✅ Chapter 3: Building Professional CI Pipelines  
✅ Chapter 4: Docker Integration (Multi-Registry Support)  
✅ Chapter 5: CD Pipelines & Deployment Strategies  

**Future Chapters (To Be Added):**
- Chapter 6: Kubernetes Deployment (AKS)
- Chapter 7: Azure Integration
- Chapter 8: Advanced Workflows & Reusable Actions
- Chapter 9: Security Best Practices
- Chapter 10: Real-World Enterprise Patterns

---

# How to Convert This to PDF

## Option A: Using Pandoc (Recommended)

```bash
# Install pandoc first
pandoc COMPLETE_GUIDE.md -o GitHubActions-Complete-Guide.pdf \
  --pdf-engine=xelatex \
  --toc \
  --toc-depth=2
```

## Option B: Using VS Code

1. Install "Markdown PDF" extension
2. Right-click COMPLETE_GUIDE.md
3. Select "Markdown PDF: Export (pdf)"

## Option C: Online Tools

- Upload COMPLETE_GUIDE.md to:
  - **Pandoc Online**: pandoc.org/try
  - **Dillinger.io**: dillinger.io
  - **Markdown to PDF converters**: markdown-pdf.com

## Option D: GitHub Pages (View Online)

Push this file to your repo and GitHub automatically renders it as a webpage.

---

**End of Complete Guide**

*For updates, issues, or contributions, visit: https://github.com/annamthej/GitHubAction-Notes*
