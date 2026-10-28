# Private Media Upload Portal

The frontend is hosted by GitHub Pages. `worker.js` is a Cloudflare Worker API that protects the shared password and GitHub token server-side.

## Before using the portal

GitHub Pages serves static files only. Deploy `worker.js` to Cloudflare Workers and configure its secrets before uploads can work.

1. Create a **separate private GitHub repository** for temporary client media.
2. Create a fine-grained GitHub token limited to that repository with **Contents: Read and write**. Never place this token in frontend code.
3. Deploy `worker.js` as a Cloudflare Worker.
4. Configure Worker variables/secrets:
   - `ALLOWED_ORIGIN`: `https://upload.goldcrownmakeupstudio.com`
   - `GITHUB_OWNER`: GitHub owner name
   - `GITHUB_REPO`: private storage repository name
   - `SHARED_PASSWORD`: choose a strong shared password (secret)
   - `SESSION_SECRET`: a long random secret (secret)
   - `GITHUB_TOKEN`: the fine-grained token (secret)
5. Bind the Worker to a URL you control, such as `https://api-upload.goldcrownmakeupstudio.com`, or use its Workers URL.
6. In `index.html`, replace `https://REPLACE-WITH-YOUR-UPLOAD-API` with that Worker base URL (no trailing slash), commit, and redeploy Pages.
7. Confirm the GitHub Pages custom domain and HTTPS are working.

## Limits and privacy

- Demo limit: 20 MB per file. GitHub has per-file and repository limits; this is for small temporary uploads, not large video hosting.
- Files are committed to the private storage repository and do **not** expire automatically. Delete them manually or configure a scheduled cleanup job.
- Anyone with the shared password can upload. Rotate it if it is shared too widely.
- Keep the storage repository private. Never commit the password or token.
- The frontend's login is intentionally disabled until the API URL is configured.
- For frequent or large videos, use dedicated object storage instead of GitHub.
