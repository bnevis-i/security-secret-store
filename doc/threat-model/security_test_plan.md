# Security Test Plan

## Test Cases



| ID                  | SecretStore-VaultMasterKey-001                               |
| ------------------- | ------------------------------------------------------------ |
| **Summary**         | Vault master key shall not be stored unencrypted on the file system. |
| **Pre-condition**   | Malicious container with bind-mounting of host root file system (```HOST```). |
| **Test Case**       | Run a ```find``` on ```HOST/var/lib/docker``` on the host for a ```resp-init.json``` file and parse it for a readable ```keys``` key (array). |
| **Expected Result** | The ```resp-init.json``` file is not found or ```keys``` key is not found. |
| **Post-condition**  | N/A                                                          |



| ID                  | SecretStore-VaultRootToken-001                               |
| ------------------- | ------------------------------------------------------------ |
| **Summary**         | Vault root token shall not be stored unencrypted on the file system. |
| **Pre-condition**   | Malicous container with bind-mounting of root host file system (```HOST```). |
| **Test Case**       | Run a ```find``` on ```HOST/var/lib/docker``` on the host for a ```resp-init.json``` file and parse it for a readable ```root_token``` key. |
| **Expected Result** | The ```resp-init.json``` file is not found or ```keys``` key is not found. |
| **Post-condition**  | N/A                                                          |



| ID                  | SecretStore-VaultVerifyTLS-001                               |
| ------------------- | ------------------------------------------------------------ |
| **Summary**         | Vault TLS certificate should be verified by clients.         |
| **Pre-condition**   | Good container attempting to connect to Vault microservice   |
| **Test Case**       | Query ```/sys/health``` endpoint of Vault microservice with TLS verification enabled. |
| **Expected Result** | ```/sys/health``` endpoint should return a value and generate not TLS errors. |
| **Post-condition**  | N/A                                                          |



| ID                  | SecretStore- |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |



| ID                  | SecretStore_ |
| ------------------- | ------------ |
| **Summary**         |              |
| **Pre-condition**   |              |
| **Test Case**       |              |
| **Expected Result** |              |
| **Post-condition**  |              |
