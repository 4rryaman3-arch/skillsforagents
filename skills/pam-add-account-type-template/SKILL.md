---
name: pam-as400-remote-credentials
description: Implement AS400 account template remote-credential UX in PAM with Select2 account selection, reset/verify support, and no remote connection launch behavior.
---

# PAM AS400 Remote Credentials

Use this skill when adding or fixing AS400 template behavior for:
- Reset + Verify password support
- Credentials for Remote Operations modal
- AS400-only Select2 filtering
- No launch/open remote connection actions

## Workflow

1. Confirm account type/template constants:
   - `initial_entries/accounttypes.py`
   - `accountmanagement/utils/accounttypeconstants.py`
2. Wire AS400 template to a dedicated remote-config component in:
   - `configuration/configuration_constants.py` under `ASSET_TABS -> ASSET_TEMPLATE_TABS -> remote-credentials-conf`
3. Use a dedicated Ember component/template pair for AS400 remote credentials (do not inherit `unix-devices`).
4. Ensure account Select2 passes `template_names=["AS400ApplicationTemplate"]`.
5. Ensure backend endpoints used by modal are allowed by constraints:
   - `/accountmanagement/get_remote_configured_account`
   - `/accountmanagement/get_unix_auth_accounts`
   - `/accountmanagement/configure_device_auth_account`
6. Keep remote-credential mapping enabled for AS400 (`is_remote_cred_map: true`) but keep launch/open connection hidden in account details UI.
7. Keep AS400 port handling template-specific (no SSH default-port auto population logic).

## Required Checks

- `configuration/get_devices_tabs` returns `remote-credentials-conf` for AS400 type.
- Modal renders Select2 controls (not blank body).
- Select2 API returns only AS400 template accounts.
- Saving remote credentials persists and reloads correctly.
- Open/Launch remote connection UI actions are not shown for AS400 template.

## Validation

- Run `python -m py_compile` for edited Django files.
- If Ember files changed, rebuild/restart frontend and hard refresh browser.
- If modal is blank, inspect network responses for:
  - `/accountmanagement/get_remote_configured_account`
  - `/accountmanagement/get_unix_auth_accounts`
