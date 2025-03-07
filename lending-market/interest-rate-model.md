# Interest Rate Model

## APY Function <a href="#apy-function" id="apy-function"></a>

Parameters below are different based on token type (major, stable, governance...... etc).

### **Borrow APY** <a href="#borrow-apy" id="borrow-apy"></a>

\= {1 + \[Base + Multiplier \* min(UtilizationRate, Kink 1) + max(JumpMultiplier \* UtilizationRate - Kink 2, 0)] / BlocksPerYear } ^ BlocksPerYear - 1

### **Supply APY** <a href="#supply-apy" id="supply-apy"></a>

\= Distribute (Interest Paid by Borrowers Per Block - Reserve) to all suppliers, and convert it into APY

\= Distribute \[(1 + Borrow APY) ^ (1 / BlocksPerYear) - 1] \* Total Borrow \* (1 - Reserve Factor) to all suppliers, and convert it into APY

\= {\[(1 + Borrow APY) ^ (1 / BlocksPerYear) - 1] \* Total Borrow \* (1 - Reserve Factor) / Total Supply}, and convert it into APY

\= {1 + \[(1 + Borrow APY) ^ (1 / BlocksPerYear) - 1] \* Total Borrow \* (1 - Reserve Factor) / Total Supply} ^ BlocksPerYear - 1

\= **{1+\[(1+Borrow APY)^(1/BlocksPerYear)-1]\*(1-Reserve Factor)\*Utilization Rate}^BlocksPerYear-1**

BlocksPerYear = 31,536,000 (1 sec per block)



## Major <a href="#major" id="major"></a>

<figure><img src="../.gitbook/assets/major.png" alt=""><figcaption></figcaption></figure>

| Parameter        | Value                                                                                                                                 |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Category         | Major                                                                                                                                 |
| Tokens           | WAPE, APEETH                                                                                                                          |
| Base             | 0%                                                                                                                                    |
| Multiplier       | 15%                                                                                                                                   |
| JumpMultiplier   | 500%                                                                                                                                  |
| Kink 1           | 80%                                                                                                                                   |
| Kink 2           | 90%                                                                                                                                   |
| Contract Address | [0x1A576FEB561E33324dfE3b5644679d58275e554F](https://apechain.calderaexplorer.xyz/address/0x1A576FEB561E33324dfE3b5644679d58275e554F) |

## Stable <a href="#stable" id="stable"></a>

<figure><img src="../.gitbook/assets/stable18 (1).png" alt=""><figcaption></figcaption></figure>

| Parameter        | Value                                                                                                                                                   |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Tokens           | APEUSD                                                                                                                                                  |
| Base             | 0%                                                                                                                                                      |
| Multiplier       | 13%                                                                                                                                                     |
| JumpMultiplier   | 800%                                                                                                                                                    |
| Kink 1           | 80%                                                                                                                                                     |
| Kink 2           | 90%                                                                                                                                                     |
| Contract Address | [0xB1706c6EDA5c0fBD078140EAE977C92947Dd7322](https://apechain.calderaexplorer.xyz/address/0xB1706c6EDA5c0fBD078140EAE977C92947Dd7322?tab=read_contract) |

[\
](https://docs.ib.xyz/v/optimism/lending-market/collateral-factor)
