# 🚀 CI/CD Cheat Sheet (GitHub Actions)

This README contains a **complete, clean, and structured version** of the entire discussion.

---

# 📌 1. Events (Triggers)

**Definition:**
Events define **when the CI/CD pipeline runs**.

**Common events:**

* push → when code is pushed
* pull_request → when PR is created/updated

```yaml
on:
  push:
  pull_request:
```

---

# 📌 2. Actions

**Definition:**
Reusable units of work (pre-built or custom tasks).

**Example:**

```yaml
- name: Checkout code
  uses: actions/checkout@v4
```

---

# 📌 3. Runners

**Definition:**
Machines (VMs) where your workflow runs.

**Example:**

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

---

# 📌 4. Artifacts

**Definition:**
Files saved after execution (models, logs, builds).

**Example:**

```yaml
- name: Upload artifact
  uses: actions/upload-artifact@v4
  with:
    name: model
    path: model.pkl
```

---

# 📌 5. Cache

**Definition:**
Stores dependencies to **speed up pipeline execution**.

**Example:**

```yaml
- name: Cache pip
  uses: actions/cache@v4
  with:
    path: ~/.cache/pip
    key: pip-${{ hashFiles('requirements.txt') }}
```

---

# 📌 6. Bandit (Security Scan)

**Definition:**
Tool to detect **security vulnerabilities in Python code**.

**Example:**

```yaml
- name: Run Bandit
  run: pip install bandit && bandit -r .
```

---

# 📌 7. Linting

## 🔹 Flake8 (Code Quality)

```yaml
- name: Flake8
  run: pip install flake8 && flake8 .
```

## 🔹 Black (Code Formatting)

```yaml
- name: Black
  run: pip install black && black --check .
```

---

# 🔥 Full CI Pipeline Example

```yaml
name: CI Pipeline

on:
  push:
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.10"

      - name: Cache dependencies
        uses: actions/cache@v4
        with:
          path: ~/.cache/pip
          key: pip-${{ hashFiles('requirements.txt') }}

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Lint (Flake8)
        run: flake8 .

      - name: Format check (Black)
        run: black --check .

      - name: Security (Bandit)
        run: bandit -r .

      - name: Save artifact
        uses: actions/upload-artifact@v4
        with:
          name: output
          path: .
```

---

# 💡 Quick Summary

| Concept   | Meaning                    |
| --------- | -------------------------- |
| Events    | When pipeline runs         |
| Actions   | Tasks performed            |
| Runners   | Machine executing pipeline |
| Artifacts | Saved outputs              |
| Cache     | Speed optimization         |
| Bandit    | Security scan              |
| Flake8    | Code quality               |
| Black     | Code formatting            |

---

# ✅ One-Line Understanding

* Events → trigger pipeline
* Actions → tasks
* Runners → execution machine
* Artifacts → saved files
* Cache → faster builds
* Bandit → security
* Flake8 → linting
* Black → formatting

---

# 📦 Usage

1. Create `.github/workflows/ci.yml`
2. Paste the full CI example
3. Push code to GitHub
4. Pipeline runs automatically 🚀

---

# 🎯 End Goal

Automate:

* Code quality checks
* Security scanning
* Dependency handling
* Build artifact storage

---

**You can directly download and use this README for your GitHub project.**
