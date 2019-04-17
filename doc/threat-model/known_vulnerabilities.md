# Known Vulnerabilities

There are a number of known vulnerabilities with the secret management solution as currently implemented in the Edinburgh pre-release.

### PKI private key compromise

The PKI root CA and private certificate for Vault are published in /vault/pki in the EdgeX vault container image published on Docker Hub. This vulnerability allows an attacker to generate their own server certificates to masquerade as any EdgeX service. This vulnerability also allows an attacker to eavesdrop on the data connection between a service and Vault and expose both the vault token and any application secrets transmitted along that channel. Moreover, private keys for other services are written to persistent docker volumes, where they can be harvested at rest, or through undiscovered path traversal or remote execution vulnerabilities

### Vault master key and root token exposure

The existing vault-worker service stores the vault master key and root token unencrypted in a persistent docker volume. This allows the entire contents of the vault to be decrypted simply by making a copy of the bulk storage media and analyzing it offline. Moreover, an attacker could compromise the integrity and confidentiality of the vault by having a non-expiring access token that can bypass all security controls in the Vault API.

### Vault eavesdropping attack

Although the reference implementation implements secure service location, existing code turns off TLS verification when talking to Vault, potentially allowing for MITM attacks against Vault.

### Vault token distribution infrastructure missing

There is currently no defined way to issue and distribute vault access tokens to services. At the present time all services would have to use the vault root token to retrieve secrets, with the consequence that every service can read every other service's secrets.