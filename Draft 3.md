# Complete Git, GitHub & GitHub Actions Hands-On Class Guide

## 🎯 Class Overview (45-60 minutes)
**Target Audience:** Students learning version control and CI/CD basics  
**Format:** Interactive hands-on session with real-time command execution

---

## 📚 Timeline Analogy: Understanding Git Like Time Travel

### The Flash/Loki Time Travel Analogy
Think of Git like **The Flash's ability to travel through time and create alternate timelines**:

- **Git Repository** = The main timeline (like the Sacred Timeline in Loki)
- **Commits** = Snapshots of moments in time that you can return to
- **Branches** = Alternate timelines/realities you can create and explore
- **Merging** = Combining different timelines back together
- **GitHub** = The Time Variance Authority (TVA) - the central hub that monitors and manages all timelines
- **GitHub Actions** = Automated guardians that test and validate changes across timelines

Just like how Barry Allen can:
1. **Save checkpoint moments** (commits)
2. **Create alternate timelines** (branches) 
3. **Jump between different realities** (checkout)
4. **Merge timelines** without breaking reality (merge)

Git lets developers do the same with code!

---

## 🚀 Class Structure

### Phase 1: Git Fundamentals (15 minutes)
### Phase 2: GitHub Collaboration (15 minutes)  
### Phase 3: GitHub Actions CI/CD (15 minutes)
### Phase 4: SonarQube Integration & Multi-language Examples (10 minutes)

---

## 📋 Prerequisites & Setup

### Student Requirements:
- Git installed locally
- GitHub account created
- Code editor (VS Code recommended)
- Terminal/Command Prompt access

### Preparation Checklist:
- [ ] Sample repository prepared
- [ ] GitHub Classroom/Organization setup (optional)
- [ ] Demo projects in multiple languages ready
- [ ] Screen sharing setup for live coding

---

## 🎬 Phase 1: Git Fundamentals (15 minutes)

### 1.1 What is Git? (2 minutes)
**Explain:** Git is like having a time machine for your code. Every change you make can be saved as a "snapshot" that you can return to later.

### 1.2 Initial Setup (3 minutes)
```bash
# Configure Git (students type along)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Verify configuration
git config --list
```

### 1.3 Creating Your First Repository (5 minutes)
```bash
# Create a new directory
mkdir my-first-repo
cd my-first-repo

# Initialize Git repository
git init

# Check status (empty repo)
git status

# Create a simple file
echo "# My First Project" > README.md
echo "print('Hello, World!')" > hello.py

# Check status again (untracked files)
git status
```

**Visual Learning:** Show the difference in `git status` output before and after creating files.

### 1.4 Your First Commit (5 minutes)
```bash
# Stage files (prepare for commit)
git add README.md
git add hello.py
# OR: git add .

# Check staged files
git status

# Create your first commit (snapshot)
git commit -m "Initial commit: Add README and hello.py"

# View commit history
git log --oneline
```

**Key Concept:** Explain staging area as "preparation room" before permanent snapshot.

---

## 🌐 Phase 2: GitHub Collaboration (15 minutes)

### 2.1 Understanding GitHub (2 minutes)
**Explain:** If Git is your local time machine, GitHub is like the **Time Variance Authority (TVA)** - a central hub where all timelines (repositories) are stored and managed in the cloud.

### 2.2 Creating GitHub Repository (3 minutes)
```bash
# Connect local repo to GitHub
git remote add origin https://github.com/username/my-first-repo.git

# Push to GitHub
git push -u origin main

# Check remote connections
git remote -v
```

**Live Demo:** Show the repository appearing on GitHub web interface.

### 2.3 Cloning & Forking (5 minutes)

#### Cloning (Making a local copy):
```bash
# Clone a repository
git clone https://github.com/username/demo-project.git
cd demo-project
```

#### Forking (Creating your own copy):
**Demo:** Show forking process on GitHub web interface, then:
```bash
# Clone your fork
git clone https://github.com/your-username/demo-project.git
cd demo-project

# Add upstream remote (original repo)
git remote add upstream https://github.com/original-owner/demo-project.git
```

### 2.4 Branching & Merging (5 minutes)
```bash
# Create and switch to new branch
git checkout -b feature/add-calculator
# OR: git switch -c feature/add-calculator

# Create new file
echo "def add(a, b): return a + b" > calculator.py

# Commit changes
git add calculator.py
git commit -m "Add calculator function"

# Switch back to main
git checkout main
# OR: git switch main

# Merge feature branch
git merge feature/add-calculator

# Delete feature branch
git branch -d feature/add-calculator
```

**Visual Learning:** Show branch visualization using `git log --graph --oneline --all`

---

## 🔄 Phase 3: GitHub Actions CI/CD (15 minutes)

### 3.1 What is CI/CD? (2 minutes)
**Explain:** CI/CD is like having **automated guardians** (like the TVA's timeline monitors) that:
- **Continuous Integration (CI):** Automatically test every change
- **Continuous Deployment (CD):** Automatically deploy approved changes

### 3.2 Creating Your First GitHub Action (8 minutes)

Create `.github/workflows/ci.yml`:
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  # Build Stage
  build:
    runs-on: ubuntu-latest
    name: Build Application
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.9'
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    
    - name: Build application
      run: |
        echo "Building application..."
        python -m py_compile *.py

  # Test Stage
  test:
    runs-on: ubuntu-latest
    needs: build
    name: Run Tests
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.9'
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install pytest
    
    - name: Run tests
      run: |
        pytest tests/ || echo "No tests found, skipping..."

  # Deploy Stage
  deploy:
    runs-on: ubuntu-latest
    needs: [build, test]
    name: Deploy Application
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Deploy to staging
      run: |
        echo "Deploying to staging environment..."
        echo "Application deployed successfully!"
```

### 3.3 Adding Required Files (3 minutes)
```bash
# Create requirements.txt
echo "pytest==7.4.0" > requirements.txt

# Create a simple test
mkdir tests
echo "def test_hello():
    assert 'Hello' in 'Hello, World!'" > tests/test_hello.py

# Commit and push
git add .
git commit -m "Add CI/CD pipeline"
git push origin main
```

### 3.4 Watching the Pipeline (2 minutes)
**Live Demo:** Show the Actions tab on GitHub, explain the pipeline stages running.

---

## 🔍 Phase 4: SonarQube Integration & Multi-language Examples (10 minutes)

### 4.1 Adding SonarQube to Pipeline (5 minutes)

Enhanced `.github/workflows/ci.yml` with SonarQube:
```yaml
name: CI/CD Pipeline with Code Analysis

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  # Code Quality Analysis
  sonarqube:
    runs-on: ubuntu-latest
    name: SonarQube Analysis
    
    steps:
    - uses: actions/checkout@v3
      with:
        fetch-depth: 0  # Shallow clones should be disabled for better analysis
    
    - name: SonarQube Scan
      uses: sonarqube-quality-gate-action@master
      env:
        SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
        SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}

  # Build Stage
  build:
    runs-on: ubuntu-latest
    needs: sonarqube
    name: Build Application
    
    strategy:
      matrix:
        python-version: [3.8, 3.9, 3.10]
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up Python ${{ matrix.python-version }}
      uses: actions/setup-python@v4
      with:
        python-version: ${{ matrix.python-version }}
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    
    - name: Lint with flake8
      run: |
        pip install flake8
        flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
    
    - name: Build application
      run: |
        echo "Building application..."
        python -m py_compile *.py

  # Security Scan
  security:
    runs-on: ubuntu-latest
    name: Security Scan
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Run security scan
      uses: securecodewarrior/github-action-add-sarif@v1
      with:
        sarif-file: 'security-scan-results.sarif'
```

### 4.2 Multi-language Pipeline Examples (5 minutes)

#### Node.js Example:
```yaml
# .github/workflows/nodejs-ci.yml
name: Node.js CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [14, 16, 18]
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm test
    
    - name: SonarQube Scan
      uses: sonarqube-quality-gate-action@master
      env:
        SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

#### Java Example:
```yaml
# .github/workflows/java-ci.yml
name: Java CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'temurin'
    
    - name: Build with Maven
      run: mvn clean compile
    
    - name: Run tests
      run: mvn test
    
    - name: SonarQube Analysis
      run: mvn sonar:sonar
      env:
        SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```

---

## 🎯 Hands-On Project: "Code Timeline Manager"

### Project Structure:
```
code-timeline-manager/
├── README.md
├── requirements.txt
├── app.py
├── tests/
│   └── test_app.py
├── .github/
│   └── workflows/
│       └── ci.yml
└── sonar-project.properties
```

### Student Exercise:
1. **Fork the demo repository**
2. **Clone their fork locally**
3. **Create a feature branch**
4. **Add a new function**
5. **Write a test for the function**
6. **Commit and push changes**
7. **Create a pull request**
8. **Watch the CI/CD pipeline run**

---

## 🧠 Key Concepts Explained

### Git Workflow (The Flash Timeline Analogy):
1. **Working Directory** = Current reality where Barry Allen is
2. **Staging Area** = The Speed Force (preparation zone)
3. **Repository** = The complete timeline history
4. **Remote Repository** = The shared multiverse (GitHub)

### CI/CD Pipeline Stages:
1. **Build** = Compile and prepare the application
2. **Test** = Run automated tests to catch bugs
3. **Code Analysis** = SonarQube checks for code quality
4. **Security Scan** = Check for vulnerabilities
5. **Deploy** = Release to production environment

### SonarQube Integration Benefits:
- **Code Quality Metrics**
- **Security Vulnerability Detection**
- **Code Duplication Analysis**
- **Technical Debt Assessment**
- **Maintainability Index**

---

## 📊 Interactive Demo Commands

### Students Follow Along:
```bash
# 1. Initial setup
git clone https://github.com/instructor/demo-project.git
cd demo-project

# 2. Create feature branch
git checkout -b feature/my-improvement

# 3. Make changes
echo "# My improvement" >> README.md

# 4. Stage and commit
git add README.md
git commit -m "Add my improvement to README"

# 5. Push and create PR
git push origin feature/my-improvement
```

---

## 🎨 Visual Learning Aids

### Git Status Visualization:
```bash
# Show different states
git status          # Clean working directory
touch new-file.txt  # Untracked file
git add new-file.txt # Staged file
git commit -m "msg"  # Committed file
```

### Branch Visualization:
```bash
git log --graph --oneline --all
```

### Pipeline Status:
- Show GitHub Actions tab
- Explain green checkmarks vs red X's
- Demonstrate pipeline logs

---

## 🔧 Troubleshooting Common Issues

### Git Issues:
- **Permission denied**: Check SSH keys
- **Merge conflicts**: Show resolution process
- **Detached HEAD**: Explain and fix

### GitHub Actions Issues:
- **Failed builds**: Check logs
- **Secret variables**: Explain setup
- **Workflow syntax**: YAML validation

---

## 📈 Assessment & Next Steps

### What Students Should Know:
- [ ] Basic Git commands (add, commit, push, pull)
- [ ] GitHub workflow (clone, fork, PR)
- [ ] CI/CD pipeline concepts
- [ ] SonarQube integration basics

### Advanced Topics (Future Classes):
- Git rebase and advanced branching
- Docker integration in CI/CD
- Deployment strategies
- Advanced GitHub Actions features

---

## 🎬 Class Conclusion

### Recap Timeline Analogy:
"You've just learned to be like The Flash - you can now:
- ✅ Travel through your code's timeline (Git)
- ✅ Create alternate realities safely (Branches)
- ✅ Collaborate across the multiverse (GitHub)
- ✅ Have automated guardians protect your timeline (GitHub Actions)
- ✅ Ensure code quality across all realities (SonarQube)"

### Final Challenge:
"Create your own timeline, add a feature, and watch your automated guardians test it!"

---

## 📚 Resources for Students

### Documentation:
- [Git Documentation](https://git-scm.com/doc)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [SonarQube Documentation](https://docs.sonarqube.org/)

### Practice Platforms:
- [GitHub Learning Lab](https://lab.github.com/)
- [Learn Git Branching](https://learngitbranching.js.org/)
- [Git Immersion](https://gitimmersion.com/)

### Quick Reference:
```bash
# Essential Git Commands
git init          # Initialize repository
git add .         # Stage all changes
git commit -m ""  # Commit with message
git push          # Push to remote
git pull          # Pull from remote
git branch        # List branches
git checkout -b   # Create and switch branch
git merge         # Merge branches
git status        # Check status
git log           # View history
```