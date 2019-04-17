# Task List

## Prioritized Plan of Action

### Phase 1

1. Develop test infrastructure for framework bringup

   a. Docker-based bringup

   b. Snap-based bringup

2. Support enhanced PKI initialization flow

   a. Write test cases to fail PKI practices that will be changed.

   b. Move PKI generation to standalone container

   c. Generate PKI in scratch area

   d. Add option to shred of CA keying material after PKI generation

   e. Add hooks to customize deployment location of PKI artifacts including public CA certificates and private TLS keys for bootstrap services.

   f. Implement PKI caching option

   g. Implement option to pre-load PKI from an off-device CA

3. Block startup of core services until PKI is available.

4. Remove TLS skip-verify overrides from client services.

5. Revoke previously generated tokens on every reboot.

6. Generate per-service tokens at system startup.

7. Revoke Vault root token.

8. Implement Vault cubbyhole response-wrapping.

9. Implement Vault secrets client library (integrate with registration service client library?)

### Phase 2
1. Generate unique-per-installation PGP key pair.
2. Derive PGP passphrase with an HMAC-KDF using hardware fingerprint as IKM and random salt.
3. Pass PKI and Vault token secrets via tmpfs volumes.
4. Revoke CA and intermediates after creating leaf certificates.
5. Token issuance driven by service registration.
6. Automated revocation of Vault tokens for failed services.
7. Self-token-rotation (token issuing service).
