# CipherVault

CipherVault is a Java desktop security project that stores personal files in an encrypted local vault. It is designed to be presentation-ready for a course submission while still being practical enough to reuse as a portfolio project on GitHub.

## What it does

- Creates a master-password protected vault on first launch
- Uses `PBKDF2WithHmacSHA256` to derive password hashes and encryption keys
- Encrypts every imported file with `AES/GCM/NoPadding`
- Stores encrypted file blobs locally and metadata in `SQLite`
- Records an audit trail for setup, login attempts, imports, exports, and deletes
- Offers a JavaFX dashboard for day-to-day use

## Tech stack

- Java 21
- JavaFX
- SQLite JDBC
- Maven
- JUnit 5

## Project structure

```text
CipherVault/
  docs/
  src/
    main/
      java/
      resources/
    test/
      java/
  pom.xml
```

## How to run

### 1. Install prerequisites

If Java 21 and Maven are not already installed:

```bash
brew install openjdk@21 maven
```

### 2. Start the app

```bash
cd /Users/naveen/Desktop/112233/CipherVault
export PATH="/opt/homebrew/opt/openjdk@21/bin:/opt/homebrew/bin:$PATH"
export JAVA_HOME="/opt/homebrew/opt/openjdk@21/libexec/openjdk.jdk/Contents/Home"
mvn javafx:run
```

### 3. Run tests

```bash
export PATH="/opt/homebrew/opt/openjdk@21/bin:/opt/homebrew/bin:$PATH"
export JAVA_HOME="/opt/homebrew/opt/openjdk@21/libexec/openjdk.jdk/Contents/Home"
mvn test
```

## Where data is stored

CipherVault stores runtime data in:

```text
~/.ciphervault/
```

That folder contains:

- `ciphervault.db` for SQLite metadata and audit entries
- `vault/` for encrypted file blobs

## Why this has GitHub value

- It shows real security concepts, not just basic CRUD
- It is easy to demo with screenshots and a short video
- It includes architecture, persistence, cryptography, UI, and tests
- It can be extended later into backup, restore, tagging, or multi-user versions

## Docs

- [Project Plan](/Users/naveen/Desktop/112233/CipherVault/docs/PROJECT_PLAN.md)
- [Research Notes](/Users/naveen/Desktop/112233/CipherVault/docs/RESEARCH_NOTES.md)
