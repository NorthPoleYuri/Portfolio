| Severity | Title |
| -------- | -------- | 
|H-01 |Malicious users can see other user's passwords.|
|H-02 |Lack of access control|

## [H-01] Malicious users can see other user's passwords.
## Vulnerability details
On the blockchain, all transactions are public and queryable. In Solidity, private merely signifies that a function can only be invoked within the contract's internal context.
### Impact
Malicious users can see other people's passwords, violating the restriction in getPassword() that only allows the owner to view the password.

### Proof of Concept
https://github.com/Cyfrin/2023-10-PasswordStore/blob/main/src/PasswordStore.sol#L14

## Recommended Mitigation Steps
Smart contracts in public blockchains have no built-in mechanism to store secret data securely. It is important to protect sensitive data from reading by an untrusted actor. You can explore more about ["V3: Blockchain Data"](https://github.com/securing/SCSVS/blob/master/1.2/0x12-V3-Blockchain-Data.md) in the SCSVS key areas.

## [H-02] Malicious users can see other user's passwords.
## Vulnerability details
In PasswordStore.sol, the setPassword() function allows only the owner to set a new password, but it lacks any permission control.

### Impact
A malicious user can call the function arbitrarily to change the password.

### Proof of Concept
https://github.com/Cyfrin/2023-10-PasswordStore/blob/main/src/PasswordStore.sol#L26

## Recommended Mitigation Steps
check only owner can set a new password
```
if (msg.sender != s_owner) {
    revert PasswordStore__NotOwner();
      }
```