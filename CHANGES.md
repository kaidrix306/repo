# Changes made to the VPS Deployer bot

1. **`!manage` → sshx button**: already existed and worked; relabeled to
   `🌐 sshx` exactly as requested (was `🌐 SSHX`). It installs sshx on the
   VPS if missing and DMs the sshx URL to the user.

2. **Removed self-deploy (`!deploy`)**: the free/role-based instant deploy
   command and its role/slot config are gone.

3. **Credits + Coupons**:
   - `!addcoupon <code> <amount>` (admin only) — creates a single-use
     coupon worth `<amount>` credits.
   - `!redeem <code>` — any user redeems a coupon for credits.
   - `!balance` — check your credit balance.
   - New `credits` and `coupons` tables added to the database automatically.

4. **`!plans`**: lists 4GB / 8GB / 12GB / 16GB plans (RAM/CPU/Disk/price)
   plus a "Custom — contact an admin" option. Edit the `PLANS` dict near
   the top of `bot.py` to change specs or prices — the starting prices
   are just a placeholder (100 credits per GB) since none were given.

5. **`!buy <plan>`**: spends credits, lets the user pick an OS, and
   deploys the VPS automatically (reuses the old deploy logic, generalized
   to the chosen plan). Credits are only charged after the VPS is
   successfully created — if deployment fails, nothing is charged.

6. **DMs disabled for all commands**: any command used in a DM now
   replies "Commands can't be used in DMs — please head back to the
   server and run this command there instead!" (the requested sentence,
   lightly polished).

7. **`!setpwd <container> <password>`**: lets a user change the root
   password of a VPS *they own* (not the host machine). Requires the
   container name since a user can own more than one VPS.

8. **`!autofix <container>`**: runs a battery of common fixes (broken
   packages, networking, DNS, SSH restart, disk/log cleanup) on a VPS the
   user owns. If any step can't be confirmed, it DMs the main admin
   automatically as an escalation and tells the user to open a ticket if
   the issue persists.

9. **`requirements.txt`** added with all dependencies actually imported
   by `bot.py` (discord.py, python-dotenv, requests, psutil, PyNaCl).

## Things you may want to adjust
- `PLANS` dict (top of `bot.py`) — specs/prices are placeholders.
- Coupon redemption is currently single-use per code, no expiry.
- `!autofix` runs best-effort shell fixes — it can't detect every
  possible error, so it always escalates to the admin's DMs if a step
  fails, per your instructions.
