# CipherVault Project Plan

## Project summary

Build a Java desktop application that lets a user protect sensitive files in a local encrypted vault.

## Problem statement

Normal local folders store files in readable form. If a device is shared, lost, or inspected, sensitive files can be exposed. CipherVault solves this by encrypting files before storage and protecting access with a master password.

## Why this is the right course project

- It demonstrates real security concepts instead of only CRUD screens
- It stays small enough to complete within a course timeline
- It gives a clean demo story: log in, upload, encrypt, view, decrypt, audit
- It has obvious GitHub value because the purpose is practical and portfolio-friendly

## Target version

Single-user local desktop application.

This is the best tradeoff between impressive and manageable. It avoids the extra risk of web hosting, session management, and network attack surfaces while still showing strong security engineering decisions.

## Main modules

### 1. Authentication module

- First-run master password setup
- Login screen
- Password verification using a derived hash and stored salt
- Failed-login counter

### 2. Cryptography module

- Generate a unique random IV for each file
- Derive keys from the master password using PBKDF2
- Encrypt files using AES-GCM
- Decrypt files only after successful authentication

### 3. Vault storage module

- Store encrypted file data on disk
- Store metadata in SQLite
- Keep file name, size, created time, and vault path

### 4. Audit log module

- Record login success and failure
- Record upload, decrypt/export, and delete actions
- Show recent activity in the UI

### 5. Desktop UI module

- Login view
- Vault dashboard
- Upload/import flow
- File list table
- Export/decrypt flow

## Recommended tech stack

- Language: Java 21
- UI: JavaFX
- Database: SQLite
- Build tool: Maven
- Crypto: Java Cryptography Architecture
- Tests: JUnit 5

## Proposed folder structure

```text
CipherVault/
  docs/
    PROJECT_PLAN.md
    RESEARCH_NOTES.md
  src/
    main/
      java/
      resources/
    test/
      java/
```

## Core data design

### `app_user`

- `id`
- `password_hash`
- `password_salt`
- `created_at`
- `failed_attempts`
- `locked_until`

### `vault_file`

- `id`
- `original_name`
- `stored_name`
- `mime_type`
- `size_bytes`
- `iv`
- `created_at`

### `audit_log`

- `id`
- `event_type`
- `details`
- `created_at`

## MVP delivery plan

### Phase 1: Setup and skeleton

- Create Maven project
- Add JavaFX and SQLite dependencies
- Create app shell and navigation

### Phase 2: Security foundation

- First-run password setup
- Password hashing and verification
- AES-GCM encrypt/decrypt service

### Phase 3: Vault features

- Import file
- Encrypt and save
- List vault entries
- Export decrypted copy

### Phase 4: Polish and submission

- Audit log screen
- Error handling
- Basic tests
- README, screenshots, and presentation notes

## Minimum success criteria

- A user can set a master password
- A user can log in after restarting the app
- A file is stored only in encrypted form
- The app can decrypt and export the file correctly
- Important actions appear in the audit log

## Stretch goals if time allows

- Backup and restore
- Dark and light themes
- Category labels
- Drag-and-drop import
- Password strength meter

## Demo script

1. Launch app and log in
2. Import a sample PDF or image
3. Show that the stored vault file is unreadable
4. Open the vault list and inspect metadata
5. Export a decrypted copy
6. Show audit log entries

## Out of scope

- Cloud storage
- Team accounts
- Live collaboration
- Custom cryptographic algorithms
- Public file sharing links
