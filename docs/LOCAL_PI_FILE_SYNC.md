# Maintainer: Local File Sync / Repair Workflow

This document is for **maintainer-only** recovery/repair work when files were created or edited locally and need to be pushed to the Pi.

It is intentionally separate from user-facing operations docs.

## Sync local repo files to Pi

### Bash / Linux / macOS shell

Run from your local repo root:

```bash
rsync -av --delete \
  --exclude '.git' \
  --exclude '.venv' \
  --exclude 'data' \
  --exclude '.env' \
  ./ pi@<PI_HOST>:/opt/fpv-board/
```

### PowerShell (Windows)

Use PowerShell backticks (`` ` ``) for line continuation (not backslashes):

```powershell
rsync -av --delete `
  --exclude '.git' `
  --exclude '.venv' `
  --exclude 'data' `
  --exclude '.env' `
  ./ pi@<PI_HOST>:/opt/fpv-board/
```

If you prefer, use this single line in PowerShell:

```powershell
rsync -av --delete --exclude '.git' --exclude '.venv' --exclude 'data' --exclude '.env' ./ pi@<PI_HOST>:/opt/fpv-board/
```

## Reinstall requirements + restart timers on Pi

```bash
ssh pi@<PI_HOST> 'cd /opt/fpv-board && . .venv/bin/activate && pip install -r requirements.txt && sudo systemctl daemon-reload && sudo systemctl restart fpv-board.timer weekly-report.timer yearly-log-reset.timer'
```

## Optional: one-liner (sync + restart)

```bash
rsync -av --delete --exclude '.git' --exclude '.venv' --exclude 'data' --exclude '.env' ./ pi@<PI_HOST>:/opt/fpv-board/ && ssh pi@<PI_HOST> 'cd /opt/fpv-board && . .venv/bin/activate && pip install -r requirements.txt && sudo systemctl daemon-reload && sudo systemctl restart fpv-board.timer weekly-report.timer yearly-log-reset.timer'
```
