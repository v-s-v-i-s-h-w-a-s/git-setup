# Git, GitHub & GitHub Actions - 45-60 Minute Workshop

## Workshop Overview

**Duration:** 45-60 minutes  
**Format:** Fast-paced hands-on demonstration  
**Goal:** Understand Git → GitHub → CI/CD pipeline with real examples

---

## Pre-Workshop Setup (5 minutes)

**Students do this simultaneously while you explain concepts**

### Quick Tool Check

```bash
# Verify installations (students type along)
git --version
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

---

## Module 1: Git Basics → GitHub (15 minutes)

### Real-World Timeline Example

**Scenario: You're building a calculator app for a company**

```
Day 1: Start project → Initialize Git
Day 2: Add calculator → Commit changes
Day 3: Push to GitHub → Share with team
Day 4: New feature request → Create branch
Day 5: Complete feature → Merge via Pull Request
```

### Lightning Hands-On

```bash
# 1. Initialize project (30 seconds)
mkdir calculator-project && cd calculator-project
git init
echo "# Calculator App" > README.md
git add README.md
git commit -m "Initial commit"

# 2. Create the main application (1 minute)
cat > calculator.py << 'EOF'
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    if b != 0:
        return a / b
    else:
        raise ValueError("Cannot divide by zero")

if __name__ == "__main__":
    print("Calculator App v1.0")
    print(f"5 + 3 = {add(5, 3)}")
    print(f"10 - 4 = {subtract(10, 4)}")
EOF

git add calculator.py
git commit -m "Add basic calculator functions"
```

### Connect to GitHub (2 minutes)

```bash
# Create repository on GitHub (show students the UI)
# Then connect:
git remote add origin https://github.com/YOUR_USERNAME/calculator-project.git
git push -u origin main
```

### Feature Branch Workflow (3 minutes)

```bash
# Create feature branch
git checkout -b feature/advanced-operations

# Add new feature
cat >> calculator.py << 'EOF'

def power(a, b):
    return a ** b

def square_root(a):
    return a ** 0.5
EOF

git add calculator.py
git commit -m "Add power and square root functions"
git push -u origin feature/advanced-operations
```

**Show GitHub UI:** Create Pull Request → Merge → Delete branch

---

## Module 2: Understanding CI/CD Pipeline (10 minutes)

### CI/CD Timeline Visualization

```
Code Push → GitHub → Trigger Pipeline → Run Tests → Deploy
    ↓         ↓           ↓               ↓         ↓
  1 sec    2 sec      30 sec          2 min     5 min
```

### Real Company Example Timeline

**"What happens when you push code at 9:00 AM"**

```
09:00:00 - Developer pushes code to GitHub
09:00:01 - GitHub Actions triggered automatically
09:00:02 - Pipeline starts: "Building application..."
09:00:30 - Install dependencies complete
09:01:00 - Code quality check (SonarQube) starts
09:02:00 - SonarQube analysis complete
09:02:30 - Unit tests start running
09:03:00 - All tests pass
09:03:30 - Deploy to staging environment
09:05:00 - Deployment complete
09:05:30 - Slack notification: "Deployment successful!"
```

### Create Complete CI/CD Pipeline (5 minutes)

```bash
# Switch back to main and create workflow
git checkout main
git pull origin main
mkdir -p .github/workflows

# Create comprehensive pipeline with SonarQube
cat > .github/workflows/ci-cd-pipeline.yml << 'EOF'
name: Complete CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  # Stage 1: Build & Quality Check
  build-and-quality:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v3
      with:
        fetch-depth: 0  # Needed for SonarQube

    - name: Set up Python
      uses: actions/setup-python@v3
      with:
        python-version: 3.9

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install pytest pytest-cov flake8

    - name: Code Quality Check (Linting)
      run: |
        flake8 calculator.py --max-line-length=88 --extend-ignore=E203

    - name: SonarQube Analysis
      uses: SonarSource/sonarcloud-github-action@master
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}

  # Stage 2: Test
  test:
    needs: build-and-quality
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.8, 3.9, '3.10']

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v3
      with:
        python-version: ${{ matrix.python-version }}

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install pytest pytest-cov

    - name: Run tests with coverage
      run: |
        pytest tests/ --cov=calculator --cov-report=xml --cov-report=html

    - name: Upload coverage reports
      uses: codecov/codecov-action@v3
      with:
        file: ./coverage.xml

  # Stage 3: Security Scan
  security:
    needs: build-and-quality
    runs-on: ubuntu-latest
    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Run security scan
      uses: aquasecurity/trivy-action@master
      with:
        scan-type: 'fs'
        scan-ref: '.'

  # Stage 4: Deploy
  deploy:
    needs: [test, security]
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Deploy to Staging
      run: |
        echo " Building application..."
        echo " Packaging application..."
        echo " Deploying to staging environment..."
        echo " Deployment successful!"
        echo " App URL: https://calculator-staging.example.com"

    - name: 🔍 Health Check
      run: |
        echo " Running health checks..."
        echo " All systems operational"

    - name:  Notify Team
      run: |
        echo " Deployment notification sent to team"
EOF
```

---

## Module 3: Tests & SonarQube Setup (10 minutes)

### Create Test Suite (3 minutes)

```bash
# Create tests directory and files
mkdir tests
cat > tests/test_calculator.py << 'EOF'
import pytest
from calculator import add, subtract, multiply, divide, power, square_root

def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
    assert add(0, 0) == 0

def test_subtract():
    assert subtract(5, 3) == 2
    assert subtract(0, 5) == -5

def test_multiply():
    assert multiply(3, 4) == 12
    assert multiply(-2, 3) == -6

def test_divide():
    assert divide(10, 2) == 5.0
    assert divide(7, 2) == 3.5

    with pytest.raises(ValueError):
        divide(5, 0)

def test_power():
    assert power(2, 3) == 8
    assert power(5, 0) == 1

def test_square_root():
    assert square_root(9) == 3.0
    assert square_root(16) == 4.0
EOF

# Create empty __init__.py
touch tests/__init__.py
```

### SonarQube Configuration (2 minutes)

```bash
# Create SonarQube properties file
cat > sonar-project.properties << 'EOF'
# SonarQube Configuration
sonar.projectKey=calculator-project
sonar.organization=your-org
sonar.projectName=Calculator Project
sonar.projectVersion=1.0

# Source code
sonar.sources=.
sonar.exclusions=tests/**,**/__pycache__/**,.github/**

# Test coverage
sonar.python.coverage.reportPaths=coverage.xml
sonar.python.xunit.reportPath=test-results.xml

# Language
sonar.language=python
EOF
```

### Quick SonarQube Setup Demo (3 minutes)

**Show students the SonarCloud interface:**

1. **Go to SonarCloud.io** → Sign up with GitHub
2. **Import Repository** → Select your calculator project
3. **Get SONAR_TOKEN** → Copy token
4. **GitHub Settings** → Secrets → Add `SONAR_TOKEN`
5. **Show Quality Gates** → Explain code coverage, bugs, vulnerabilities

### Visual SonarQube Benefits Timeline

```
Without SonarQube:
Bug in Production → Customer Complains → 2 hours debugging → Fix → Deploy
     ↓                    ↓                 ↓              ↓       ↓
  Day 1              Day 2           Day 2-3        Day 3   Day 4

With SonarQube:
Code Push → SonarQube detects issue → Fix immediately → Deploy
    ↓              ↓                      ↓               ↓
  9:00 AM        9:02 AM              9:05 AM         9:10 AM
```

---

## Module 4: Multi-Language Quick Demo (8 minutes)

### Node.js Example (3 minutes)

```bash
# Create Node.js example
mkdir -p examples/nodejs
cd examples/nodejs

# Quick Express app
cat > app.js << 'EOF'
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.json({ message: 'Hello from Node.js Calculator!' });
});

app.get('/add/:a/:b', (req, res) => {
  const result = parseInt(req.params.a) + parseInt(req.params.b);
  res.json({ result });
});

module.exports = app;
EOF

# Package.json
cat > package.json << 'EOF'
{
  "name": "calculator-api",
  "version": "1.0.0",
  "scripts": {
    "start": "node app.js",
    "test": "jest"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "jest": "^29.3.1",
    "supertest": "^6.3.3"
  }
}
EOF

cd ../..
```

### Java Example (2 minutes)

```bash
# Create Java structure
mkdir -p examples/java/src/main/java
cat > examples/java/src/main/java/Calculator.java << 'EOF'
public class Calculator {
    public static int add(int a, int b) {
        return a + b;
    }

    public static void main(String[] args) {
        System.out.println("Java Calculator: 5 + 3 = " + add(5, 3));
    }
}
EOF

# Maven POM
cat > examples/java/pom.xml << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>calculator</artifactId>
    <version>1.0</version>
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>
</project>
EOF
```

### Language-Specific Pipeline Differences (2 minutes)

**Show quick workflow comparison:**

```yaml
# Python Pipeline
- name: Set up Python
  uses: actions/setup-python@v3
- run: pip install -r requirements.txt
- run: pytest

# Node.js Pipeline
- name: Set up Node.js
  uses: actions/setup-node@v3
- run: npm ci
- run: npm test

# Java Pipeline
- name: Set up JDK
  uses: actions/setup-java@v3
- run: mvn clean test
```

---

## Module 5: Push & Watch Pipeline (5 minutes)

### Final Commit & Push

```bash
# Add all files
git add .
git commit -m "Add complete CI/CD pipeline with tests and SonarQube"
git push origin main
```

### Live Pipeline Demo (5 minutes)

**Show students in real-time:**

1. **GitHub Actions Tab** → Watch pipeline start
2. **Build Stage** → Show logs, timing
3. **SonarQube Stage** → Show quality analysis
4. **Test Stage** → Show test results, coverage
5. **Deploy Stage** → Show deployment simulation

### Pipeline Success Timeline

```
 Pipeline Complete in 3 minutes:
00:00 - Code pushed to GitHub
00:05 - Build & Quality check complete
00:45 - SonarQube analysis complete
01:30 - Tests complete (100% coverage)
02:00 - Security scan complete
02:30 - Deploy to staging complete
03:00 - All notifications sent
```

---

## Module 6: Key Takeaways & Next Steps (5 minutes)

### What We Built

- **Complete Git workflow** with branching and merging
- **Professional CI/CD pipeline** with 4 stages
- **Quality gates** with SonarQube integration
- **Multi-language examples** for different projects

### Business Impact Timeline

```
Before CI/CD:
Manual testing → 2 hours
Manual deployment → 1 hour
Bug discovery → 2 days
Total: 2 days + 3 hours

After CI/CD:
Automatic testing → 2 minutes
Automatic deployment → 3 minutes
Bug discovery → 2 minutes
Total: 7 minutes
```

### Next Steps for Students

1. **Fork this repository** and modify it
2. **Add your own features** and watch the pipeline run
3. **Set up SonarCloud** for your personal projects
4. **Try different languages** using the examples provided

### Quick Resources

- **GitHub Actions Marketplace** - Pre-built actions
- **SonarCloud** - Free for open source projects
- **Codecov** - Code coverage reporting
- **GitHub Skills** - Interactive learning

### Challenge Assignment

**Create your own project with:**

- ✅ Git repository with branches
- ✅ CI/CD pipeline with tests
- ✅ SonarQube integration
- ✅ Deploy to GitHub Pages
- ✅ Share repository URL for review

---

## Quick Reference Commands

### Git Essentials

```bash
git init                    # Initialize repository
git add .                   # Stage all changes
git commit -m "message"     # Commit changes
git push origin main        # Push to GitHub
git checkout -b feature     # Create feature branch
git merge feature          # Merge branch
```

### Pipeline Debugging

```bash
# Check workflow syntax locally
npm install -g @github/workflows-cli
workflows validate .github/workflows/ci-cd-pipeline.yml

# View pipeline logs
# Go to GitHub → Actions → Click on failed run
```

### SonarQube Quick Setup

1. **SonarCloud.io** → Import GitHub repo
2. **Copy SONAR_TOKEN**
3. **GitHub Settings** → Secrets → Add token
4. **Push code** → Watch analysis

---

## Common Issues & Solutions

**Git Permission Denied:**

```bash
git remote set-url origin https://github.com/username/repo.git
```

**Pipeline Fails:**

- Check indentation in YAML files
- Verify secrets are set in GitHub
- Check file paths in workflow

**SonarQube Not Working:**

- Verify SONAR_TOKEN is set
- Check sonar-project.properties file
- Ensure repository is public or has SonarCloud access

**Tests Not Running:**

- Check test file naming (test\_\*.py)
- Verify pytest installation
- Check import statements in tests
