# iToken Introduction

### Introduction <a href="#introduction" id="introduction"></a>

Each asset supported by Zeno Lend is integrated through a iToken contract, which is an [EIP-20](https://eips.ethereum.org/EIPS/eip-20) compliant representation of balances supplied to the protocol. By minting iTokens, users (1) earn interest through the iToken's exchange rate, which increases in value relative to the underlying asset, and (2) gain the ability to use iTokens as collateral.

iTokens are the primary means of interacting with Zeno Lend; when user mints, redeems, borrows, repays a borrow, liquidates a borrow, or transfers iTokens, they will do so using the iToken contract.

Zeno Lend GitHub Organization: [https://github.com/ArtangleVentures](https://github.com/ArtangleVentures)

### Mint <a href="#mint" id="mint"></a>

The mint function transfers an asset into the protocol, which begins accumulating interest based on the current Supply Rate for the asset. The user receives a quantity of iTokens equal to the underlying tokens supplied, divided by the current [Exchange Rate](itoken-introduction.md#exchange-rate)

#### CErc20 <a href="#cerc20" id="cerc20"></a>

```
function mint(uint mintAmount) returns (uint)
```

`msg.sender:` The account which shall supply the asset, and own the minted cTokens.

`mintAmount`: The amount of the asset to be supplied, in units of the underlying asset.

`RETURN:` 0 on success, otherwise an Error code

Before supplying an asset, users must first approve the iToken to access their token balance.

#### Solidity <a href="#solidity" id="solidity"></a>

```
Erc20 underlying = Erc20(0xToken...);     // get a handle for the underlying asset contract
CErc20 iToken = CErc20(0x3FDA...);        // get a handle for the corresponding cToken contract
underlying.approve(address(cToken), 100); // approve the transfer
assert(iToken.mint(100) == 0);            // mint the cTokens and assert there is no error
```

#### Web3 1.0 <a href="#web3-1.0" id="web3-1.0"></a>

```
const underlying = Erc20.at(0xToken...); 
const iToken = CErc20.at(0x3FDB...);
await underlying.methods.approve(cToken, 100).send({from: myAccount});
await iToken.methods.mint(100).send({from: myAccount});
```

### **Redeem** <a href="#redeem" id="redeem"></a>

The redeem function converts a specified quantity of iTokens into the underlying asset, and returns them to the user. The amount of underlying tokens received is equal to the quantity of iTokens redeemed, multiplied by the current [Exchange Rate](itoken-introduction.md#exchange-rate). The amount redeemed must be less than the user's Account Liquidity and the market's available liquidity.

#### CErc20 <a href="#cerc20-1" id="cerc20-1"></a>

```
function redeem(uint redeemTokens) returns (uint)
```

`msg.sender:` The account to which redeemed funds shall be transferred.

`redeemTokens:` The number of iTokens to be redeemed.

`RETURN:` 0 on success, otherwise an Error code

#### **Solidity** <a href="#solidity-1" id="solidity-1"></a>

```
CErc20 iToken = CErc20(0x3FDB...);
require(iToken.redeem(7) == 0, "something went wrong");
```

#### **Web3 1.0** <a href="#web3-1.0-1" id="web3-1.0-1"></a>

```
const iToken = CErc20.at(0x3FDA...);
iToken.methods.redeem(1).send({from: ...});
```

### **Redeem Underlying** <a href="#redeem-underlying" id="redeem-underlying"></a>

The redeem underlying function converts iTokens into a specified quantity of the underlying asset, and returns them to the user. The amount of iTokens redeemed is equal to the quantity of underlying tokens received, divided by the current [Exchange Rate](itoken-introduction.md#exchange-rate). The amount redeemed must be less than the user's Account Liquidity and the market's available liquidity.

#### CErc20 <a href="#cerc20-2" id="cerc20-2"></a>

```
function redeemUnderlying(uint redeemAmount) returns (uint)
```

#### Solidity <a href="#solidity-2" id="solidity-2"></a>

```
CErc20 iToken = CErc20(0x3FDB...);
require(iToken.redeemUnderlying(50) == 0, "something went wrong");
```

#### Web3 1.0 <a href="#web3-1.0-2" id="web3-1.0-2"></a>

```
const iToken = CErc20.at(0x3FDA...);
iToken.methods.redeemUnderlying(10).send({from: ...});
```

### **Borrow** <a href="#borrow" id="borrow"></a>

The borrow function transfers an asset from the protocol to the user, and creates a borrow balance which begins accumulating interest based on the Borrow Rate for the asset. The amount borrowed must be less than the user's Account Liquidity and the market's available liquidity.

To borrow Ether, the borrower must be 'payable' (solidity).

#### **CErc20** <a href="#cerc20-3" id="cerc20-3"></a>

```
function borrow(uint borrowAmount) returns (uint)
```

`msg.sender:` The account to which redeemed funds shall be transferred.

`borrowAmount:` The number of iTokens to be borrowed

`RETURN:` 0 on success, otherwise an Error code

#### Solidity <a href="#solidity-3" id="solidity-3"></a>

```
CErc20 iToken = CErc20(0x3FDB...);
require(iToken.redeemUnderlying(50) == 0, "got collateral?");
```

#### Web3 1.0 <a href="#web3-1.0-3" id="web3-1.0-3"></a>

```
const iToken = CErc20.at(0x3FDA...);
await iToken.methods.borrow(50).send({from: 0xMyAccount});
```

### **Repay Borrow** <a href="#repay-borrow" id="repay-borrow"></a>

The repay function transfers an asset into the protocol, reducing the user's borrow balance.

#### **CErc20** <a href="#cerc20-4" id="cerc20-4"></a>

```
function repayBorrow(uint borrowAmount) returns (uint)
```

`msg.sender:` The account to which borrowed the asset, and shall repay the borrow.

`borrowAmount:` The amount of the underlying borrowed asset to be repaid. A value of -1 (i.e. 2^256 - 1) can be used to repay the full amount.

`RETURN:` 0 on success, otherwise an Error code

Before repaying an asset, users must first approve the iToken to access their token balance.

#### Solidity <a href="#solidity-4" id="solidity-4"></a>

```
CErc20 iToken = CErc20(0x3FDB...);
require(iToken.repayBorrow.value(100() == 0, "transfer approved?");
iToken.methods.repayBorrow(10000).send({from: ...});
```

#### Web3 1.0 <a href="#web3-1.0-4" id="web3-1.0-4"></a>

```
const iToken = CErc20.at(0x3FDA...);
await iToken.methods.borrow(50).send({from: 0xMyAccount});
```

### **Repay Borrow Behalf** <a href="#repay-borrow-behalf" id="repay-borrow-behalf"></a>

The repay function transfers an asset into the protocol, reducing the target user's borrow balance.

#### CErc20 <a href="#cerc20-5" id="cerc20-5"></a>

```
function repayBorrowBehalf(address borrower, uint repayAmount) returns (uint)
```

`msg.sender:` The account to which shall repay the borrow

`borrowAmount:` The account which borrowed the asset to be repaid.

`repayAmount`: The amount of the underlying borrowed asset to be repaid. A value of -1 (i.e. 2^256 - 1) can be used to repay the full amount.

`RETURN:` 0 on success, otherwise an Error code

Before repaying an asset, users must first approve the iToken to access their token balance.

#### **Solidity** <a href="#solidity-5" id="solidity-5"></a>

```
CErc20 iToken = CErc20(0x3FDB...);
require(iToken.repayBorrowBehalf.value(100)(0xBorrower) == 0, "transfer approved?");
```

#### **Web3 1.0** <a href="#web3-1.0-5" id="web3-1.0-5"></a>

```
const iToken = CErc20.at(0x3FDA...);
await iToken.methods.repayBorrowBehalf(0xBorrower, 10000).send({from: 0xPayer});
```

### **Liquidate Borrow** <a href="#liquidate-borrow" id="liquidate-borrow"></a>

A user who has negative account liquidity is subject to liquidation by other users of the protocol to return his/her account liquidity back to positive (i.e. above the collateral requirement). When a liquidation occurs, a liquidator may repay some or all of an outstanding borrow on behalf of a borrower and in return receive a discounted amount of collateral held by the borrower; this discount is defined as the liquidation incentive.

A liquidator may close up to a certain fixed percentage (i.e. close factor) of any individual outstanding borrow of the underwater account. When collateral is seized, the liquidator is transferred iTokens which they may redeem the same as if they had supplied the asset themselves. Users must approve each iToken contract before calling liquidate (i.e. on the borrowed asset which they are repaying), as they are transferring funds into the contract.

#### **CErc20** <a href="#cerc20-6" id="cerc20-6"></a>

```
function liquidateBorrow(address borrower, uint amount, address collateral) returns (uint)
```

`msg.sender:` The account which shall liquidate the borrower by repaying their debt and seizing their collateral.

`borrower:` The account with negative account liquidity that shall be liquidated.

`repayAmount:` The amount of the borrowed asset to be repaid and converted into collateral, specified in units of the underlying borrowed asset.

`cTokenCollateral:` The address of the iToken currently held as collateral by a borrower, that the liquidator shall seize.

`RETURN:` 0 on success, otherwise an Error code

Before supplying an asset, users must first approve the iToken to access their token balance.

#### Solidity <a href="#solidity-6" id="solidity-6"></a>

```
CErc20 iToken = CErc20(0x3FDB...);
CErc20 iTokenCollateral = CErc20(0x3FDA...);
require(iToken.liquidateBorrow(0xBorrower, repayAmount, iTokenCollateral) == 0, "borrower underwater??");
```

#### **Web3 1.0** <a href="#web3-1.0-6" id="web3-1.0-6"></a>

```
const iToken = CErc20.at(0x3FDA...);
const iTokenCollateral = CErc20.at(0x3FDB...);
await iToken.methods.liquidateBorrow(0xBorrower, repayAmount, iTokenCollateral).send({from: 0xLiquidator});
```

### **Exchange Rate** <a href="#exchange-rate" id="exchange-rate"></a>

Each iToken is convertible into an ever increasing quantity of the underlying asset, as interest accrues in the market. The exchange rate between a iToken and the underlying asset is equal to:

```
exchangeRate = (getCash() + totalBorrows() - totalReserves()) / totalSupply()
```

#### CErc20 <a href="#cerc20-7" id="cerc20-7"></a>

```
function exchangeRateCurrent() returns (uint)
```

#### Solidity <a href="#solidity-7" id="solidity-7"></a>

```
CErc20 iToken = CErc20(0x3FDA...);
uint exchangeRateMantissa = iToken.exchangeRateCurrent();
```

#### Web3 1.0 <a href="#web3-1.0-7" id="web3-1.0-7"></a>

```
const iToken = CErc20.at(0x3FDB...);
const exchangeRate = (await iToken.methods.exchangeRateCurrent().call()) / 1e18;
```

Tip: note the use of call vs. send to invoke the function from off-chain without incurring gas costs.

### Get Cash <a href="#get-cash" id="get-cash"></a>

Cash is the amount of underlying balance owned by this iToken contract. One may query the total amount of cash currently available to this market.

#### CErc20 <a href="#cerc20-8" id="cerc20-8"></a>

```
function getCash() returns (unit)
```

`RETURN:` The quantity of underlying asset owned by the contract.

#### Solidity <a href="#solidity-8" id="solidity-8"></a>

```
CErc20 iToken = CErc20(0x3FDA...);
uint cash = iToken.getCash();
```

#### Web3 1.0 <a href="#web3-1.0-8" id="web3-1.0-8"></a>

```
const iToken = CErc20.at(0x3FDB...);
const cash = (await iToken.methods.getCash().call());
```

### &#x20;<a href="#undefined" id="undefined"></a>

### Total Borrow <a href="#total-borrow" id="total-borrow"></a>

Total Borrows is the amount of underlying currently loaned out by the market, and the amount upon which interest is accumulated to suppliers of the market.

#### CErc20 <a href="#cerc20-9" id="cerc20-9"></a>

```
function totalBorrowCurrent() returns (unit)
```

`RETURN:` The total amount of borrowed underlying, with interest.

#### Solidity <a href="#solidity-9" id="solidity-9"></a>

```
CErc20 iToken = CErc20(0x3FDA...);
uint borrows = iToken.totalBorrowsCurrent();
```

#### Web3 1.0 <a href="#web3-1.0-9" id="web3-1.0-9"></a>

```
const iToken = CErc20.at(0x3FDB...);
const borrows = (await iToken.methods.totalBorrowsCurrent().call());
```

### Borrow Balance <a href="#borrow-balance" id="borrow-balance"></a>

A user who borrows assets from the protocol is subject to accumulated interest based on the current borrow rate. Interest is accumulated every block and integrations may use this function to obtain the current value of a user's borrow balance with interest.

#### CErc20 <a href="#cerc20-10" id="cerc20-10"></a>

```
function borrowBalanceCurrent(addressacount) returns (unit)
```

`RETURN:` The total amount of borrowed underlying, with interest.

#### Solidity <a href="#solidity-10" id="solidity-10"></a>

```
CErc20 iToken = CErc20(0x3FDA...);
uint borrows = iToken.borrowBalanceCurrent(msg.caller);
```

#### Web3 1.0 <a href="#web3-1.0-10" id="web3-1.0-10"></a>

```
const iToken = CErc20.at(0x3FDB...);
const borrows = await iToken.methods.totalBorrowsCurrent(account).call();
```

### Borrow Rate <a href="#borrow-rate" id="borrow-rate"></a>

At any point in time one may query the contract to get the current borrow rate per block.

#### CErc20 <a href="#cerc20-11" id="cerc20-11"></a>

```
function borrowRatePerBlock() returns (unit)
```

`RETURN:` The total amount of borrowed underlying, with interest.

#### Solidity <a href="#solidity-11" id="solidity-11"></a>

```
CErc20 iToken = CErc20(0x3FDA...);
uint borrowRateMantissa = iToken.borrowRatePerBlock();
```

#### Web3 1.0 <a href="#web3-1.0-11" id="web3-1.0-11"></a>

```
const iToken = CErc20.at(0x3FDB...);
const borrowRate = (await iToken.methods.borrowRatePerBlock().call());
```

### Total Supply <a href="#total-supply" id="total-supply"></a>

Total Supply is the number of tokens currently in circulation in this iToken market. It is part of the EIP-20 interface of the iToken contract.

#### CErc20 <a href="#cerc20-12" id="cerc20-12"></a>

```
function totalSupply() returns (unit)
```

`RETURN:` The total number of tokens in circulation for the market.

#### Solidity <a href="#solidity-12" id="solidity-12"></a>

```
CErc20 iToken = CErc20(0x3FDA...);
uint tokens = iToken.totalSupply();
```

#### Web3 1.0 <a href="#web3-1.0-12" id="web3-1.0-12"></a>

```
const iToken = CErc20.at(0x3FDB...);
const tokens = (await iToken.methods.totalSupply().call());
```

### Underlying Balance <a href="#underlying-balance" id="underlying-balance"></a>

The user's underlying balance, representing their assets in the protocol, is equal to the user's iToken balance multiplied by the [Exchange Rate](itoken-introduction.md#exchange-rate).

#### CErc20 <a href="#cerc20-13" id="cerc20-13"></a>

```
function balanceOfUnderlying(address account) returns (unit)
```

`account:` The account to get the underlying balance of.

`RETURN:` The amount of underlying currently owned by the account.

#### Solidity <a href="#solidity-13" id="solidity-13"></a>

```
CErc20 iToken = CErc20(0x3FDA...);
uint tokens = iToken.balanceOfUnderlying(msg.caller);
```

#### Web3 1.0 <a href="#web3-1.0-13" id="web3-1.0-13"></a>

```
const iToken = CErc20.at(0x3FDB...);
const tokens = await iToken.methods.balanceOfUnderlying(account).call();
```

### Supply Rate <a href="#supply-rate" id="supply-rate"></a>

At any point in time one may query the contract to get the current supply rate per block. The supply rate is derived from the borrow rate, reserve factor and the amount of total borrows.

#### CErc20 <a href="#cerc20-14" id="cerc20-14"></a>

```
function supplyRatePerBlock() returns (unit)
```

`RETURN:` The current supply rate as an unsigned integer, scaled by 1e18.

#### Solidity <a href="#solidity-14" id="solidity-14"></a>

```
CErc20 iToken = CErc20(0x3FDA...);
uint supplyRateMantissa = iToken.supplyRatePerBlock();
```

#### Web3 1.0 <a href="#web3-1.0-14" id="web3-1.0-14"></a>

```
const cToken = CErc20.at(0x3FDB...);
const supplyRate = (await cToken.methods.supplyRatePerBlock().call()) / 1e18;
```

### Total Reserves <a href="#total-reserves" id="total-reserves"></a>

Reserves are an accounting entry in each iToken contract that represents a portion of historical interest set aside as cash which can be withdrawn or transferred through the protocol's governance. A small portion of borrower interest accrues into the protocol, determined by the reserve factor.

#### CErc20 <a href="#cerc20-15" id="cerc20-15"></a>

```
function totalReserves() returns (unit)
```

`RETURN:` The total amount of reserves held in the market

#### Solidity <a href="#solidity-15" id="solidity-15"></a>

```
CErc20 iToken = CErc20(0x3FDA...);
uint reserves = iToken.totalReserves();
```

#### Web3 1.0 <a href="#web3-1.0-15" id="web3-1.0-15"></a>

```
const iToken = CErc20.at(0x3FDB...);
const reserves = (await iToken.methods.totalReserves().call());
```

### Reserve Factor <a href="#reserve-factor" id="reserve-factor"></a>

The reserve factor defines the portion of borrower interest that is converted into reserves.

#### CErc20 <a href="#cerc20-16" id="cerc20-16"></a>

```
function reserveFactorMantissa() returns (unit)
```

`RETURN:` The current reserve factor as an unsigned itneger, scaled by 1e18.

#### Solidity <a href="#solidity-16" id="solidity-16"></a>

```
CErc20 iToken = CErc20(0x3FDA...);
const reserveFactor = (await cToken.methods.reserveFactorMantissa().call()) / 1e18;
```

#### Web3 1.0 <a href="#web3-1.0-16" id="web3-1.0-16"></a>

```
const iToken = CErc20.at(0x3FDB...);
const reserves = (await iToken.methods.totalReserves().call());
```
