| Severity | Title |
| -------- | -------- | 
|H-01 |Lack of access control|
|H-02 |Can arbitrarily burn someone else's SantaTokens to buy presents|

## [H-01] Lack of access control
## Vulnerability details
Lack of access control. checkList() can only be called by Santa but without any restrictions.

### Impact
Everyone can call checkList() to put themselves on the nice list.

### Proof of Concept
https://github.com/Cyfrin/2023-11-Santas-List/blob/main/src/SantasList.sol#L121

## Recommended Mitigation Steps
add ```onlySanta``` modifier

## [H-02] Can arbitrarily burn someone else's SantaTokens to buy presents

## Vulnerability details
There is no check in buyPresent() to verify if presentReceiver is the same as msg.sender. This can be exploited to burn someone else's SantaTokens and mint NFTs for oneself.

### Impact
Attacker can burn the balance of everyone and mint a large number of NFT for themselves.

### Proof of Concept
https://github.com/Cyfrin/2023-11-Santas-List/blob/main/src/SantasList.sol#L172

## Recommended Mitigation Steps
add below check
```
require(presentReceiver == msg.sender) 
```