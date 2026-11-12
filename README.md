# clone-lin-aave-liq

Experimental **LIN** clone of Aave V3 `LiquidationLogic._calculateAvailableCollateralToLiquidate`
(collateral vs debt pricing, close factor, liquidation bonus, protocol fee).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [aave/aave-v3-core](https://github.com/aave/aave-v3-core) `782f51917056a53a2c228701058a6c3fb233684a`
  - `LiquidationLogic.sol` sha256 `4fecfabd4ebbeccba176f41b3417dd5b3f16d65100d9e9cd373b7ed0dec51d5a` blob `57f00fd47d14af2643dd8103a47adb70b664ec00`
  - `PercentageMath.sol` sha256 `6f165ad32ddcc3383cbb611cc3045c1a4bb28f16f83a85015fe202803de0161f` blob `c000be3a6c38a959aa3d28737445eaa5762c53b0`
- **License of the algorithm:** MIT after BUSL Change Date 2023-01-27. This clone is original LIN/C11.
- **Class:** EXPERIMENTAL uint64-scale, `WAD' = 1e9`, `PERCENTAGE_FACTOR = 1e4` (mainnet `WAD = 1e18` HF threshold fits i64 and is recorded).

## Files

- `src/lin_aave_liq.lin` — LIN kernel (`aliq_avail(dp, debt, cu, cp, du, user_bal, bonus, fee_pct, want)`)
- `test/oracles/aave_liq_c11.c` — independent C11 `__int128` XOR limb oracle (not TCB)

## Reproduce proofs (in lin-open)

```bash
make aave-liq-proof
# or: python3 test/prove_aave_liq_external.py
```

Named goldens: equal-price 5% bonus **1050000000**; 10% protocol fee of that bonus
**1045000000 / fee 5000000**; capped user balance `5e8` → debtNeeded **476190476**;
`percentMul(1, 0.5e4)` half-up **1** vs floor **0**.

## Not claimed

Uint256 / WAD=1e18 / oracle / aToken mainnet parity. LIN replacing Aave or the EVM.
TVL or dollar savings. zk soundness.
