# Barbary Coast Marine — Deployment & Migration Plan

## What this folder contains
- `index.html` (one-page site)
- `styles.css`
- `script.js`
- `app-spec.yaml` (DigitalOcean App Platform template)

## 1) Put this site in a GitHub repo
DigitalOcean App Platform is easiest/reliable from GitHub.

Example:
```bash
cd sites/barbarycoastmarine-onepage
git init
git add .
git commit -m "Barbary Coast Marine one-page site"
# create repo on GitHub first, then:
git remote add origin git@github.com:YOUR_GITHUB_ORG/YOUR_REPO.git
git branch -M main
git push -u origin main
```

## 2) Create App Platform app
In DigitalOcean control panel:
1. Create App
2. Choose GitHub repo + main branch
3. App type: **Static Site**
4. Source directory: `/`
5. Build command: *(leave blank)*
6. Output directory: `/`
7. Set index: `index.html`
8. Set error doc: `index.html`
9. Deploy

## 3) Add custom domain
1. In App Platform, add your domain (e.g., `barbarycoastmarine.com`)
2. Update DNS at your registrar:
   - CNAME `www` -> DO app default hostname
   - A/ALIAS root -> as instructed by DigitalOcean
3. Wait for SSL certificate provisioning (automatic)

## 4) Safe migration from droplet (near-zero downtime)
1. Lower DNS TTL for current records to 60s (do this before cutover)
2. Deploy App Platform site and validate on the temporary `ondigitalocean.app` URL
3. Confirm content, links, and contact actions
4. Switch DNS records from droplet to App Platform
5. Monitor traffic for 24h
6. Keep droplet serving old site for fallback during DNS propagation
7. Decommission droplet web service after verification

## 5) Optional rollback
If any issue appears after cutover:
- Re-point DNS back to droplet records
- Fix app and redeploy
- Retry cutover

## Notes
- If you currently rely on server-side forms on the droplet, replace with a hosted form endpoint before migration.
- Current contact form uses `mailto:` fallback; easiest zero-backend option for now.
