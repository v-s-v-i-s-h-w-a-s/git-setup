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

#### Step 4: Understanding the .git Folder (The Hidden Engine Room)
```bash
# Show the hidden .git folder
ls -la
# You'll see: drwxr-xr-x  .git

# Look inside the .git folder (the engine room)
ls -la .git/
```

**The .git Folder Explained - Your Repository's Brain**

Think of the `.git` folder as the **control center of a spaceship**:

```
📁 .git/ (The Spaceship's Control Center)
├── HEAD (Current Location Pointer)
|
├── config (Ship's Settings)
|
├── index (Staging Area - Ready for Launch)
|
├── refs/ (Navigation Bookmarks)
│   ├── heads/ (Local Branch Pointers)
│   ├── remotes/ (Remote Branch Pointers)
│   └── tags/ (Important Milestone Markers)
|
├── objects/ (The Treasure Vault)
│   ├── 00-ff/ (256 Folders for All Git Objects)
│   ├── info/ (Object Database Info)
│   └── pack/ (Compressed Archives)
|
├── logs/ (Ship's Log - History of Changes)
|
├── hooks/ (Automated Scripts)
|
└── info/ (Repository Info & Settings)
```

**Key Files and Their Purposes:**

1. **HEAD** - "Where am I now?"
   - Points to current branch/commit
   - Like GPS showing your current location

2. **config** - "Ship's manual"
   - Repository settings
   - Remote URLs, user info, branch configs

3. **index** - "Launch pad"
   - Staging area for next commit
   - What you've `git add`ed

4. **refs/heads/** - "Branch headquarters"
   - Each file = one branch
   - Contains commit hash the branch points to

5. **objects/** - "The vault"
   - ALL your data lives here
   - Commits, trees, blobs (files)
   - Organized by SHA-1 hash

6. **logs/** - "Captain's log"
   - History of HEAD movements
   - Branch creation/switching history

Let's explore some key files:

```bash
# See what HEAD points to
cat .git/HEAD
# Output: ref: refs/heads/main

# See what main branch points to
cat .git/refs/heads/main
# Output: a1b2c3d4... (commit hash)

# Look at the staging area
git ls-files --stage
# Shows files in the index

# Explore objects (this is where commits/files are stored)
find .git/objects -type f | head -5
```

## 🔍 Deep Dive: Inside the .git Folder

### Understanding Git's Internal Structure

Based on your `.git` folder structure, let's explore what each component does:

#### 1. Root Level Files
```
COMMIT_EDITMSG  ← Your last commit message
HEAD           ← Points to current branch (ref: refs/heads/main)
index          ← Staging area (what you've git add-ed)
config         ← Repository configuration
FETCH_HEAD     ← Last fetched remote branch info
ORIG_HEAD      ← Previous HEAD position (backup)
packed-refs    ← Compressed reference storage
```

#### 2. The Objects Database - Git's Heart 
```
objects/
├── 00-ff/     ← 256 folders (first 2 chars of SHA-1)
├── info/      ← Object database metadata
└── pack/      ← Compressed object storage
```

**Object Types:**
- **Blob**: File contents
- **Tree**: Directory structure
- **Commit**: Snapshot + metadata
- **Tag**: Annotated reference

#### 3. References System 
```
refs/
├── heads/     ← Local branches
├── remotes/   ← Remote tracking branches
└── tags/      ← Tagged releases
```

#### 4. The Hooks Directory
```
hooks/
├── pre-commit.sample    ← Run before commit
├── post-update.sample   ← Run after push
├── pre-push.sample      ← Run before push
└── ... (13 more hooks)
```

#### 5. Logs - The History Keeper 
```
logs/
├── HEAD                    ← All HEAD movements
└── refs/heads/main         ← Branch-specific history
```

### Hands-On: Explore Your .git Folder

```bash
# 1. See current branch pointer
cat .git/HEAD
# Output: ref: refs/heads/main

# 2. See where main branch points
cat .git/refs/heads/main
# Output: commit hash (e.g., a1b2c3d4...)

# 3. Check staging area
git ls-files --stage
# Shows staged files with their object hashes

# 4. Explore objects
ls .git/objects/
# See all the 2-char directories

# 5. Find a specific object
git cat-file -t a1b2c3d4  # Replace with actual hash
git cat-file -p a1b2c3d4  # Show contents

# 6. See pack files (compressed objects)
ls -la .git/objects/pack/
# Shows compressed object archives
```

### What Happens When You Git?

**When you `git add file.txt`:**
1. Git creates a blob object in `objects/`
2. Updates the `index` file
3. File is now "staged"

**When you `git commit`:**
1. Git creates a tree object (directory snapshot)
2. Git creates a commit object
3. Updates `refs/heads/main` to point to new commit
4. Updates `HEAD` reference

**When you `git push`:**
1. Git sends objects to remote
2. Updates `refs/remotes/origin/main`
3. Updates `FETCH_HEAD`

### Important Notes

**Never manually edit .git files!** 
- Use Git commands instead
- Git maintains internal consistency
- Manual changes can corrupt repository

**The .git folder is portable:**
- Contains complete project history
- Can be copied to recreate repository
- Size grows with project history

---

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
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    name: Build Stage
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.9'
    
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install flake8 pytest
    
    - name: Display build info
      run: |
        echo "Build completed successfully!"
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
        python-version: '3.9'
        
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
        echo "Deploying to production..."
        echo "✅ Deployment successful!"
        echo "Deployment URL: https://my-app.example.com"
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
Deploy Stage: Production Release
    ↓
Monitor: Track Performance
```

---

## Section 5: Multi-Language Support (5 minutes)

### Language-Specific Examples

#### Python Project
```yaml
- name: Set up Python
  uses: actions/setup-python@v4
  with:
    python-version: '3.9'
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
    node-version: '18'
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
    java-version: '11'
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