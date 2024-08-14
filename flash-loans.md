# Flash Loans

### Flash Loans

Zeno Lend is bringing Flash Loans to our money markets.

Flash Loans allow developers access to undercollateralized loans, provided that the borrowed amount (and fee) is returned within one transaction block.

Flash Loans offer a wide range of use cases, including democratized liquidations, arbitrage, collateral swapping and interest rate swapping.

1. Using Flash Loans devs **interact with CToken contract,** instead of the lending pool.
2. Flash Loan fee is 0.09%

Our markets comply to EIP-3156 interfaces natively. Users should call flashloan on specific market with the following interface:

```
/**
 * @notice Flash loan funds to a given account.
 * @param receiver The receiver address for the funds
 * @param token The loan currency. Must match the address of this contract's underlying.
 * @param amount The amount of the funds to be loaned
 * @param data The other data
 * @return uint 0=success, otherwise a failure (see ErrorReporter.sol for details)
 */
function flashLoan(
    ERC3156FlashBorrowerInterface receiver,
    address token,
    uint256 amount,
    bytes calldata data
) external returns (bool);
```

Not all markets have Flash Loans. Check out [iToken page](resources/itoken-addresses.md)

