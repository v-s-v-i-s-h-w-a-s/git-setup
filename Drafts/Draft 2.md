# Git, GitHub & GitHub Actions - Hands-On Workshop Guide

## Workshop Overview
**Duration:** 4-6 hours  
**Format:** Hands-on coding workshop with live demonstrations  
**Prerequisites:** Basic command line knowledge, code editor installed  

## Pre-Workshop Setup (15 minutes)

### Required Tools
1. **Git** - Download from [git-scm.com](https://git-scm.com/)
2. **GitHub Account** - Sign up at [github.com](https://github.com/)
3. **Code Editor** - VS Code, IntelliJ, or any preferred editor
4. **Terminal/Command Prompt** access

### Verification Commands (Students execute together)
```bash
# Check Git installation
git --version

# Configure Git (replace with your details)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Verify configuration
git config --list
```

---

## Module 1: Git Fundamentals (45 minutes)

### Theory: What is Git?
- **Version Control System** - Track changes in files over time
- **Distributed** - Every developer has complete history
- **Snapshots** - Git stores snapshots, not differences
- **Three States**: Working Directory → Staging Area → Repository

### Hands-On Exercise 1: Local Git Repository

#### Step 1: Create Your First Repository
```bash
# Create project directory
mkdir my-first-project
cd my-first-project

# Initialize Git repository
git init

# Check status
git status
```

**Expected Output:** "On branch main, No commits yet, nothing to commit"

#### Step 2: Create and Track Files
```bash
# Create a README file
echo "# My First Project" > README.md

# Check status
git status

# Add file to staging area
git add README.md

# Check status again
git status

# Commit the file
git commit -m "Add README file"

# View commit history
git log
```

**Visual Learning:** Show the three states diagram as students execute commands

#### Step 3: Understanding Git Workflow
```bash
# Create a simple Python file
echo 'print("Hello, Git!")' > hello.py

# Check status
git status

# Add and commit
git add hello.py
git commit -m "Add hello.py script"

# View history
git log --oneline
```

### Theory Break: Git Object Model
- **Commits** - Snapshots of your project
- **Trees** - Directory structure
- **Blobs** - File contents
- **References** - Pointers to commits

---

## Module 2: GitHub Integration (45 minutes)

### Theory: What is GitHub?
- **Remote Repository Hosting** - Cloud storage for Git repositories
- **Collaboration Platform** - Multiple developers working together
- **Additional Features** - Issues, Pull Requests, Actions, Pages

### Hands-On Exercise 2: GitHub Repository

#### Step 1: Create GitHub Repository
1. Go to GitHub.com
2. Click "New Repository"
3. Name it "my-first-project"
4. **Don't initialize** with README (we already have one)
5. Click "Create Repository"

#### Step 2: Connect Local to Remote
```bash
# Add remote origin
git remote add origin https://github.com/YOUR_USERNAME/my-first-project.git

# Verify remote
git remote -v

# Push to GitHub
git push -u origin main

# Check your GitHub repository in browser
```

**Visual Learning:** Show the GitHub interface and explain the connection

#### Step 3: Clone vs Fork Understanding
```bash
# Clone demonstration (in a new directory)
cd ..
git clone https://github.com/YOUR_USERNAME/my-first-project.git cloned-project
cd cloned-project
ls -la
```

**Theory: Clone vs Fork**
- **Clone** - Copy repository to local machine
- **Fork** - Copy repository to your GitHub account
- **Use Cases** - Fork for contributing to others' projects

---

## Module 3: Branching and Collaboration (60 minutes)

### Theory: Git Branching
- **Branches** - Parallel development lines
- **Main/Master** - Primary branch
- **Feature Branches** - Isolated development
- **Merge Strategies** - Combining branches

### Hands-On Exercise 3: Feature Branch Workflow

#### Step 1: Create and Switch Branches
```bash
# Go back to original repository
cd ../my-first-project

# Create and switch to feature branch
git checkout -b feature/calculator

# Check current branch
git branch

# Alternative: Create branch and switch (Git 2.23+)
git switch -c feature/calculator
```

#### Step 2: Develop Feature
```bash
# Create calculator.py
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
        return "Error: Division by zero"

# Simple calculator
if __name__ == "__main__":
    print("Simple Calculator")
    print(f"5 + 3 = {add(5, 3)}")
    print(f"10 - 4 = {subtract(10, 4)}")
    print(f"6 * 7 = {multiply(6, 7)}")
    print(f"15 / 3 = {divide(15, 3)}")
EOF

# Add and commit
git add calculator.py
git commit -m "Add basic calculator functionality"

# Push feature branch
git push -u origin feature/calculator
```

#### Step 3: Merge via Pull Request
1. Go to GitHub repository
2. Click "Compare & pull request"
3. Add description: "Add calculator functionality"
4. Click "Create pull request"
5. Review changes
6. Click "Merge pull request"
7. Delete branch on GitHub

#### Step 4: Sync Local Repository
```bash
# Switch back to main
git checkout main

# Pull latest changes
git pull origin main

# Delete local feature branch
git branch -d feature/calculator

# View updated history
git log --oneline --graph
```

---

## Module 4: GitHub Actions CI/CD Pipeline (90 minutes)

### Theory: CI/CD Concepts
- **Continuous Integration (CI)** - Automated testing of code changes
- **Continuous Deployment (CD)** - Automated deployment to production
- **GitHub Actions** - GitHub's built-in CI/CD platform
- **Workflows** - Automated processes triggered by events

### Hands-On Exercise 4: Basic CI Pipeline

#### Step 1: Create Workflow Directory
```bash
# Create GitHub Actions directory
mkdir -p .github/workflows

# Create basic CI workflow
cat > .github/workflows/ci.yml << 'EOF'
name: CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v3
      with:
        python-version: 3.9
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install pytest
    
    - name: Run tests
      run: |
        python -m pytest tests/ -v
EOF

# Commit workflow
git add .github/workflows/ci.yml
git commit -m "Add basic CI pipeline"
```

#### Step 2: Create Test Files
```bash
# Create tests directory
mkdir tests

# Create test file
cat > tests/test_calculator.py << 'EOF'
import sys
import os
sys.path.insert(0, os.path.dirname(os.path.dirname(os.path.abspath(__file__))))

from calculator import add, subtract, multiply, divide

def test_add():
    assert add(2, 3) == 5
    assert add(-1, 1) == 0
    assert add(0, 0) == 0

def test_subtract():
    assert subtract(5, 3) == 2
    assert subtract(0, 5) == -5
    assert subtract(10, 10) == 0

def test_multiply():
    assert multiply(3, 4) == 12
    assert multiply(-2, 3) == -6
    assert multiply(0, 5) == 0

def test_divide():
    assert divide(10, 2) == 5
    assert divide(7, 2) == 3.5
    assert divide(5, 0) == "Error: Division by zero"
EOF

# Add test requirements
echo "pytest>=6.0" > requirements.txt

# Commit tests
git add tests/ requirements.txt
git commit -m "Add calculator tests and requirements"
```

#### Step 3: Push and Watch Pipeline
```bash
# Push to trigger pipeline
git push origin main
```

**Visual Learning:** 
1. Go to GitHub repository
2. Click "Actions" tab
3. Watch the pipeline run
4. Explain each step as it executes

### Advanced CI/CD Pipeline with Multiple Stages

#### Step 4: Enhanced Pipeline with Build, Test, and Deploy
```bash
# Create advanced pipeline
cat > .github/workflows/advanced-ci.yml << 'EOF'
name: Advanced CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  # Build Stage
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v3
      with:
        python-version: 3.9
    
    - name: Cache dependencies
      uses: actions/cache@v3
      with:
        path: ~/.cache/pip
        key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements.txt') }}
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
        pip install flake8 pytest pytest-cov
    
    - name: Lint code
      run: |
        flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
        flake8 . --count --exit-zero --max-complexity=10 --max-line-length=127 --statistics

  # Test Stage
  test:
    needs: build
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        python-version: [3.8, 3.9, '3.10']
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v3
      with:
        python-version: ${{ matrix.python-version }}
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
        pip install pytest pytest-cov
    
    - name: Run tests with coverage
      run: |
        pytest tests/ --cov=. --cov-report=xml
    
    - name: Upload coverage to Codecov
      uses: codecov/codecov-action@v3
      with:
        file: ./coverage.xml
        flags: unittests
        name: codecov-umbrella

  # Code Quality Stage
  code-quality:
    needs: build
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0  # Shallow clones should be disabled for better relevancy
    
    - name: SonarCloud Scan
      uses: SonarSource/sonarcloud-github-action@master
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}

  # Deploy Stage (only on main branch)
  deploy:
    needs: [test, code-quality]
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Deploy to staging
      run: |
        echo "Deploying to staging environment..."
        # Add your deployment commands here
        echo "Deployment completed successfully!"
EOF

# Update requirements
cat > requirements.txt << 'EOF'
pytest>=6.0
flake8>=4.0
pytest-cov>=3.0
EOF

# Commit advanced pipeline
git add .github/workflows/advanced-ci.yml requirements.txt
git commit -m "Add advanced CI/CD pipeline with multiple stages"
```

#### Step 5: SonarQube Integration Setup
```bash
# Create SonarQube configuration
cat > sonar-project.properties << 'EOF'
sonar.projectKey=your-project-key
sonar.organization=your-organization
sonar.sources=.
sonar.exclusions=tests/**,**/__pycache__/**
sonar.python.coverage.reportPaths=coverage.xml
sonar.python.xunit.reportPath=test-results.xml
EOF

git add sonar-project.properties
git commit -m "Add SonarQube configuration"
```

**Theory Break: Pipeline Stages Explained**
- **Build** - Compile code, install dependencies, lint
- **Test** - Run unit tests, integration tests, coverage
- **Code Quality** - Static analysis, security scanning
- **Deploy** - Deploy to environments (staging, production)

---

## Module 5: Multi-Language Examples (45 minutes)

### Theory: Language-Specific CI/CD
Different programming languages require different tools and approaches for CI/CD.

### Hands-On Exercise 5: Create Multi-Language Examples

#### JavaScript/Node.js Example
```bash
# Create Node.js project structure
mkdir -p examples/nodejs
cd examples/nodejs

# Initialize Node.js project
npm init -y

# Create simple Express app
cat > app.js << 'EOF'
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.json({ message: 'Hello from Node.js!' });
});

app.get('/health', (req, res) => {
  res.json({ status: 'healthy', timestamp: new Date().toISOString() });
});

if (require.main === module) {
  app.listen(port, () => {
    console.log(`Server running on port ${port}`);
  });
}

module.exports = app;
EOF

# Create test file
mkdir tests
cat > tests/app.test.js << 'EOF'
const request = require('supertest');
const app = require('../app');

describe('GET /', () => {
  it('should return hello message', async () => {
    const res = await request(app).get('/');
    expect(res.statusCode).toBe(200);
    expect(res.body.message).toBe('Hello from Node.js!');
  });
});

describe('GET /health', () => {
  it('should return health status', async () => {
    const res = await request(app).get('/health');
    expect(res.statusCode).toBe(200);
    expect(res.body.status).toBe('healthy');
  });
});
EOF

# Update package.json
cat > package.json << 'EOF'
{
  "name": "nodejs-example",
  "version": "1.0.0",
  "description": "Node.js CI/CD example",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "test": "jest",
    "lint": "eslint .",
    "dev": "nodemon app.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "jest": "^29.3.1",
    "supertest": "^6.3.3",
    "eslint": "^8.30.0",
    "nodemon": "^2.0.20"
  },
  "jest": {
    "testEnvironment": "node"
  }
}
EOF

# Create Node.js workflow
mkdir -p ../../.github/workflows
cat > ../../.github/workflows/nodejs-ci.yml << 'EOF'
name: Node.js CI

on:
  push:
    paths: ['examples/nodejs/**']
  pull_request:
    paths: ['examples/nodejs/**']

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [14.x, 16.x, 18.x]
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
        cache-dependency-path: examples/nodejs/package-lock.json
    
    - name: Install dependencies
      run: |
        cd examples/nodejs
        npm ci
    
    - name: Run linter
      run: |
        cd examples/nodejs
        npm run lint
    
    - name: Run tests
      run: |
        cd examples/nodejs
        npm test
EOF

cd ../..
```

#### Java Example
```bash
# Create Java project structure
mkdir -p examples/java/src/main/java/com/example
mkdir -p examples/java/src/test/java/com/example

# Create Java application
cat > examples/java/src/main/java/com/example/Calculator.java << 'EOF'
package com.example;

public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
    
    public int subtract(int a, int b) {
        return a - b;
    }
    
    public int multiply(int a, int b) {
        return a * b;
    }
    
    public double divide(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("Division by zero");
        }
        return (double) a / b;
    }
}
EOF

# Create test file
cat > examples/java/src/test/java/com/example/CalculatorTest.java << 'EOF'
package com.example;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class CalculatorTest {
    private final Calculator calculator = new Calculator();
    
    @Test
    public void testAdd() {
        assertEquals(5, calculator.add(2, 3));
        assertEquals(0, calculator.add(-1, 1));
    }
    
    @Test
    public void testSubtract() {
        assertEquals(2, calculator.subtract(5, 3));
        assertEquals(-5, calculator.subtract(0, 5));
    }
    
    @Test
    public void testMultiply() {
        assertEquals(12, calculator.multiply(3, 4));
        assertEquals(0, calculator.multiply(0, 5));
    }
    
    @Test
    public void testDivide() {
        assertEquals(5.0, calculator.divide(10, 2));
        assertEquals(3.5, calculator.divide(7, 2));
        assertThrows(IllegalArgumentException.class, () -> calculator.divide(5, 0));
    }
}
EOF

# Create Maven pom.xml
cat > examples/java/pom.xml << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>calculator</artifactId>
    <version>1.0-SNAPSHOT</version>
    
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <junit.version>5.8.2</junit.version>
        <jacoco.version>0.8.7</jacoco.version>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.0.0-M7</version>
            </plugin>
            <plugin>
                <groupId>org.jacoco</groupId>
                <artifactId>jacoco-maven-plugin</artifactId>
                <version>${jacoco.version}</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>prepare-agent</goal>
                        </goals>
                    </execution>
                    <execution>
                        <id>report</id>
                        <phase>test</phase>
                        <goals>
                            <goal>report</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
EOF

# Create Java workflow
cat > .github/workflows/java-ci.yml << 'EOF'
name: Java CI

on:
  push:
    paths: ['examples/java/**']
  pull_request:
    paths: ['examples/java/**']

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        java-version: [11, 17, 19]
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up JDK ${{ matrix.java-version }}
      uses: actions/setup-java@v3
      with:
        java-version: ${{ matrix.java-version }}
        distribution: 'temurin'
    
    - name: Cache Maven dependencies
      uses: actions/cache@v3
      with:
        path: ~/.m2
        key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
    
    - name: Run tests
      run: |
        cd examples/java
        mvn clean test
    
    - name: Generate test report
      run: |
        cd examples/java
        mvn jacoco:report
    
    - name: Upload coverage to Codecov
      uses: codecov/codecov-action@v3
      with:
        file: ./examples/java/target/site/jacoco/jacoco.xml
EOF
```

### Commit All Examples
```bash
# Add all files
git add .

# Commit multi-language examples
git commit -m "Add Node.js and Java CI/CD examples"

# Push to trigger pipelines
git push origin main
```

**Visual Learning:** Show different pipeline runs for different languages in GitHub Actions

---

## Module 6: Advanced Topics & Best Practices (30 minutes)

### Theory: CI/CD Best Practices

#### Security Best Practices
```bash
# Create security workflow
cat > .github/workflows/security.yml << 'EOF'
name: Security Scan

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  schedule:
    - cron: '0 2 * * 1'  # Weekly security scan

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    
    - name: Run Trivy vulnerability scanner
      uses: aquasecurity/trivy-action@master
      with:
        scan-type: 'fs'
        scan-ref: '.'
        format: 'sarif'
        output: 'trivy-results.sarif'
    
    - name: Upload Trivy scan results
      uses: github/codeql-action/upload-sarif@v2
      with:
        sarif_file: 'trivy-results.sarif'
    
    - name: Dependency Review
      uses: actions/dependency-review-action@v3
      if: github.event_name == 'pull_request'
EOF
```

#### Environment-specific Deployments
```bash
# Create deployment workflow
cat > .github/workflows/deploy.yml << 'EOF'
name: Deploy

on:
  push:
    branches: [ main ]
  workflow_dispatch:
    inputs:
      environment:
        description: 'Environment to deploy to'
        required: true
        default: 'staging'
        type: choice
        options:
        - staging
        - production

jobs:
  deploy-staging:
    if: github.ref == 'refs/heads/main' || github.event.inputs.environment == 'staging'
    runs-on: ubuntu-latest
    environment: staging
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Deploy to Staging
      run: |
        echo "Deploying to staging environment..."
        echo "Environment: ${{ github.event.inputs.environment || 'staging' }}"
        # Add staging deployment commands here
    
    - name: Run smoke tests
      run: |
        echo "Running smoke tests on staging..."
        # Add smoke test commands here

  deploy-production:
    if: github.event.inputs.environment == 'production'
    runs-on: ubuntu-latest
    environment: production
    needs: deploy-staging
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Deploy to Production
      run: |
        echo "Deploying to production environment..."
        # Add production deployment commands here
    
    - name: Run production health checks
      run: |
        echo "Running production health checks..."
        # Add health check commands here
EOF
```

### Commit Advanced Features
```bash
git add .github/workflows/security.yml .github/workflows/deploy.yml
git commit -m "Add security scanning and deployment workflows"
git push origin main
```

---

## Workshop Wrap-up & Resources (15 minutes)

### Key Takeaways
1. **Git** - Version control for tracking changes
2. **GitHub** - Collaboration platform and remote repository
3. **CI/CD** - Automated testing and deployment
4. **Best Practices** - Security, testing, and deployment strategies

### Next Steps for Students
1. **Practice** - Create your own projects with CI/CD
2. **Explore** - Try different GitHub Actions from the marketplace
3. **Learn** - Dive deeper into specific tools (Docker, Kubernetes, etc.)
4. **Contribute** - Participate in open-source projects

### Additional Resources
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Git Book](https://git-scm.com/book)
- [GitHub Skills](https://skills.github.com/)
- [CI/CD Best Practices](https://docs.github.com/en/actions/guides)

### Homework Assignment
1. Create a personal project repository
2. Implement a CI/CD pipeline
3. Add tests for your code
4. Set up automatic deployment
5. Share your repository URL for review

---

## Troubleshooting Common Issues

### Git Issues
```bash
# Fix "Author identity unknown" error
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Fix merge conflicts
git status
# Edit conflicted files
git add .
git commit -m "Resolve merge conflicts"

# Undo last commit (keep changes)
git reset --soft HEAD~1
```

### GitHub Actions Issues
```bash
# Check workflow syntax
# Use GitHub's workflow validator in the Actions tab

# Debug workflow runs
# Add debug step to workflow:
- name: Debug
  run: |
    echo "Current directory: $(pwd)"
    echo "Files: $(ls -la)"
    echo "Environment: $(env)"
```

### Permission Issues
```bash
# Fix permission denied for SSH
ssh-keygen -t ed25519 -C "your_email@example.com"
# Add public key to GitHub account

# Fix HTTPS authentication
git config --global credential.helper store
```

---

## Assessment Criteria

### Practical Skills (70%)
- [ ] Successfully create and manage Git repositories
- [ ] Implement branching and merging strategies
- [ ] Create functional GitHub Actions workflows
- [ ] Demonstrate understanding of CI/CD concepts

### Theoretical Knowledge (30%)
- [ ] Explain version control concepts
- [ ] Understand CI/CD pipeline stages
- [ ] Identify best practices for collaboration
- [ ] Describe security considerations

### Bonus Points
- [ ] Implement advanced workflow features
- [ ] Create multi-language CI/CD examples
- [ ] Demonstrate troubleshooting skills
- [ ] Help other students during exercises