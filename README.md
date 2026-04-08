# CipherVault

A local file vault for your desktop. CipherVault lets you protect sensitive files behind a master password — everything stored on disk is encrypted, nothing is sent anywhere.

---

## Features

- **Master password setup** — on first launch you create a password; all subsequent access requires it
- **Strong key derivation** — uses `PBKDF2WithHmacSHA256` with a random salt to turn your password into an encryption key
- **AES-GCM encryption** — every file you import is encrypted with `AES/GCM/NoPadding` before it touches disk
- **Local SQLite storage** — file metadata and audit records live in a single database file on your machine
- **Audit log** — every login attempt, import, export, and deletion is recorded with a timestamp
- **JavaFX dashboard** — a clean desktop UI for managing your vault day to day

---

## Tech stack

| Layer | Technology |
|---|---|
| Language | Java 21 |
| UI | JavaFX 21 |
| Database | SQLite via `sqlite-jdbc` |
| Build | Maven |
| Testing | JUnit 5 |

---

## Project structure

```
CipherVault/
├── docs/                          # Additional documentation
├── src/
│   ├── main/
│   │   ├── java/com/ciphervault/
│   │   │   ├── app/               # Application entry point (Launcher, CipherVaultApp)
│   │   │   ├── config/            # Path configuration (AppPaths)
│   │   │   ├── db/                # Database layer (DatabaseManager, repositories)
│   │   │   ├── model/             # Data models (UserRecord, VaultFileRecord, AuditEntry)
│   │   │   ├── security/          # Crypto & password logic (CryptoService, PasswordService)
│   │   │   └── service/           # Application service layer (AppService)
│   │   └── resources/
│   └── test/
│       └── java/com/ciphervault/security/  # Unit tests
└── pom.xml
```

---

## Getting started

### Prerequisites

You need **Java 21** and **Maven**. On macOS with Homebrew:

```bash
brew install openjdk@21 maven
```

### Run the app

```bash
# Set up Java 21 if it's not your default
export JAVA_HOME="$(brew --prefix openjdk@21)/libexec/openjdk.jdk/Contents/Home"
export PATH="$JAVA_HOME/bin:$PATH"

# Launch
mvn javafx:run
```

### Run the tests

```bash
export JAVA_HOME="$(brew --prefix openjdk@21)/libexec/openjdk.jdk/Contents/Home"
export PATH="$JAVA_HOME/bin:$PATH"

mvn test
```

---

## Where your data lives

CipherVault keeps everything under your home directory:

```
~/.ciphervault/
├── ciphervault.db     ← SQLite database (metadata + audit log)
└── vault/             ← Encrypted file blobs
```

Nothing is written anywhere else and nothing leaves your machine.

---

## How the security works

1. **On setup** — your master password is hashed with `PBKDF2WithHmacSHA256` (random salt, 310 000 iterations) and stored. The same password is used to derive the AES key.
2. **On login** — the entered password is re-derived and compared against the stored hash. Wrong password means no access.
3. **On import** — the file is read into memory, encrypted with AES-256-GCM (random IV), and written to `~/.ciphervault/vault/`. Only the ciphertext ever touches disk.
4. **On export** — the encrypted blob is decrypted in memory and written to your chosen destination.

---

## Documentation

- [Project Plan](docs/PROJECT_PLAN.md)
- [Research Notes](docs/RESEARCH_NOTES.md)
