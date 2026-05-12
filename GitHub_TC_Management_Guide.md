# GitHub Test Case Management — QA Team Guide

**Author:** Harish M  
**Date:** 2026-05-12  
**Purpose:** End-to-end process for managing test cases, sprints, and test cycles using GitHub

---

## Overview

This guide covers how to manage the entire QA test cycle using GitHub — from writing test cases to tracking execution status — without any paid tools like TestRail or Zephyr.

### Mapping QA Concepts to GitHub

| QA Concept       | GitHub Feature       |
|------------------|----------------------|
| Test Case        | Markdown file (.md)  |
| Sprint           | Milestone            |
| Test Cycle       | Project (Board)      |
| TC Execution     | Issue (one per TC)   |
| Pass/Fail/Blocked| Labels               |

---

## Step 1 — Write Test Cases as Markdown Files

Create one `.md` file per module. Example structure:

```markdown
# TC_LOGIN — Login Module Test Cases

## Overview
| Field        | Value              |
|--------------|--------------------|
| Module       | Authentication     |
| Author       | Harish M           |
| Last Updated | 2026-05-12         |
| App          | KeeboHealth Mobile |

---

## TC_LOGIN_001 — Successful login with valid credentials

**Priority:** High  
**Type:** Positive

**Preconditions:**
- App is installed and launched
- Valid registered account exists

**Steps:**
1. Launch the app
2. Enter valid email
3. Enter correct password
4. Tap Login

**Expected Result:**  
User is navigated to Home dashboard.

---

## Test Run Log

| TC ID        | Date | Build | Result | Tester | Notes |
|--------------|------|-------|--------|--------|-------|
| TC_LOGIN_001 |      |       |        |        |       |
```

**Naming convention:** `TC_<ModuleName>.md`  
**Location:** Store all TC files in a dedicated GitHub repo.

---

## Step 2 — Push TC Files to GitHub

```bash
# One-time setup — navigate to your TC folder
cd "TC hands on"

# Initialize git and connect to repo
git init
git remote add origin git@github.com:<your-username>/<repo-name>.git

# Add, commit and push
git add TC_Login.md
git commit -m "Add Login module test cases"
git branch -M main
git push -u origin main
```

> If the remote repo already has content, pull first:
> ```bash
> git pull origin main --allow-unrelated-histories
> git push -u origin main
> ```

---

## Step 3 — Install GitHub CLI (gh)

The `gh` CLI lets you manage milestones, labels, issues, and projects from the terminal — much faster than doing it manually on the website.

**Install via conda (recommended, no sudo needed):**
```bash
conda install -c conda-forge gh -y
```

**Authenticate:**
```bash
gh auth login
```
Choose: **GitHub.com → SSH → Select your SSH key → Login with browser**

**Verify:**
```bash
gh auth status
```

---

## Step 4 — Create Sprint (Milestone)

A Milestone represents your sprint scope — which TCs are planned for this sprint.

**Via GitHub website:**
1. Go to your repo → **Issues** tab → **Milestones**
2. Click **New milestone**
3. Title: `Sprint 1`, add due date if needed
4. Click **Create milestone**

**Via gh CLI:**
```bash
gh api repos/<username>/<repo>/milestones \
  -f title="Sprint 1" \
  -f due_on="2026-05-30T00:00:00Z"
```

---

## Step 5 — Create Labels

Labels track execution status of each TC.

**Via GitHub website:**  
Go to `github.com/<username>/<repo>/labels` → **New label**

| Label       | Color   | Meaning              |
|-------------|---------|----------------------|
| In Progress | #cfd3d7 | Currently executing  |
| Pass        | #0e8a16 | TC passed            |
| Fail        | #b60205 | TC failed            |
| Blocked     | #e4e669 | TC blocked           |

**Via gh CLI:**
```bash
gh label create "In Progress" --color "cfd3d7" --repo <username>/<repo>
gh label create "Pass"        --color "0e8a16" --repo <username>/<repo>
gh label create "Fail"        --color "b60205" --repo <username>/<repo>
gh label create "Blocked"     --color "e4e669" --repo <username>/<repo>
```

---

## Step 6 — Create Issues (TC Execution Trackers)

### Why both .md files and Issues?

| | `.md` file | Issue |
|---|---|---|
| Purpose | Test case document | Execution tracker |
| Contains | Steps, expected result | Pass/Fail status, comments |
| Updated when | TC is added or changed | TC is executed |
| Analogy | Test script | Test result sheet |

The `.md` file is the **single source of truth** for TC content. The issue tracks **execution status only** — it links back to the `.md` file so there is no duplication.

### Create issues with a direct link to the TC

**Via gh CLI:**
```bash
gh issue create \
  --repo <username>/<repo> \
  --title "TC_LOGIN_001 — Successful login with valid credentials" \
  --body "**Test Case:** [TC_LOGIN_001 — Successful login with valid credentials](https://github.com/<username>/<repo>/blob/main/TC_Login.md#tc_login_001--successful-login-with-valid-credentials)" \
  --label "In Progress" \
  --milestone "Sprint 1"

gh issue create \
  --repo <username>/<repo> \
  --title "TC_LOGIN_002 — Login with invalid password" \
  --body "**Test Case:** [TC_LOGIN_002 — Login with invalid password](https://github.com/<username>/<repo>/blob/main/TC_Login.md#tc_login_002--login-with-invalid-password)" \
  --label "In Progress" \
  --milestone "Sprint 1"
```

> Opening the issue and clicking the link takes you directly to the full TC with steps and expected result in the `.md` file.

### Update issue body with direct link (if already created)

```bash
gh issue edit <issue-number> --repo <username>/<repo> \
  --body "**Test Case:** [TC_LOGIN_001 — Successful login with valid credentials](https://github.com/<username>/<repo>/blob/main/TC_Login.md#tc_login_001--successful-login-with-valid-credentials)"
```

---

## Step 7 — Create Project Board (Test Cycle)

The Project board is your test cycle execution view — visual Kanban showing status of each TC.

**Grant project permissions first (one-time):**
```bash
gh auth refresh -s project,read:project
```

**Create the project (one project for all sprints):**
```bash
gh project create --owner <username> --title "KeeboHealth — QA Test Cycles"
```

> Use one project board across all sprints. Filter by milestone on the board to see Sprint 1 or Sprint 2 separately. No need to create a new board every sprint.

**Add issues to the board:**
```bash
gh project item-add 1 --owner <username> --url https://github.com/<username>/<repo>/issues/1
gh project item-add 1 --owner <username> --url https://github.com/<username>/<repo>/issues/2
```

**Board columns to set up:**
- To Do
- In Progress
- Pass
- Fail
- Blocked

---

## Step 8 — Update TC Status After Execution

When you execute a TC, update both the label and move the card on the board.

**Update label via gh CLI:**
```bash
# Mark as Pass
gh issue edit <issue-number> --repo <username>/<repo> \
  --remove-label "In Progress" --add-label "Pass"

# Mark as Fail
gh issue edit <issue-number> --repo <username>/<repo> \
  --remove-label "In Progress" --add-label "Fail"
```

**Close issue when TC is done:**
```bash
gh issue close <issue-number> --repo <username>/<repo>
```

---

## Milestone vs Project — Key Difference

| | Milestone | Project Board |
|---|---|---|
| QA equivalent | Sprint scope / Test Plan | Test Cycle execution |
| Question answered | "What TCs are in Sprint 1?" | "Which TCs passed/failed?" |
| Tracks | Completion % | Execution status |
| Scope | Single repo | Multiple repos |

**In short:** Milestone = scope. Project = status.

---

## Folder Structure (Recommended)

```
QA-TestCases/
├── TC_Login.md
├── TC_BloodPressure.md
├── TC_ECGUpload.md
└── GitHub_TC_Management_Guide.md
```

---

## Quick Reference — Common gh Commands

```bash
# List all issues
gh issue list --repo <username>/<repo>

# List milestones
gh api repos/<username>/<repo>/milestones

# List labels
gh label list --repo <username>/<repo>

# View project board
gh project list --owner <username>

# View specific issue
gh issue view <issue-number> --repo <username>/<repo>
```

---

*Document maintained by QA Team — Tricog Health*
