| Severity | Title |
| -------- | -------- | 
|H-01 |Risk of silent overflow|
|H-02 |Bad Randomness|
|M-01 |Out of gas|

## [H-01] Risk of silent overflow
## Vulnerability details
In the PuppyRaffle.sol selectWinner(),the uint256fee is forcefully cast to uint64. In the 2023-07-PoolTogether contest on C4, there is relevant and valid finding for this issue. You can find it here: https://code4rena.com/reports/2023-07-pooltogether#m-19-silent-overflow-could-alter-computation-when-calculating-the-vaultportion-in-the-prizepool-contract

### Impact
This can potentially result in a silent overflow and may lead to totalFees goes wrong.

### Proof of Concept
https://github.com/Cyfrin/2023-10-Puppy-Raffle/blob/main/src/PuppyRaffle.sol#L133-L134

## Recommended Mitigation Steps
Add checks that the casting value is not greater than the uint64 type max value:
```
if (fee > type(uint64).max) {Overflow();}
```

## [H-02] Bad Randomness
## Vulnerability details
All transactions on the Ethereum blockchain are public and queryable. When writing contracts using random numbers, failure to consider this feature may lead to vulnerabilities that malicious users can exploit for their advantage. The 'winnerIndex' is generated by calculating keccak256 hash using msg.sender, block.timestamp, and block.difficulty. You can refer to [DASP TOP's documentation](https://dasp.co/#item-6) for more details about bad Randomness.

### Impact
The winner can be predicted and controlled.

### Proof of Concept
https://github.com/Cyfrin/2023-10-Puppy-Raffle/blob/main/src/PuppyRaffle.sol#L128

## Recommended Mitigation Steps
You might consider using [Chainlink VRF](https://docs.chain.link/vrf#overview). Chainlink VRF (Verifiable Random Function) is a provably fair and verifiable random number generator (RNG) that enables smart contracts to access random values without compromising security or usability. For each request, Chainlink VRF generates one or more random values and cryptographic proof of how those values were determined. The proof is published and verified on-chain before any consuming applications can use it. This process ensures that results cannot be tampered with or manipulated by any single entity including oracle operators, miners, users, or smart contract developers.

## [M-01] Out of gas
## Vulnerability details
enterRaffle() traverse all registered participants and search for duplicate addresses. Whenever a new participant enter the raffle, it will be compared with each existing participant in the array to ensure there are no duplicates. As the number of participants increases, the gas consumption can become more expensive each time a new collateral address is added to the array.

### Impact
Potentially leading to an 'Out of Gas' error or reaching the 'Block Gas Limit' in the worst-case scenario.

### Proof of Concept
https://github.com/Cyfrin/2023-10-Puppy-Raffle/blob/main/src/PuppyRaffle.sol#L101

## Recommended Mitigation Steps
Refactor the code using a more efficient method