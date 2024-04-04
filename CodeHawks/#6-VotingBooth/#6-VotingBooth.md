| Severity | Title |
| -------- | -------- | 
|H-01 |If the proposal passed, the reward for users voting in favor is less than expected.|


## [H-01] If the proposal passed, the reward for users voting in favor is less than expected
## Vulnerability details
Reward calculation does not match expectations.

### Impact
According to the comments, rewards are distributed to the 'For' voters because the proposal passed. However, when calculating rewardPerVoter, it is divided by totalVotes, resulting in users who voted in favor receiving less reward than expected.

### Proof of Concept
https://github.com/Cyfrin/2023-12-Voting-Booth/blob/main/src/VotingBooth.sol#L192

## Recommended Mitigation Steps
```
uint256 rewardPerVoter = totalRewards / totalVotesFor;
```
