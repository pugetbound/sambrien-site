# sambrien.com — Setup Guide

## What this is

A Quarto website configured as a professional blog + portfolio, deploying automatically to GitHub Pages at sambrien.com. Every push to `main` triggers a rebuild and deploy via GitHub Actions.

---

## Step 1: Install Quarto

**Mac:**
```bash
brew install quarto
```
Or download the installer from [quarto.org/docs/get-started](https://quarto.org/docs/get-started/)

**Windows:** Download the `.msi` installer from quarto.org/docs/get-started/

**Verify:**
```bash
quarto --version
```

---

## Step 2: Install the VS Code Extension (Recommended)

In VS Code, install the **Quarto** extension (Publisher: quarto.org). This gives you:
- Live preview in the editor
- Syntax highlighting for `.qmd` files
- Render on save

---

## Step 3: Create and Configure the GitHub Repo

1. Go to [github.com/new](https://github.com/new)
2. Name the repo `sambrien.com` (or `sambrien-site` — doesn't matter)
3. Set to **Public** (required for free GitHub Pages)
4. Do NOT initialize with a README — you'll push the template files directly

---

## Step 4: Clone and Add Your Files

```bash
# Clone the empty repo
git clone https://github.com/YOUR_USERNAME/sambrien.com.git
cd sambrien.com

# Copy all the template files into this directory
# (drag the contents of the downloaded template into the folder,
#  or copy manually — everything at the same level as _quarto.yml)
```

Your directory should look like:
```
sambrien.com/
├── _quarto.yml
├── index.qmd
├── about.qmd
├── styles.css
├── CNAME
├── .gitignore
├── blog/
│   ├── index.qmd
│   └── posts/
│       └── 2026-06-04-getting-started/
│           └── index.qmd
├── portfolio/
│   ├── index.qmd
│   └── seattle-ferry-finder/
│       └── index.qmd
└── .github/
    └── workflows/
        └── publish.yml
```

---

## Step 5: Update _quarto.yml with Your Details

Open `_quarto.yml` and replace:
- `YOUR_GITHUB_USERNAME` with your actual GitHub username
- `YOUR_LINKEDIN` with your LinkedIn profile path
- `YOUR_TWITTER_HANDLE` if you have one (or delete those lines)

---

## Step 6: Preview Locally

```bash
quarto preview
```

This opens a browser at `localhost:4444` (or similar) with live reload. Every time you save a file, the preview updates.

---

## Step 7: Enable GitHub Pages

1. Push your files to GitHub:
```bash
git add .
git commit -m "initial site"
git push origin main
```

2. GitHub Actions will automatically run (check the **Actions** tab in your repo to watch it)

3. Once the action completes, go to your repo **Settings → Pages**:
   - Source: **Deploy from a branch**
   - Branch: **gh-pages** · **/ (root)**
   - Click **Save**

4. GitHub will show a URL like `https://YOUR_USERNAME.github.io/sambrien.com` — this is temporary, the custom domain replaces it.

---

## Step 8: Connect sambrien.com

### At your domain registrar (wherever you bought sambrien.com):

Add these **A records** pointing to GitHub's Pages servers:
```
Type: A    Name: @    Value: 185.199.108.153
Type: A    Name: @    Value: 185.199.109.153
Type: A    Name: @    Value: 185.199.110.153
Type: A    Name: @    Value: 185.199.111.153
```

Add this **CNAME record** for www:
```
Type: CNAME    Name: www    Value: YOUR_USERNAME.github.io
```

### In GitHub (Settings → Pages):
- Under "Custom domain", enter `sambrien.com`
- Click Save
- Check "Enforce HTTPS" once it's available (usually a few minutes after DNS propagates)

DNS propagation takes anywhere from 10 minutes to 48 hours. You can check status at [dnschecker.org](https://dnschecker.org).

---

## Day-to-Day Workflow

### Writing a new blog post:
```bash
# Create a new post directory
mkdir blog/posts/2026-06-15-my-post-title
# Create the post file
touch blog/posts/2026-06-15-my-post-title/index.qmd
```

Start every post with this frontmatter:
```yaml
---
title: "Your Post Title"
description: "One sentence that appears in the listing."
date: 2026-06-15
categories: [forecasting, workforce planning]
draft: false
---
```

### Adding a portfolio project:
```bash
mkdir portfolio/my-project-name
touch portfolio/my-project-name/index.qmd
```

Start every portfolio item with:
```yaml
---
title: "Project Name"
description: "What it does in one sentence."
date: 2026-06-15
categories: [Python, AWS, forecasting]
image: thumbnail.png   # optional: add a screenshot
---
```

### Publishing:
```bash
git add .
git commit -m "add post: my post title"
git push origin main
```

That's it. GitHub Actions handles the rest. Site is updated within ~2 minutes.

---

## Embedding Charts and Code

Quarto can execute Python code blocks and embed the output — charts, tables, anything. Example:

````markdown
```{python}
#| echo: false
import plotly.express as px
import pandas as pd

df = pd.DataFrame({"week": range(1,13), "headcount": [820,835,841,...]})
fig = px.line(df, x="week", y="headcount", title="Headcount Trend")
fig.show()
```
````

The chart renders inline in the published page. This is the feature that makes Quarto worth using over a plain Markdown blog — your analysis and your writing live in the same document.

---

## Themes

If you want to change the visual theme, edit `_quarto.yml`:
```yaml
format:
  html:
    theme: flatly   # or: cosmo, litera, lux, minty, sandstone
```

Preview at [bootswatch.com](https://bootswatch.com) to see options.
