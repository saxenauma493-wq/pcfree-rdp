# Free Windows RDP — GitHub Actions + Tailscale

Public repo. Standard Windows runner is free on public repos. Each run gives a fresh Windows PC for up to 6 hours (350 minutes).

## One-time setup

1. Create a free Tailscale account: https://login.tailscale.com/
2. Create an auth key: https://login.tailscale.com/admin/settings/keys
   - Reusable, expiry as long as allowed.
3. Add repo secrets:

```bash
gh secret set TAILSCALE_AUTHKEY -R saxenauma493-wq/pcfree-rdp
gh secret set RDP_PASSWORD -R saxenauma493-wq/pcfree-rdp
gh secret set PHONE_IP -R saxenauma493-wq/pcfree-rdp
```

- `TAILSCALE_AUTHKEY`: key from Tailscale admin.
- `RDP_PASSWORD`: any password you choose for the RDP user.
- `PHONE_IP`: phone's Tailscale IP (from the Tailscale app) — used for the `Z:` drive.

Phone must have Tailscale app installed and logged into the same account. Phone SFTP server must be running on port 2222.

## Start a session

```bash
gh workflow run rdp.yml -R saxenauma493-wq/pcfree-rdp
gh run watch -R saxenauma493-wq/pcfree-rdp
```

Open the run log. Find `TAILSCALE_IP=x.x.x.x`.

## Connect

Microsoft Remote Desktop app:
- PC: `<TAILSCALE_IP>:3389`
- User: `RDP`
- Password: the `RDP_PASSWORD` secret

Phone files appear as `Z:\` if `PHONE_IP` is set. Installers for Office/Tally/Brave/Claude are not included — add them in the workflow or install manually after connect.

## Stop early

```bash
gh run list -R saxenauma493-wq/pcfree-rdp --limit 1
gh run cancel <run-id> -R saxenauma493-wq/pcfree-rdp
```

Runner is fresh every run. Save work only to `Z:\` (phone) — `C:\` is wiped when the run ends.
