# Task List

## Prioritized Plan of Action
1. Develop test infrastructure for framework bring-up
   a. Docker-based bringup
   b. Snap-based bringup
2. Support enhanced PKI initialization flow
   a. Write test cases to fail PKI practices that will be changed.
   b. Move PKI generation to standalone container
   c. Generate PKI in scratch area
   d. Add option to shred of CA keying material after PKI generation
   e. Add hooks to customize deployment location of PKI artifacts including public CA certificates and private TLS keys for bootstrap services
   f. Implement PKI caching option
   g. Implement option to pre-load PKI from an off-device CA
   h. Block startup of core services until PKI is available
   i. Remove TLS skip-verify overrides from client services
3. Security enhancement to Vault provisoning
   a. Create `SALT_SCRIPT` and hook and test cases
   b. Create `IKM_SCRIPT` and hook and test cases
   c. Create `KDF_SCRIPT` (software SHA-256) and hook and test cases
   d. Parameterize Vault master key encryption algorithm with test cases
   e. Implement AES-256-GCM encryption of Vault master key with test cases
   f. Create token-issuing-token in tmpfs area (e.g. SCRATCH/service/token-issuing/vault-token) where for containers SCRATCH is /run and for snaps SCRATCH is /run/snap.edgexfoundry.$snapid
   g. Revoke root token by default and regenerate on-the-fly as needed
   h. Continually monitor Vault health and automatically unseal Vault when it is unsealed with test cases
4. Generate per-service tokens
   a. Implement `file-server` daemon that bulk-generates tokens and places them in configurable mailbox location
   b. Implement `unix-server-docker` daemon that issues tokens on-demand after authenticating peer PID against Docker
   c. Start `${TOKEN_SERVER}` daemon process after Vault is unsealed (assuming)
   d. Automatically restart `TOKEN_SERVER` if it fails
   e. Rename vault-worker to security-service
5. Enhancements to go-mod-core-security
   a. Implement `FILE_TOKEN_HANDLER` to retrieve Vault token from specified file path.
   b. Implement `UNIX_TOKEN_HANDLER` to retrieve Vault token from Unix domain socket
   c. Accept `${TOKEN_URL}` parameter that points to location of Vault token
6. Implement integration tests
   a. `file://` token handler
   b. `unix://` token handler
7. Revoke previously generated tokens on every reboot
8. Self-token-rotation (token issuing service)
