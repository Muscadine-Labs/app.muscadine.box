**To Work on Today:**

**To work on another day:**
- **Deposit gate — use on-chain RPC whitelist (optional):** Today the app uses a config-only depositor allowlist (gate UI always active) and never calls `sendAssetsGate` / `canSendAssets` over RPC. Lets have it so it fast loads with our config, while chekcing onchain so it than checks if the current address is onchain, than if it is it uses it. So, there is less time between loading. 
- use morpho sdks or onchain for forced deallocation for both the underlying vaults and wrapper vaults. So users on the fee wrapper can withdraw if not enough idle liquidity
- 1. accrue W and C, read fresh state
2. instant = W.idle + C.idle + C_liqAdapterCapacity
3. if assets <= instant:
       [W.withdraw(assets, receiver, user)]
   else:
       shortfall = assets - instant
       pick sources on C, greedily, largest first:
         pullable = min(adapter.expectedSupplyAssets(mId),
                        mkt.totalSupplyAssets - mkt.totalBorrowAssets)
       calls = [C.forceDeallocate(adapter_i, data_i, amt_i, user) ...]
             + [W.withdraw(assets, receiver, user)]
4. bundle all calls in one Bundler3 multicall
5. simulate, then send

**Future (optional):**
- Have multichain for viewing such as with stocks, vaults like robinhood chain. With the actual functions on settings be able to switch the chain. 
- Smart wallet (AA) deposit issue when USDC is used for gas — investigate before changing tx code.
- Stock/token/cash boxes on dashboard. Was deleted because it was unnecessary. If website gains other functions can be useful to have for user experience to see their wallet and to abstract away crypto.
