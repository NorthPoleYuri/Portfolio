| Severity | Title |
| -------- | -------- | 
|M-01 |Not implement ERC4626 properly|

## [M-01] Not implement ERC4626 properly
### Impact
According to EIP-4626, previewMint and previewWithdraw should round up.

In previewMint and previewWithdraw, the currencyAmount is calculated by calling _calculateCurrencyAmount.
```
function _calculateCurrencyAmount(uint128 trancheTokenAmount, address liquidityPool, uint256 price)
        internal
        view
        returns (uint128 currencyAmount)
    {
        (uint8 currencyDecimals, uint8 trancheTokenDecimals) = _getPoolDecimals(liquidityPool);

        uint256 currencyAmountInPriceDecimals = _toPriceDecimals(
            trancheTokenAmount, trancheTokenDecimals, liquidityPool
        ).mulDiv(price, 10 ** PRICE_DECIMALS, MathLib.Rounding.Down);

        currencyAmount = _fromPriceDecimals(currencyAmountInPriceDecimals, currencyDecimals, liquidityPool);
    }

```

The _calculateCurrencyAmount performs calculations using round down, which can result in a lower returnTrancheTokenAmount than expected.

### Proof of Concept
https://github.com/code-423n4/2023-09-centrifuge/blob/main/src/InvestmentManager.sol#L383

https://github.com/code-423n4/2023-09-centrifuge/blob/main/src/InvestmentManager.sol#L396

## Recommended Mitigation Steps
Calculations should take into account the requirements of EIP 4626 and use the correct rounding method.