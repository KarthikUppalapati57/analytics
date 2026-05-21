# CW-CHOTO Analytics - Vercel + GitHub Auto Sync Setup

This setup lets your dashboard run on Vercel and auto-update from Excel without your laptop running.

## Files Added
- `index.html` - dashboard UI (auto-fetches `data.json`)
- `scripts/build_data_json.py` - converts Excel to `data.json`
- `.github/workflows/sync-data.yml` - scheduled GitHub Action (every 15 min)
- `data.json` - generated dashboard data

## Step 1: Upload to GitHub
1. Create a new GitHub repo.
2. Upload this folder contents.

## Step 2: Set OneDrive Source (optional but recommended)
If your Excel in GitHub is not always latest, use OneDrive as source.

1. In OneDrive, share the Excel file as a public/read link.
2. In GitHub repo settings:
   - `Settings` -> `Secrets and variables` -> `Actions` -> `New repository secret`
   - Name: `ONEDRIVE_XLSX_URL`
   - Value: direct-download URL of the Excel file

If you skip this secret, workflow uses the Excel file committed in GitHub.

## Step 3: Enable Auto Sync
- Workflow file: `.github/workflows/sync-data.yml`
- It runs every 15 minutes and on manual trigger.
- It regenerates `data.json` and commits only when data changes.

Manual run:
1. GitHub -> `Actions`
2. `Sync Dashboard Data`
3. `Run workflow`

## Step 4: Deploy to Vercel
1. In Vercel, import your GitHub repo.
2. Framework: `Other` (static site)
3. Build command: leave empty
4. Output directory: leave empty
5. Deploy.

## How Live Updates Work on Vercel
- Dashboard fetches `./data.json` on load and every 60 seconds.
- GitHub Action updates `data.json` on schedule.
- Vercel redeploys when GitHub commit occurs.

## Notes
- This is near real-time (interval-based), not instant per-cell push.
- To speed updates, change cron in `sync-data.yml` (for example, every 5 minutes).
- Keep Excel structure same as current template.
