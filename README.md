# ResumeCraft — Free Resume Builder

A responsive, client-side resume builder built with React, Vite, jsPDF, and docx. No backend, account, API key, or paid service is required.

## Features

- 10 resume designs: Classic Professional, Modern, Minimal, Student & Fresher, Technical, Corporate, Compact One-page, Elegant, Creative Simple, and ATS Friendly.
- Education entries for Indian school, vocational, diploma, undergraduate, postgraduate, and other qualifications (including 10th, 12th, ITI, Diploma, B.Sc., B.Com., B.A., B.Tech., M.Tech., MBA).
- Editable personal details, objective, skills, education, experience, internships, projects, certifications, languages, and interests.
- Live preview, template switching, example data, browser-local autosave, and validation.
- PDF, DOCX, and plain-text TXT exports.
- Share text using the browser's native share sheet when available, otherwise copy to clipboard where supported.
- Mobile-friendly layout and keyboard-visible focus styles.

## Requirements

- Node.js 20.19+ or 22.12+ recommended (current supported LTS).
- Git and a GitHub account for Codespaces / publishing.

## Run in GitHub Codespaces

1. Create a new GitHub repository named `free-resume-builder`.
2. Upload or commit all files from this project into the repository root.
3. Open the repository on GitHub.
4. Click **Code → Codespaces → Create codespace on main**.
5. In the Codespaces terminal, run:

```bash
npm install
npm run dev
```

6. When Codespaces offers to open port 5173, click **Open in Browser**. If no pop-up appears, open the **Ports** tab, find port `5173`, and click its forwarded address.
7. Click **Try sample** to see a filled resume. Edit the fields and test the PDF, DOCX, and TXT buttons.

The app saves draft details in localStorage in that browser. Use **Reset** to clear the saved draft. Avoid using sample details in a real application submission.

## Run locally

Install Node.js first, then from the project directory:

```bash
npm install
npm run dev
```

Open the local URL printed by Vite (normally `http://localhost:5173`).

## Publish free with GitHub Pages

### 1. Put the source in a GitHub repository

The repository name should be `free-resume-builder` if you use the default Vite Pages base path in `vite.config.js`. If you choose another repository name, change `/free-resume-builder/` in that file to `/<your-repository-name>/`.

### 2. Add the GitHub Pages workflow

Create `.github/workflows/deploy.yml` with this content:

```yaml
name: Deploy ResumeCraft to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 22
      - name: Install dependencies
        run: npm install
      - name: Build site
        run: npm run build
        env:
          GITHUB_PAGES: "true"
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: dist

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### 3. Enable Pages

1. In your repository, open **Settings → Pages**.
2. Under **Build and deployment**, choose **GitHub Actions** as the source.
3. Commit and push the workflow file.
4. Open **Actions** and wait for the deployment workflow to turn green.
5. Visit `https://YOUR-GITHUB-USERNAME.github.io/free-resume-builder/`.

GitHub Pages is suitable for this static client-side app. There is no server or secret to configure.

## Alternative: Netlify or Cloudflare Pages

Build with `npm run build`, then deploy the generated `dist/` folder using the host's free static-site option. No environment variables are needed for a standard root-domain deployment. For GitHub Pages, keep the base path in `vite.config.js` correct.

## Troubleshooting

- **`npm` command not found:** install Node.js LTS or recreate the Codespace using a current Node image.
- **Page is blank after deploying:** verify the repository name matches the Vite `base` path, then check the Actions build log.
- **Export doesn't start:** allow downloads in your browser and retry. Refresh the page if a dependency fails to load.
- **Email validation error:** use a normal email format or leave the email field blank.
- **Draft missing on another device:** drafts are stored locally per browser and are not synced. Export a file to back it up.
- **Sharing isn't available:** use the TXT export and attach it to a message.
- **PDF layout:** PDF export prioritizes readable text and ATS-friendly structure rather than reproducing every visual template pixel-for-pixel.

## Privacy and limitations

The resume data is stored in browser localStorage on the device. Do not use a shared/public computer for sensitive personal details; click Reset and clear browser data when finished. Hosting the static app does not upload resume data to a database. The page loads Google Fonts from a public font CDN; remove the `@import` at the top of `src/styles.css` if you prefer no external font request.

All dependencies are open-source and the app has no paid API dependency. Check dependency and hosting terms before using the app in a commercial product.
