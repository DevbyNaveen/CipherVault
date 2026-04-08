# CipherVault Research Notes

## Chosen project form

Single-user Java desktop secure file vault.

This beats more complex options like encrypted chat or a multi-user web portal because it still shows strong security concepts while keeping the delivery risk low.

## Security choices

### 1. AES-GCM for file encryption

Why:

- Strong modern authenticated encryption
- Protects confidentiality and integrity together
- Available through standard Java cryptography providers

Decision:

- Use `AES/GCM/NoPadding`
- Generate a fresh IV for every file
- Never reuse an IV with the same key

### 2. PBKDF2 for password-based key derivation

Why:

- Available in standard Java providers
- Good fit for deriving keys from a master password
- Easier to explain in an academic project than bringing in many external crypto libraries

Decision:

- Use `PBKDF2WithHmacSHA256`
- Store a unique random salt
- Use a strong iteration count chosen during implementation and document it clearly

### 3. Local encrypted storage

Why:

- Keeps scope manageable
- Avoids web deployment risk
- Makes the app reusable as a private desktop utility

Decision:

- Store encrypted file blobs in a local vault directory
- Store file metadata and audit logs in SQLite

### 4. Audit logging

Why:

- Makes the system feel more like a real security product
- Gives you a strong demo and report section

Decision:

- Log login attempts, file imports, exports, and deletes

## Important design rules

- Do not store plaintext passwords
- Do not store plaintext copies of vaulted files
- Do not invent custom crypto algorithms
- Do not use insecure modes such as ECB
- Keep security logic in service classes, not mixed into UI code

## Why this has GitHub value

- It is a real use case, not just a toy CRUD app
- Security topics stand out in portfolios
- The project can show architecture, testing, and documentation quality
- Screenshots plus a clean README will make it presentation-ready

## Implementation notes

- Keep the first version single-user
- Keep the UI simple and clean
- Focus on a polished MVP before adding stretch goals
- Add screenshots and a short demo video after the MVP works

## Source notes

The project direction is based on current official or primary references:

- Oracle JDK provider documentation lists `AES GCM NoPadding` and `PBKDF2WithHmacSHA256` support in SunJCE:
  - https://docs.oracle.com/javase/9/security/oracleproviders.htm
- OWASP Password Storage Cheat Sheet is the baseline reference for password storage decisions:
  - https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html
- OpenJFX getting started documentation confirms JavaFX remains a supported path for Maven and desktop app setup:
  - https://openjfx.io/openjfx-docs/
