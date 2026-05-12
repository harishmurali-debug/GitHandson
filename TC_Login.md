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
- KeeboHealth app is installed and launched on device
- A valid registered patient account exists

**Steps:**
1. Launch the KeeboHealth app
2. Enter a valid registered email in the username field
3. Enter the correct password in the password field
4. Tap the **Login** button

**Expected Result:**  
User is successfully authenticated and navigated to the Home dashboard screen.

---

## TC_LOGIN_002 — Login with invalid password

**Priority:** High  
**Type:** Negative

**Preconditions:**
- KeeboHealth app is launched
- A valid registered account exists

**Steps:**
1. Enter a valid registered email in the username field
2. Enter an incorrect password in the password field
3. Tap the **Login** button

**Expected Result:**  
An error message is displayed indicating invalid credentials. User remains on the login screen and is not authenticated.

---

## Test Run Log

| TC ID        | Date       | Build/Version | Result | Tester   | Notes |
|--------------|------------|---------------|--------|----------|-------|
| TC_LOGIN_001 |            |               |        | Harish M |       |
| TC_LOGIN_002 |            |               |        | Harish M |       |
