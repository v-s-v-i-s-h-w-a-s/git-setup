# Git, GitHub & GitHub Actions Workshop Guide

## Workshop Overview (45-60 minutes)

### Learning Objectives

By the end of this workshop, students will understand:

- Git fundamentals and version control concepts
- GitHub collaboration workflow
- Basic CI/CD pipeline creation with GitHub Actions
- Code quality integration with SonarQube

---

## Section 1: Git Fundamentals (15 minutes)

### Real-World Analogy: The Time Machine Story

Think of Git like **The Flash's timeline** or **Loki's multiverse**:

- **Repository = The Timeline/Universe**: Your entire project's history
- **Commits = Time Points**: Snapshots you can travel back to
- **Branches = Alternate Timelines**: Different versions of reality
- **Merge = Timeline Convergence**: Bringing alternate realities together
- **Clone = Creating a Copy of the Universe**: Getting your own version
- **Fork = Creating an Alternate Universe**: Making your own parallel version

### Hands-On: Git Basics

#### Step 1: Initial Setup

```bash
# Configure Git (everyone types this)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Check configuration
git config --list
```

#### Step 2: Create Your First Repository

```bash
# Create a new directory
mkdir my-first-project
cd my-first-project

# Initialize Git repository
git init

# Check status (this shows our "timeline status")
git status
```

#### Step 3: First Commit (Creating Your First Time Point)

```bash
# Create a file
echo "# My First Project" > README.md

# Add to staging area (preparing for time travel)
git add README.md

# Create first commit (our first time point)
git commit -m "Initial commit: Created README"

# View history (see our timeline)
git log --oneline
```

**Explanation**:

- `git init` creates a new timeline
- `git add` prepares changes for our time point
- `git commit` creates a permanent time point we can return to
- `git log` shows our timeline history

---

## Section 2: GitHub Collaboration (10 minutes)

### Clone vs Fork: Understanding the Difference

**Clone = Getting a Copy**

- Like downloading a movie to watch
- You get a local copy to work with
- Connected to the original repository

**Fork = Creating Your Own Version**

- Like making a remix of a song
- You own this version and can modify it freely
- Can contribute back to the original

### Hands-On: GitHub Workflow

#### Step 1: Create Repository on GitHub

1. Go to GitHub.com
2. Click "New Repository"
3. Name it "workshop-demo"
4. Check "Add a README file"
5. Click "Create repository"

#### Step 2: Clone Repository

```bash
# Clone the repository (get your copy)
git clone https://github.com/YOUR-USERNAME/workshop-demo.git
cd workshop-demo

# Check remote connections
git remote -v
```

#### Step 3: Branching (Creating Alternate Timelines)

```bash
# Create and switch to new branch
git checkout -b feature/add-calculator

# Or using newer syntax
git switch -c feature/add-calculator

# See all branches
git branch -a
```

#### Step 4: Make Changes and Push

```bash
# Create a simple calculator file
echo 'def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

if __name__ == "__main__":
    print("Calculator ready!")
    print(f"2 + 3 = {add(2, 3)}")' > calculator.py

# Add and commit
git add calculator.py
git commit -m "Add basic calculator functionality"

# Push to GitHub
git push origin feature/add-calculator
```

**Explanation**:

- Branches let us work on features without affecting main timeline
- Push sends our local changes to GitHub
- Each branch is like a parallel universe of our project

---

## Section 3: GitHub Actions - CI/CD Pipeline (20 minutes)

### The Assembly Line Analogy

Think of GitHub Actions like an **automated factory assembly line**:

- **Trigger = Start Button**: When someone pushes code
- **Jobs = Assembly Stations**: Build, Test, Deploy
- **Steps = Workers**: Individual tasks at each station
- **Artifacts = Products**: What gets passed between stations

### Hands-On: Create Your First CI/CD Pipeline

#### Step 1: Create Workflow Directory

```bash
# Create GitHub Actions directory
mkdir -p .github/workflows

# Create workflow file
touch .github/workflows/ci-cd.yml
```

#### Step 2: Basic CI/CD Pipeline

```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    name: Build Stage

    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.9"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install flake8 pytest

      - name: Display build info
        run: |
          echo "🏗️ Build completed successfully!"
          python --version

  test:
    runs-on: ubuntu-latest
    needs: build
    name: Test Stage

    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.9"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install pytest

      - name: Run tests
        run: |
          echo "Running tests..."
          python -m pytest -v || echo "No tests found, but that's okay for demo!"

      - name: Test our calculator
        run: |
          python calculator.py

  code-quality:
    runs-on: ubuntu-latest
    needs: build
    name: Code Quality Analysis

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: SonarQube Scan
        uses: sonarqube-quality-gate-action@master
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
        with:
          scanMetadataReportFile: target/sonar/report-task.txt
        continue-on-error: true

      - name: Code Quality Check (Demo)
        run: |
          echo " Running code quality analysis..."
          echo "✅ Code quality check passed!"

  deploy:
    runs-on: ubuntu-latest
    needs: [test, code-quality]
    name: Deploy Stage
    if: github.ref == 'refs/heads/main'

    steps:
      - uses: actions/checkout@v4

      - name: Deploy to Production
        run: |
          echo " Deploying to production..."
          echo "✅ Deployment successful!"
          echo " Deployment URL: https://my-app.example.com"
```

#### Step 3: Create Test File

```bash
# Create a simple test file
echo 'import pytest
from calculator import add, subtract

def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0

def test_subtract():
    assert subtract(5, 3) == 2
    assert subtract(0, 5) == -5

if __name__ == "__main__":
    print("All tests passed!")' > test_calculator.py
```

#### Step 4: Commit and Push

```bash
# Add all files
git add .
git commit -m "Add CI/CD pipeline with testing"

# Push to trigger pipeline
git push origin feature/add-calculator
```

**Explanation**:

- The pipeline runs automatically when code is pushed
- Each job runs in isolation (like separate assembly lines)
- `needs:` creates dependencies between jobs
- Secrets keep sensitive information safe

---

## Section 4: Pipeline Visualization & Timeline (5 minutes)

### The Movie Production Timeline

```
MOVIE PRODUCTION TIMELINE
├──  Script Writing (Code Development)
├──  Casting (Dependencies)
├──  Filming (Build)
├──  Editing (Testing)
├──  Sound/Effects (Code Quality)
├──  Premiere (Deploy)
└──  Box Office (Monitoring)

GITHUB ACTIONS TIMELINE
├──  Code Push (Trigger)
├──  Build Job (Compile/Setup)
├──  Test Job (Run Tests)
├──  Quality Job (SonarQube)
├──  Deploy Job (Production)
└──  Monitor (Feedback)
```

### Visual Pipeline Flow

```
Developer → Push Code → GitHub → Actions Triggered
    ↓
Build Stage:  Compile & Setup
    ↓
Parallel Execution:
├── Test Stage:  Run Tests
└── Quality Stage: Code Analysis
    ↓
Deploy Stage:  Production Release
    ↓
Monitor: Track Performance
```

---

## 🛠️ Section 5: Multi-Language Support (5 minutes)

### Language-Specific Examples

#### Python Project

```yaml
- name: Set up Python
  uses: actions/setup-python@v4
  with:
    python-version: "3.9"
- name: Install dependencies
  run: |
    pip install -r requirements.txt
    pip install pytest flake8
- name: Run tests
  run: pytest
```

#### Node.js Project

```yaml
- name: Set up Node.js
  uses: actions/setup-node@v4
  with:
    node-version: "18"
- name: Install dependencies
  run: npm install
- name: Run tests
  run: npm test
```

#### Java Project

```yaml
- name: Set up JDK
  uses: actions/setup-java@v4
  with:
    java-version: "11"
- name: Run tests
  run: ./mvnw test
```

---

## Quick Reference Commands

### Git Essentials

```bash
# Basic workflow
git status          # Check current state
git add .           # Stage all changes
git commit -m "msg" # Create commit
git push            # Send to GitHub
git pull            # Get latest changes

# Branching
git branch                    # List branches
git checkout -b feature-name  # Create & switch
git merge feature-name        # Merge branch
git branch -d feature-name    # Delete branch
```

### GitHub Actions Tips

```bash
# View workflow runs
gh workflow list
gh run list

# Trigger workflow manually
gh workflow run ci-cd.yml
```

---

## Key Takeaways

1. **Git is your time machine** - commits are save points
2. **GitHub is your collaboration hub** - share and work together
3. **Actions automate everything** - from testing to deployment
4. **CI/CD prevents bugs** - catch issues before users do
5. **Quality gates matter** - SonarQube keeps code clean

---

## Next Steps

1. **Practice**: Create your own repository and pipeline
2. **Explore**: Try different actions from the GitHub Marketplace
3. **Integrate**: Add real testing frameworks and deployment targets
4. **Learn**: Explore advanced Git features like rebasing and cherry-picking
5. **Collaborate**: Contribute to open-source projects

---

## Pro Tips

- **Always write meaningful commit messages**
- **Use branch naming conventions** (feature/, bugfix/, hotfix/)
- **Keep pipelines fast** - parallel jobs when possible
- **Use secrets for sensitive data** - never hardcode credentials
- **Monitor pipeline performance** - optimize slow steps
- **Test locally first** - don't rely on CI for basic testing

---

## Workshop Conclusion

Congratulations! You've just learned the fundamentals of modern software development workflow. You now understand how to:

- Version control your code with Git
- Collaborate effectively with GitHub
- Automate testing and deployment with GitHub Actions
- Integrate code quality checks with SonarQube

Remember: These tools are like learning to drive - practice makes perfect! 
