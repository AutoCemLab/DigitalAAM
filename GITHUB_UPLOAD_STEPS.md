# Publish DigitalAAM v0.1 with GitHub Pages

These steps let you publish the site without giving anyone access to your GitHub account.

## 1. Create a repository

1. Sign in to GitHub.
2. Click **New repository**.
3. Suggested repository name: `digitalaam`.
4. Choose **Public** for the simplest free GitHub Pages setup.
5. Create the repository.

## 2. Upload the prepared files

Upload the contents of the clean GitHub Pages folder, not the folder itself.

Required files:

- `index.html`
- `README.md`
- `.nojekyll`
- `assets/digitalaam-banner.png`
- `strength_predictions.js`
- `strength_predictions_binder_350.js`
- `strength_predictions_binder_550.js`

Do not upload:

- `__pycache__/`
- `live_gpr_server.py`
- `start_live_gpr_server.cmd`
- `start_live_gpr_server_hidden.cmd`

## 3. Enable GitHub Pages

1. Open the repository on GitHub.
2. Go to **Settings**.
3. Go to **Pages**.
4. Under **Build and deployment**, choose:
   - Source: **Deploy from a branch**
   - Branch: **main**
   - Folder: **/root**
5. Click **Save**.

GitHub will show the public URL after it finishes publishing. It will usually look like:

`https://YOUR-USERNAME.github.io/digitalaam/`

## 4. Link from WordPress

On your WordPress page, add a button or text link:

Button text: `Open DigitalAAM Tool`

Link target:

`https://YOUR-USERNAME.github.io/digitalaam/`

## 5. Optional custom domain

If you later want a nicer URL, set up a subdomain such as:

`digitalaam.yourdomain.com`

This requires adding the custom domain in GitHub Pages settings and adding a DNS `CNAME` record at your domain provider.
