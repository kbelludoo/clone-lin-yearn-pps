# clone-lin-yearn-pps

Experimental LIN clone of Yearn V2 `pricePerShare` / `_calculateLockedProfit` (i64-positive scale: `DEGRADATION_COEFFICIENT=1e18`, default degradation `46e12`, 128-bit floor `(a*b)/d`, fail-closed if the quotient does not fit i64).

- **Upstream:** [yearn/yearn-vaults](https://github.com/yearn/yearn-vaults) `contracts/Vault.vy` v0.4.6 commit `97ca1b2e4fcf20f4be0ff456dabd020bfeb6697b` (AGPL-3.0-or-later). This repository does **not** vendor the Vyper source. Hashes only: sha256 `bc36fcf88e8662a7e1bd5f9c8213645214697dd72afda4e0cd87bfaeb0c4a19b`, git blob `1159912c093b12cf8d758f71d2d568cda69990af`.
- **LIN kernel:** [`src/lin_yearn_pps.lin`](src/lin_yearn_pps.lin) (sha256 `6bc6c126ce1b372cdbee88d011c32151705c18fbd6375047c5a48c622a20a076`, identical to lin-open)
- **C11 oracle copy:** [`test/oracles/yearn_pps_c11.c`](test/oracles/yearn_pps_c11.c)
- **Proofs (lin-open only):** [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open) `examples/yearn_pps/` and `python3 test/prove_yearn_pps_external.py` (23 PASS / 0 FAIL / 1 SKIP JIT this run)
- **Class:** EXPERIMENTAL. Not Ethereum Yearn yVault uint256. Not ERC-4626 virtual shares. Not a production vault replacement.

Named goldens (`decimals=18`, idle=1e18, locked=0.1e18, supply=1e18): empty PPS → **1000000000000000000**; t0 harvest → **900000000000000000** vs Lido-style ignore-lock **1000000000000000000**; 3h linear → **949680000000000000** vs step-unlock **900000000000000000**; 6h unlocked → **1000000000000000000**; `now < lastReport` fail-closed **0**; empty mint 1:1; naive i64 `7e18*3e18` wraps to **-5389669316318724096**.
