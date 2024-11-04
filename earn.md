---
hidden: true
---

# Earn

The **Zeno Lend staking rewards** system distributes rewards to iToken stakers in  [Zeno Earn](https://testnet.zeno.finance). Users must stake assets into Zeno Lend staking contracts to earn rewards. Note that staked assets cannot be used as collateral since iTokens will be transferred out of the user’s wallet.

The staking rewards system consists of three components:

1. **StakingRewardsFactory**: Manages the creation of staking reward contracts.
2. **StakingRewards**: The core contract that handles staking and rewards logic.
3. **StakingRewardsHelper**: Assists users with staking, unstaking, and claiming rewards.

#### StakingRewardsFactory

**View Functions:**

* `getAllStakingRewards`: Retrieve all staking reward contracts created by the factory.
* `getStakingRewards`: Get the staking reward contract address for a specific iToken.
* `getStakingToken`: Get the iToken address for a given underlying asset.

**Mutative Functions:**

* `createStakingRewards`: Create staking reward contracts for a list of iToken addresses. Helper contract can be set later.
* `removeStakingRewards`: Remove a staking reward contract from the factory record (does not stop rewards distribution).

#### StakingRewards

**View Functions:**

* `totalSupply`: Get the total amount of iTokens staked in the contract.
* `balanceOf`: Get a user’s staked iToken balance.
* `earned`: Get the claimable reward amount for a user.
* `getRewardRate`: Retrieve the reward rate for a reward token.
* `getRewardForDuration`: Get the total reward amount for one epoch.
* `getAllRewardsTokens`: Get all reward token addresses.
* `getStakingToken`: Get the iToken address for the staking contract.

**Mutative Functions:**

* `stake`: Stake iTokens into the staking reward contract.
* `stakeFor`: Stake iTokens on behalf of a user.
* `withdraw`: Withdraw staked iTokens from the contract.
* `withdrawFor`: Helper contract withdraws iTokens on behalf of users.
* `getReward`: Claim rewards for the user.
* `getRewardFor`: Helper contract claims rewards for users.
* `exit`: Withdraw all iTokens and claim all rewards.
* `notifyRewardAmount`: Admin function to set the reward amount for the next epoch.
* `setRewardsDuration`: Admin function to update the reward duration.
* `addRewardsToken`: Admin function to support new reward tokens.
* `setHelperContract`: Admin function to set the helper contract.

#### StakingRewardsHelper

**View Functions:**

* `getRewardTokenInfo`: Retrieve reward token information.
* `getUserClaimableRewards`: Get claimable rewards for a user.
* `getUserStaked`: Get a user’s staked iToken info.
* `getStakingInfo`: Retrieve all staking-related info.

**Mutative Functions:**

* `stake`: Stake assets into Zeno Lend and the staking reward contract for users.
* `unstake`: Unstake iTokens and redeem the underlying assets for users.
* `exit`: Exit a staking reward contract for users.
* `exitAll`: Exit all staking reward contracts for users.
* `claimRewards`: Claim staking rewards for users.
* `claimAllRewards`: Claim all rewards across contracts for users.
