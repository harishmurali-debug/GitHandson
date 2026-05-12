# TC_BP — Blood Pressure Module Test Cases

## Overview

| Field        | Value              |
|--------------|--------------------|
| Module       | Blood Pressure     |
| Author       | Harish M           |
| Last Updated | 2026-05-12         |
| App          | KeeboHealth Mobile |

---

## TC_BP_001 — Successful BP entry with valid values

**Priority:** High  
**Type:** Positive

**Preconditions:**
- User is logged in to the KeeboHealth app
- BP entry screen is accessible from the dashboard

**Steps:**
1. Navigate to the BP entry screen
2. Enter a valid systolic value (e.g. 120)
3. Enter a valid diastolic value (e.g. 80)
4. Enter a valid pulse value (e.g. 72)
5. Tap the **Save** button

**Expected Result:**  
BP reading is saved successfully and displayed in the BP history/dashboard.

---

## TC_BP_002 — BP entry with out-of-range values

**Priority:** High  
**Type:** Negative

**Preconditions:**
- User is logged in to the KeeboHealth app
- BP entry screen is accessible

**Steps:**
1. Navigate to the BP entry screen
2. Enter an invalid systolic value (e.g. 999)
3. Enter a valid diastolic value (e.g. 80)
4. Enter a valid pulse value (e.g. 72)
5. Tap the **Save** button

**Expected Result:**  
Validation error is displayed indicating the systolic value is out of acceptable range. Reading is not saved.

---

## Test Run Log

| TC ID   | Date | Build/Version | Result | Tester   | Notes |
|---------|------|---------------|--------|----------|-------|
| TC_BP_001 |    |               |        | Harish M |       |
| TC_BP_002 |    |               |        | Harish M |       |
