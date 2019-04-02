# Remediation Plan

## Prioritized Plan of Action

### Phase 1 MVP
1. Assign unique UID/GID to each microservice.
2. Develop install scripts and systemd services to bring up EdgeX framework on generic Linux platform.
3. Create PKI at runtime that is unique for each boot (remove static PKI).
4. Block startup of core services until PKI is available.
5. Remove TLS skip-verify overrides from client services.
6. Create Vault namespace standard for per-service and shared secrets.
7. Revoke previously generated tokens on every reboot.
8. Generate per-service tokens at system startup.
9. Revoke Vault root token.
10. Implement Vault cubbyhole response-wrapping.
11. Implement Vault secrets client library.
12. Enable MVP for container-based EdgeX.
13. Enable MVP for snap-based EdgeX.

### Phase 2
1. Generate unique-per-installation PGP key pair.
2. Derive PGP passphrase with an HMAC-KDF using hardware fingerprint as IKM and random salt.
3. Pass PKI and Vault token secrets via tmpfs volumes.
4. Revoke CA and intermediates after creating leaf certificates.
5. Token issuance driven by service registration.
6. Automated revocation of Vault tokens for failed services.
7. Self-token-rotation (token issuing service).

### Phase 3

1. TPM hardware secure storage (unauthenticated) for Vault master key.
2. Use TPM persistent handle and NVRAM for Vault master key.
3. Implement additional TPM authentication scenarios (simple PCR, PCR policy, and (HMAC-KDF?) password).
4. TPM-based PKI.
5. Once-per-boot decryption of Vault master key.
6. Token-issuing-token encryption at rest or recovery of token-issuing-token from HW secure storage.

### Phase 4

1. PKCS11 hardware secure storage for Vault master key
2. Implement Mandatory Access Control for EdgeX services.
