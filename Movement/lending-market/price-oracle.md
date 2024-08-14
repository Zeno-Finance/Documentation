# Price Oracle

Zeno Lend currently uses the Pyth Network oracle to provide price feeds for all of its active lending markets. The Pyth oracle is responsible for delivering the up-to-date prices for the tokens supported in the Zeno Lend ecosystem.

The way the price updates work is as follows:

1. Zeno Lend is integrated with the Pyth oracle and regularly checks for new price updates from the Pyth network.
2. Whenever the price of a token changes by 1% or more from the previous price, or if 1 hour has passed since the last update, Zeno Lend automatically triggers an update to reflect the new price on the platform.

### Oracle Latency

Using Pyth oracle helps us to avoid price manipulation within a block, but there are still chances that pyth oracle has price difference from the global price, depending on several factors. In some cases, it leads users to borrow more than they are allowed, if the borrow limit is calculated by global price.

However, Zeno Lend is an over-collateral lending protocol that users can borrow assets no more than a certain ratio of collateral value. This is defined by Collateral Factor of each market. Thus, while we keep monitoring significant price difference between pyth oracle and global price, such oracle latency has little effect on the protocol.

### Oracle Fallback <a href="#oracle-fallback" id="oracle-fallback"></a>

We use pyth oracle for all active markets on Zeno Lend by default.

While we believe in pyth providing accurate token price, it is still important to monitor price difference between our oracle and global price. If we were to find pyth oracle price significantly different from global price, we could toggle protocol price oracle from using pyth to other on-chain alternatives with the Guardian.

Only when pyth provides a stale price will we toggle the oracle using the Guardian.

Price Oracle Address  0x28851f391159E191D1EAFe9edA21560D3CF60b13
