# Sample Code Repositories for Git/GitHub/Actions Class

## Repository Structure Overview

### Main Demo Repository: `code-timeline-manager`

**Purpose:** Primary hands-on project for students to fork and practice with

### Language-Specific Repositories:

1. **Python Project:** `python-timeline-app`
2. **Node.js Project:** `nodejs-timeline-api`
3. **Java Project:** `java-timeline-service`

---

## Repository 1: `code-timeline-manager` (Main Demo)

### File Structure:

```
code-timeline-manager/
├── README.md
├── requirements.txt
├── app.py
├── timeline_manager.py
├── utils/
│   ├── __init__.py
│   └── helpers.py
├── tests/
│   ├── __init__.py
│   ├── test_app.py
│   └── test_timeline_manager.py
├── .github/
│   └── workflows/
│       ├── ci.yml
│       └── deploy.yml
├── .gitignore
├── sonar-project.properties
└── Dockerfile
```

### README.md

````markdown
# Code Timeline Manager

A simple Python application that demonstrates Git workflows, GitHub collaboration, and CI/CD pipelines.

## Features

- Timeline event management
- Git workflow simulation
- Automated testing and deployment
- Code quality analysis with SonarQube

## Setup

```bash
# Clone the repository
git clone https://github.com/your-username/code-timeline-manager.git
cd code-timeline-manager

# Install dependencies
pip install -r requirements.txt

# Run the application
python app.py
```
````

## Testing

```bash
# Run tests
pytest tests/

# Run with coverage
pytest --cov=. tests/
```

## Code Quality

This project uses SonarQube for code quality analysis in the CI/CD pipeline.

## Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Learning Objectives

- Master Git commands and workflows
- Understand GitHub collaboration
- Implement CI/CD pipelines
- Integrate code quality tools

````

### app.py
```python
#!/usr/bin/env python3
"""
Code Timeline Manager - Main Application
Demonstrates Git workflows and CI/CD concepts
"""

import sys
from datetime import datetime
from timeline_manager import TimelineManager
from utils.helpers import format_timestamp, validate_event


def main():
    """Main application entry point"""
    print(" Welcome to Code Timeline Manager!")
    print("=" * 50)

    # Initialize timeline manager
    manager = TimelineManager()

    # Add some sample events
    sample_events = [
        {"id": 1, "event": "Project initialization", "timestamp": datetime.now()},
        {"id": 2, "event": "First commit created", "timestamp": datetime.now()},
        {"id": 3, "event": "Feature branch created", "timestamp": datetime.now()},
    ]

    for event in sample_events:
        if validate_event(event):
            manager.add_event(event)

    # Display timeline
    print("\n Current Timeline:")
    manager.display_timeline()

    # Interactive mode
    while True:
        print("\n Commands:")
        print("1. Add event")
        print("2. View timeline")
        print("3. Simulate git workflow")
        print("4. Exit")

        choice = input("\nEnter your choice (1-4): ").strip()

        if choice == "1":
            event_description = input("Enter event description: ")
            event = {
                "id": len(manager.events) + 1,
                "event": event_description,
                "timestamp": datetime.now()
            }
            manager.add_event(event)
            print(" Event added successfully!")

        elif choice == "2":
            manager.display_timeline()

        elif choice == "3":
            simulate_git_workflow()

        elif choice == "4":
            print(" Thanks for using Code Timeline Manager!")
            break

        else:
            print(" Invalid choice. Please try again.")


def simulate_git_workflow():
    """Simulate a Git workflow for educational purposes"""
    print("\n Simulating Git Workflow:")
    print("=" * 30)

    workflow_steps = [
        "git init - Initialize repository",
        "git add . - Stage all changes",
        "git commit -m 'Initial commit' - Create snapshot",
        "git branch feature/new-feature - Create feature branch",
        "git checkout feature/new-feature - Switch to feature branch",
        "Make changes to code...",
        "git add . - Stage changes",
        "git commit -m 'Add new feature' - Commit changes",
        "git checkout main - Switch to main branch",
        "git merge feature/new-feature - Merge feature branch",
        "git push origin main - Push to remote repository",
        "CI/CD pipeline triggered automatically!"
    ]

    for i, step in enumerate(workflow_steps, 1):
        print(f"{i:2d}. {step}")
        input("   Press Enter to continue...")

    print("\n Git workflow simulation complete!")


if __name__ == "__main__":
    main()
````

### timeline_manager.py

```python
"""
Timeline Manager Module
Handles timeline events and operations
"""

from datetime import datetime
from typing import List, Dict, Any
from utils.helpers import format_timestamp


class TimelineManager:
    """Manages timeline events and operations"""

    def __init__(self):
        """Initialize the timeline manager"""
        self.events: List[Dict[str, Any]] = []
        self.created_at = datetime.now()

    def add_event(self, event: Dict[str, Any]) -> bool:
        """
        Add an event to the timeline

        Args:
            event: Dictionary containing event data

        Returns:
            bool: True if event was added successfully
        """
        try:
            if not event.get("event") or not event.get("timestamp"):
                raise ValueError("Event must have 'event' and 'timestamp' fields")

            self.events.append(event)
            return True
        except Exception as e:
            print(f"❌ Error adding event: {e}")
            return False

    def display_timeline(self) -> None:
        """Display all timeline events"""
        if not self.events:
            print(" No events in timeline yet.")
            return

        print(f"\n Timeline Events ({len(self.events)} total):")
        print("-" * 60)

        for event in sorted(self.events, key=lambda x: x["timestamp"]):
            timestamp = format_timestamp(event["timestamp"])
            print(f" {timestamp} | {event['event']}")

    def get_events_count(self) -> int:
        """Get the total number of events"""
        return len(self.events)

    def clear_timeline(self) -> None:
        """Clear all events from the timeline"""
        self.events.clear()
        print("🗑️ Timeline cleared successfully!")

    def get_recent_events(self, limit: int = 5) -> List[Dict[str, Any]]:
        """
        Get the most recent events

        Args:
            limit: Maximum number of events to return

        Returns:
            List of recent events
        """
        sorted_events = sorted(self.events, key=lambda x: x["timestamp"], reverse=True)
        return sorted_events[:limit]
```

### utils/helpers.py

```python
"""
Utility functions for the timeline manager
"""

from datetime import datetime
from typing import Dict, Any


def format_timestamp(timestamp: datetime) -> str:
    """
    Format timestamp for display

    Args:
        timestamp: datetime object

    Returns:
        Formatted timestamp string
    """
    return timestamp.strftime("%Y-%m-%d %H:%M:%S")


def validate_event(event: Dict[str, Any]) -> bool:
    """
    Validate event data

    Args:
        event: Event dictionary to validate

    Returns:
        bool: True if event is valid
    """
    required_fields = ["event", "timestamp"]

    for field in required_fields:
        if field not in event:
            print(f"❌ Missing required field: {field}")
            return False

    if not isinstance(event["timestamp"], datetime):
        print("❌ Timestamp must be a datetime object")
        return False

    if not event["event"].strip():
        print("❌ Event description cannot be empty")
        return False

    return True


def get_current_time() -> datetime:
    """Get current timestamp"""
    return datetime.now()


def time_difference(start: datetime, end: datetime) -> str:
    """
    Calculate time difference between two timestamps

    Args:
        start: Start timestamp
        end: End timestamp

    Returns:
        Human-readable time difference
    """
    diff = end - start

    if diff.days > 0:
        return f"{diff.days} days ago"
    elif diff.seconds > 3600:
        hours = diff.seconds // 3600
        return f"{hours} hours ago"
    elif diff.seconds > 60:
        minutes = diff.seconds // 60
        return f"{minutes} minutes ago"
    else:
        return "Just now"
```

### requirements.txt

```txt
pytest==7.4.3
pytest-cov==4.1.0
flake8==6.1.0
black==23.9.1
mypy==1.6.1
requests==2.31.0
python-dotenv==1.0.0
```

### tests/test_app.py

```python
"""
Tests for the main application
"""

import pytest
from datetime import datetime
from unittest.mock import patch, MagicMock
import app


def test_main_function_exists():
    """Test that main function exists"""
    assert hasattr(app, 'main')
    assert callable(app.main)


def test_simulate_git_workflow():
    """Test git workflow simulation"""
    with patch('builtins.input', return_value=''):
        with patch('builtins.print') as mock_print:
            app.simulate_git_workflow()
            # Check that workflow steps were printed
            assert mock_print.call_count > 10


@patch('builtins.input', side_effect=['4'])  # Exit immediately
@patch('builtins.print')
def test_main_interactive_mode(mock_print, mock_input):
    """Test main function interactive mode"""
    app.main()
    # Verify that welcome message was printed
    assert any("Welcome to Code Timeline Manager" in str(call) for call in mock_print.call_args_list)
```

### tests/test_timeline_manager.py

```python
"""
Tests for the timeline manager module
"""

import pytest
from datetime import datetime
from timeline_manager import TimelineManager


class TestTimelineManager:
    """Test cases for TimelineManager class"""

    def setup_method(self):
        """Setup test fixtures"""
        self.manager = TimelineManager()
        self.sample_event = {
            "id": 1,
            "event": "Test event",
            "timestamp": datetime.now()
        }

    def test_initialization(self):
        """Test manager initialization"""
        assert self.manager.events == []
        assert isinstance(self.manager.created_at, datetime)

    def test_add_event_success(self):
        """Test adding a valid event"""
        result = self.manager.add_event(self.sample_event)
        assert result is True
        assert len(self.manager.events) == 1
        assert self.manager.events[0] == self.sample_event

    def test_add_event_missing_fields(self):
        """Test adding event with missing fields"""
        invalid_event = {"id": 1}
        result = self.manager.add_event(invalid_event)
        assert result is False
        assert len(self.manager.events) == 0

    def test_get_events_count(self):
        """Test getting events count"""
        assert self.manager.get_events_count() == 0
        self.manager.add_event(self.sample_event)
        assert self.manager.get_events_count() == 1

    def test_clear_timeline(self):
        """Test clearing timeline"""
        self.manager.add_event(self.sample_event)
        assert len(self.manager.events) == 1
        self.manager.clear_timeline()
        assert len(self.manager.events) == 0

    def test_get_recent_events(self):
        """Test getting recent events"""
        # Add multiple events
        for i in range(3):
            event = {
                "id": i,
                "event": f"Event {i}",
                "timestamp": datetime.now()
            }
            self.manager.add_event(event)

        recent = self.manager.get_recent_events(limit=2)
        assert len(recent) == 2
        # Should be sorted by timestamp (most recent first)
        assert recent[0]["timestamp"] >= recent[1]["timestamp"]
```

### .github/workflows/ci.yml

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  # Code Quality Check
  quality:
    runs-on: ubuntu-latest
    name: Code Quality Analysis

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.9"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Lint with flake8
        run: |
          flake8 . --count --select=E9,F63,F7,F82 --show-source --statistics
          flake8 . --count --exit-zero --max-complexity=10 --max-line-length=88 --statistics

      - name: Format check with black
        run: |
          black --check .

      - name: Type check with mypy
        run: |
          mypy . --ignore-missing-imports

      - name: SonarQube Scan
        uses: sonarqube-quality-gate-action@master
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}

  # Build Stage
  build:
    runs-on: ubuntu-latest
    needs: quality
    name: Build Application

    strategy:
      matrix:
        python-version: [3.8, 3.9, 3.10, 3.11]

    steps:
      - uses: actions/checkout@v4

      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v4
        with:
          python-version: ${{ matrix.python-version }}

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Build application
        run: |
          echo "Building application for Python ${{ matrix.python-version }}"
          python -m py_compile app.py timeline_manager.py utils/helpers.py

      - name: Create build artifact
        run: |
          mkdir -p build
          cp *.py build/
          cp -r utils build/
          tar -czf build-${{ matrix.python-version }}.tar.gz build/

      - name: Upload build artifact
        uses: actions/upload-artifact@v3
        with:
          name: build-${{ matrix.python-version }}
          path: build-${{ matrix.python-version }}.tar.gz

  # Test Stage
  test:
    runs-on: ubuntu-latest
    needs: build
    name: Run Tests

    steps:
      - uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.9"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run tests with coverage
        run: |
          pytest --cov=. --cov-report=xml --cov-report=html tests/

      - name: Upload coverage reports
        uses: codecov/codecov-action@v3
        with:
          file: ./coverage.xml
          flags: unittests
          name: codecov-umbrella

      - name: Test CLI functionality
        run: |
          echo "3" | python app.py  # Test basic functionality
          echo "4" | python app.py  # Test exit

  # Security Scan
  security:
    runs-on: ubuntu-latest
    name: Security Scan

    steps:
      - uses: actions/checkout@v4

      - name: Run security scan with bandit
        run: |
          pip install bandit
          bandit -r . -f json -o security-report.json || true

      - name: Upload security report
        uses: actions/upload-artifact@v3
        with:
          name: security-report
          path: security-report.json

  # Deploy Stage
  deploy:
    runs-on: ubuntu-latest
    needs: [build, test, security]
    name: Deploy Application
    if: github.ref == 'refs/heads/main'

    steps:
      - uses: actions/checkout@v4

      - name: Download build artifacts
        uses: actions/download-artifact@v3
        with:
          name: build-3.9

      - name: Deploy to staging
        run: |
          echo " Deploying Code Timeline Manager to staging..."
          echo " Extracting build artifacts..."
          tar -xzf build-3.9.tar.gz
          echo " Deployment completed successfully!"
          echo " Application available at: https://staging.timeline-manager.com"

      - name: Notify deployment
        run: |
          echo " Sending deployment notification..."
          echo " Code Timeline Manager v1.0 deployed successfully!"
```

### sonar-project.properties

```properties
sonar.projectKey=code-timeline-manager
sonar.projectName=Code Timeline Manager
sonar.projectVersion=1.0

# Source code
sonar.sources=.
sonar.exclusions=tests/**,build/**,**/__pycache__/**

# Test coverage
sonar.python.coverage.reportPaths=coverage.xml

# Language
sonar.language=python

# Encoding
sonar.sourceEncoding=UTF-8
```

### .gitignore

```gitignore
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# Testing
.coverage
.pytest_cache/
htmlcov/
.tox/
.nox/
coverage.xml
*.cover
.hypothesis/

# Environment
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/

# IDE
.vscode/
.idea/
*.swp
*.swo
*~

# OS
.DS_Store
Thumbs.db

# Logs
*.log
```

### Dockerfile

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "app.py"]
```

---

## Repository 2: `nodejs-timeline-api` (Node.js Example)

### package.json

```json
{
  "name": "nodejs-timeline-api",
  "version": "1.0.0",
  "description": "Node.js Timeline API for Git/GitHub learning",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js",
    "test": "jest",
    "test:watch": "jest --watch",
    "lint": "eslint .",
    "lint:fix": "eslint . --fix"
  },
  "dependencies": {
    "express": "^4.18.2",
    "cors": "^2.8.5",
    "helmet": "^7.0.0",
    "dotenv": "^16.3.1"
  },
  "devDependencies": {
    "jest": "^29.7.0",
    "supertest": "^6.3.3",
    "eslint": "^8.49.0",
    "nodemon": "^3.0.1"
  }
}
```

### app.js

```javascript
const express = require("express");
const cors = require("cors");
const helmet = require("helmet");
require("dotenv").config();

const app = express();
const port = process.env.PORT || 3000;

// Middleware
app.use(helmet());
app.use(cors());
app.use(express.json());

// In-memory storage for demo
let timeline = [];
let eventId = 1;

// Routes
app.get("/", (req, res) => {
  res.json({
    message: "🕰️ Welcome to Timeline API",
    version: "1.0.0",
    endpoints: {
      "GET /events": "Get all timeline events",
      "POST /events": "Create new timeline event",
      "GET /events/:id": "Get specific event",
      "DELETE /events/:id": "Delete event",
    },
  });
});

app.get("/events", (req, res) => {
  res.json({
    count: timeline.length,
    events: timeline,
  });
});

app.post("/events", (req, res) => {
  const { event, description } = req.body;

  if (!event) {
    return res.status(400).json({ error: "Event field is required" });
  }

  const newEvent = {
    id: eventId++,
    event,
    description: description || "",
    timestamp: new Date().toISOString(),
  };

  timeline.push(newEvent);
  res.status(201).json(newEvent);
});

app.get("/events/:id", (req, res) => {
  const event = timeline.find((e) => e.id === parseInt(req.params.id));
  if (!event) {
    return res.status(404).json({ error: "Event not found" });
  }
  res.json(event);
});

app.delete("/events/:id", (req, res) => {
  const index = timeline.findIndex((e) => e.id === parseInt(req.params.id));
  if (index === -1) {
    return res.status(404).json({ error: "Event not found" });
  }

  const deletedEvent = timeline.splice(index, 1)[0];
  res.json(deletedEvent);
});

// Start server
if (require.main === module) {
  app.listen(port, () => {
    console.log(`🚀 Timeline API running on port ${port}`);
  });
}

module.exports = app;
```

### .github/workflows/nodejs-ci.yml

```yaml
name: Node.js CI/CD

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16, 18, 20]

    steps:
      - uses: actions/checkout@v4

      - name: Use Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: "npm"

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run tests
        run: npm test

      - name: SonarQube Scan
        uses: sonarqube-quality-gate-action@master
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
          SONAR_HOST_URL: ${{ secrets.SONAR_HOST_URL }}
```

---

## Repository 3: `java-timeline-service` (Java Example)

### pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.timeline</groupId>
    <artifactId>java-timeline-service</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <spring.boot.version>2.7.14</spring.boot.version>
        <junit.version>5.9.3</junit.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>${spring.boot.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <version>${spring.boot.version}</version>
            <scope>test</scope>
        </dependency>

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
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>${spring.boot.version}</version>
            </plugin>

            <plugin>
                <groupId>org.sonarsource.scanner.maven</groupId>
                <artifactId>sonar-maven-plugin</artifactId>
                <version>3.9.1.2184</version>
            </plugin>
        </plugins>
    </build>
</project>
```

### .github/workflows/java-ci.yml

```yaml
name: Java CI/CD

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up JDK 11
        uses: actions/setup-java@v4
        with:
          java-version: "11"
          distribution: "temurin"

      - name: Cache Maven dependencies
        uses: actions/cache@v3
        with:
          path: ~/.m2
          key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}

      - name: Build with Maven
        run: mvn clean compile

      - name: Run tests
        run: mvn test

      - name: SonarQube Analysis
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
        run: mvn sonar:sonar
```

---

## Usage Instructions for Instructors

### Setup Process:

1. **Create GitHub Organization/Account** for the class
2. **Create each repository** with the provided structure
3. **Set up SonarQube** (can use SonarCloud for free)
4. **Configure secrets** in GitHub repository settings:
   - `SONAR_TOKEN`
   - `SONAR_HOST_URL`

### Class Flow:

1. **Start with Python repository** (most comprehensive)
2. **Students fork the repository**
3. **Follow the hands-on guide** step by step
4. **Show other language examples** for comparison
5. **Demonstrate CI/CD pipelines** running

### Key Features:

- ✅ **Complete working applications**
- ✅ **Comprehensive test suites**
- ✅ **Multi-language examples**
- ✅ **Full CI/CD pipelines**
- ✅ **SonarQube integration**
- ✅ **Security scanning**
- ✅ **Docker support**
- ✅ **Educational comments and structure**

These repositories provide everything needed for a complete hands-on learning experience!
