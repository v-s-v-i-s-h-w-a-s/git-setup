# Git, GitHub and GitHub Actions tools setup and hands-on Guide
*Duration: 45-60 minutes | Smooth Progressive Flow*

## Workshop Flow Overview 

### The Learning Journey
```
Local Git → GitHub Connection → Branching & PRs → Fork/Clone → CI/CD Pipeline
   ↓              ↓                    ↓              ↓           ↓
Foundation → Remote Storage → Collaboration → Open Source → Automation
```

### Timeline Analogy 
- **Local Git** = Your personal time machine
- **GitHub** = The central timeline archive
- **Branches** = Alternate timelines you can create
- **Pull Requests** = Asking to merge timelines
- **Forking** = Creating your own copy of someone else's timeline
- **GitHub Actions** = Automated timeline guardians

---

## Part 1: Local Git Foundation (10 minutes)

### Setup & First Repository
```bash
# Check Git installation
git --version

# Configure Git (first time only)
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Create your first project
mkdir my-first-project
cd my-first-project
git init
```

**What just happened?** 
- Created a `.git` folder (your time machine database)
- Git is now tracking this directory

### Basic Git Workflow - The Three States
```bash
# Create a simple file
echo "# My First Project" > README.md
echo "console.log('Hello World!');" > app.js

# Check status (see what Git notices)
git status

# Stage files (prepare for commit)
git add README.md
git add app.js
# or add all: git add .

# Commit (create a snapshot in time)
git commit -m "Initial project setup"

# View history
git log --oneline
```

**Visual Exercise:** Show how `git status` changes at each step

---

## Part 2: Connecting to GitHub (8 minutes)

### Creating Remote Repository
1. **Go to GitHub.com**
2. **Click "New Repository"**
3. **Name it "my-first-project"**
4. **DON'T initialize with README** (we already have files)
5. **Copy the repository URL**

### Linking Local to Remote
```bash
# Add remote connection
git remote add origin https://github.com/YOUR-USERNAME/my-first-project.git

# Verify remote connection
git remote -v

# Push to GitHub for the first time
git push -u origin main

# Future pushes (after first time)
git push
```

**What just happened?**
- `origin` = nickname for your GitHub repository
- `-u` = sets up tracking between local and remote branches
- Your local timeline is now backed up in the cloud!

### Making Changes and Syncing
```bash
# Make changes to app.js
echo "console.log('Updated version!');" >> app.js

# Stage and commit
git add app.js
git commit -m "Add updated message"

# Push to GitHub
git push

# Check GitHub - see your changes appear!
```

---

## Part 3: Branching, Pull Requests & Collaboration (15 minutes)

### Understanding Branches - Multiple Timelines
```bash
# See current branches
git branch

# Create a new branch (new timeline)
git branch feature-login

# Switch to the new branch
git checkout feature-login
# or create and switch in one command:
# git checkout -b feature-login

# Verify you're on the new branch
git branch
```

### Working on Feature Branch
```bash
# Create a new file for login feature
echo "function login() { console.log('User logged in'); }" > login.js

# Stage and commit on feature branch
git add login.js
git commit -m "Add login functionality"

# Push feature branch to GitHub
git push -u origin feature-login
```

### Creating Pull Request
1. **Go to GitHub repository**
2. **Click "Compare & pull request"** (appears after pushing branch)
3. **Add description:** "Add login functionality"
4. **Click "Create pull request"**

**What is a Pull Request?**
- A request to merge your feature timeline into the main timeline
- Allows code review and discussion
- Shows exactly what changes you're proposing

### Merging and Cleanup
```bash
# After PR is approved and merged on GitHub:
# Switch back to main branch
git checkout main

# Pull the merged changes
git pull origin main

# Delete the feature branch (cleanup)
git branch -d feature-login

# Delete remote branch
git push origin --delete feature-login
```

### Git Reset - Time Travel Backwards
```bash
# See commit history
git log --oneline

# Create a mistake to demonstrate reset
echo "This is a mistake" > mistake.txt
git add mistake.txt
git commit -m "Add mistake file"

# Reset to previous commit (undo last commit)
git reset --hard HEAD~1

# Verify the mistake is gone
ls
git log --oneline
```

**Reset Options:**
- `--soft`: Keep changes in staging area
- `--mixed`: Keep changes in working directory
- `--hard`: Completely remove changes

---

## Part 4: Forking vs Cloning - The Key Difference (10 minutes)

### Instructor Setup (Do this before class)
Create a demo repository: `https://github.com/v-s-v-i-s-h-w-a-s/git-demo.git`

### Fork vs Clone Demonstration

#### Step 1: Students Fork First
**Instructions for Students:**
1. **Go to:** `https://github.com/v-s-v-i-s-h-w-a-s/git-demo.git`
2. **Click "Fork"** in the top right
3. **Select your account** as the destination
4. **Wait for fork to complete**

**What is Forking?**
- Creates YOUR own copy of someone else's repository
- You can make changes without affecting the original
- Perfect for contributing to open source projects

#### Step 2: Clone Your Fork
```bash
# Clone YOUR fork (not the original)
git clone https://github.com/YOUR-STUDENT-USERNAME/git-demo-repo.git
cd git-demo-repo

# Check remotes
git remote -v
# You'll see: origin points to YOUR fork
```

#### Step 3: Add Upstream Remote
```bash
# Add connection to original repository
git remote add upstream https://github.com/v-s-v-i-s-h-w-a-s/git-demo.git

# Verify remotes
git remote -v
# Now you see:
# origin    -> your fork
# upstream  -> original repository
```

#### Step 4: Make Changes to Your Fork
```bash
# Create a new feature
git checkout -b add-student-name

# Add your name to a file
echo "Student: YOUR-NAME" >> contributors.txt

# Commit and push to YOUR fork
git add contributors.txt
git commit -m "Add my name to contributors"
git push origin add-student-name
```

#### Step 5: Create Pull Request to Original
1. **Go to your fork on GitHub**
2. **Click "Compare & pull request"**
3. **Notice:** Base repository is the instructor's original repo
4. **Create pull request**

### When to Fork vs Clone

| Fork | Clone |
|------|-------|
| ✅ Contributing to open source | ✅ Your own repositories |
| ✅ No write access to original | ✅ Team repositories with access |
| ✅ Want to experiment safely | ✅ Direct collaboration |
| ✅ Making significant changes | ✅ Simple local development |

---

## Part 5: GitHub Actions CI/CD Pipeline (15 minutes)

### What is CI/CD? 
**Continuous Integration/Continuous Deployment**
- **CI:** Automatically test code when changes are made
- **CD:** Automatically deploy code when tests pass

**GitHub Actions = Your Code's Automated Assistant**

### Creating Your First Workflow

#### Step 1: Create Workflow File
```bash
# In your repository, create the workflow directory
mkdir -p .github/workflows

# Create the workflow file
touch .github/workflows/ci.yml
```

#### Step 2: Simple CI Pipeline
```yaml
# .github/workflows/ci.yml
name: Simple CI Pipeline

# When should this run?
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

# What jobs should run?
jobs:
  test-and-build:
    runs-on: ubuntu-latest
    
    steps:
    # Step 1: Get the code
    - name: Checkout code
      uses: actions/checkout@v4
    
    # Step 2: Setup Node.js
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '18'
    
    # Step 3: Install dependencies
    - name: Install dependencies
      run: |
        echo "Installing dependencies..."
        npm init -y
        npm install --save-dev jest
    
    # Step 4: Run tests
    - name: Run tests
      run: |
        echo "Running tests..."
        # If you have tests: npm test
        echo "All tests passed!"
    
    # Step 5: Build application
    - name: Build application
      run: |
        echo "Building application..."
        echo "Build completed successfully!"
```

#### Step 3: Create Package.json and Test
```bash
# Create package.json
npm init -y

# Add test script
echo '{
  "name": "workshop-project",
  "version": "1.0.0",
  "scripts": {
    "test": "echo \"All tests passed!\"",
    "build": "echo \"Build completed!\""
  }
}' > package.json
```

#### Step 4: Commit and Push
```bash
# Add all files
git add .
git commit -m "Add CI pipeline"
git push origin main
```

#### Step 5: Watch Magic Happen! 
1. **Go to GitHub repository**
2. **Click "Actions" tab**
3. **Watch your pipeline run automatically**
4. **See each step execute**

### Enhanced CI Pipeline (Advanced Demo)
```yaml
name: Advanced CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  # Stage 1: Code Quality
  quality-check:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-node@v4
      with:
        node-version: '18'
    - run: npm install
    - run: npm run lint
    
  # Stage 2: Security Scan
  security-scan:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - run: npm audit
    
  # Stage 3: Test (runs after quality and security)
  test:
    needs: [quality-check, security-scan]
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [16, 18, 20]
    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-node@v4
      with:
        node-version: ${{ matrix.node-version }}
    - run: npm install
    - run: npm test
    
  # Stage 4: Build and Deploy
  build-deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
    - uses: actions/checkout@v4
    - run: npm run build
    - run: echo "Deploy to production!"
```

---

## Part 6: Integration & Review (2 minutes)

### Complete Workflow Recap
```
1. Local Git Repository → 2. Connect to GitHub → 3. Feature Branches
                                     ↓
6. CI/CD Automation ← 5. Clone Your Fork ← 4. Fork Original Project
```

### What Students Learned
1. ✅ **Local Git:** Initialize, add, commit, push
2. ✅ **GitHub Connection:** Remote repositories, syncing
3. ✅ **Branching:** Feature development, merging, cleanup
4. ✅ **Pull Requests:** Code review workflow
5. ✅ **Git Reset:** Time travel and mistake recovery
6. ✅ **Fork vs Clone:** When and why to use each
7. ✅ **CI/CD Pipeline:** Automated testing and deployment

### Key Commands Summary
```bash
# Local Git
git init, git add, git commit, git log

# Remote Connection
git remote add origin, git push, git pull

# Branching
git branch, git checkout, git merge

# Reset
git reset --hard HEAD~1

# Fork Workflow
git clone, git remote add upstream

# CI/CD
Create .github/workflows/ci.yml
```

---

## Why This Flow Works Perfectly 

### 1. **Natural Progression**
- Start with basics students can see locally
- Add complexity gradually
- Each concept builds on the previous

### 2. **Hands-On Learning**
- Students execute every command
- Immediate visual feedback
- Real-world scenarios

### 3. **Smooth Transitions**
- Local → Remote (natural extension)
- Branching → PRs (logical workflow)
- Fork/Clone → CI/CD (collaboration to automation)

### 4. **Clear Distinctions**
- Students experience fork vs clone practically
- See why each approach is used
- Understand when to use which method

### 5. **Automation Payoff**
- End with the "wow factor" of automation
- Students see their code automatically tested
- Demonstrates professional development practices

This flow creates a logical learning journey that feels natural and builds confidence at each step! 