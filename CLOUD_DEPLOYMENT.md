# Mandi Purchase Tracker — Cloud / HTTP deployment

This build is prepared for Render and keeps the existing Flask + SQLite architecture.

## What was changed
- Gunicorn production server (`gunicorn app:app`)
- `PORT` environment variable support
- `/health` HTTP health-check endpoint
- `SECRET_KEY` environment variable support
- `MANDI_DATA_DIR` support so SQLite databases, uploads, settings and generated reports can live on a persistent disk
- `.python-version` set to Python 3.13
- `render.yaml` Blueprint included
- `.gitignore` prevents local databases/uploads from being committed accidentally
- PWA/HTTPS files from the mobile build are retained

## Render deployment (recommended)
1. Create a GitHub repository and upload this project folder.
2. In Render choose **New → Blueprint** and connect the repository. Render supports `runtime: python`, health checks, and persistent disks in Blueprint configuration.
3. Render will read `render.yaml`.
4. The service uses:
   - Build: `pip install -r requirements.txt`
   - Start: `gunicorn app:app`
   - Health: `/health`
   - Persistent data: `/var/data`
5. After the first successful deploy, open the generated `https://....onrender.com` URL.

## Existing local data
Do NOT commit `mandi.db`, `profiles.db`, or your `uploads` folder to GitHub.

To move existing data to the cloud:
1. Run the local app.
2. Use the app's **Backup** feature and download the ZIP backup.
3. Deploy the cloud app.
4. Use **Reports → Restore** to restore the active profile database.
5. Repeat with additional profile backups if needed.

The app writes cloud data under `/var/data` because that is the persistent disk mount configured in `render.yaml`.

## Important Render note
Render's normal filesystem is ephemeral. Only files written under the attached persistent disk survive deploys/restarts. Persistent disks are available on paid web services. For a larger multi-user production system, migrating the SQLite databases to Render Postgres is the better long-term architecture.

## Local use remains unchanged
Double-click `start_mandi_tracker.bat` on Windows as before. Without `MANDI_DATA_DIR`, the app continues to store its local data beside `app.py`.
