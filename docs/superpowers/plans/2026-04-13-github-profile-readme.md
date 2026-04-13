# GitHub Profile README Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rewrite the GitHub profile README to present Chien Chuan Wang as someone at the intersection of lighting design and software engineering, with a rich, structured, emoji-free layout.

**Architecture:** Two deliverables -- a full README.md rewrite (7 sections, linear flow, shields.io badges, github-readme-stats widgets, blog post markers) and a GitHub Action workflow that auto-pulls blog post titles from the site's sitemap.xml (no RSS feed available). The README uses only markdown and inline HTML where needed for badge/stat alignment.

**Tech Stack:** Markdown, shields.io, github-readme-stats (anuraghazra), GitHub Actions, curl/xmllint for sitemap parsing

---

### Task 1: Write the README header and tech stack sections

**Files:**
- Modify: `README.md` (full rewrite, starting from scratch)

- [ ] **Step 1: Write the header section**

Replace the entire contents of `README.md` with:

```markdown
## Chien Chuan Wang

Concert Lighting Designer / Full Stack Engineer

Call me Chuan.
```

- [ ] **Step 2: Add the tech stack section with shields.io badges**

Append to `README.md`:

```markdown

---

### Tech Stack

**Languages**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white)
![Lua](https://img.shields.io/badge/Lua-2C2D72?style=flat&logo=lua&logoColor=white)
![Ruby](https://img.shields.io/badge/Ruby-CC342D?style=flat&logo=ruby&logoColor=white)

**Frameworks**

![Django](https://img.shields.io/badge/Django-092E20?style=flat&logo=django&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat&logo=fastapi&logoColor=white)
![Rails](https://img.shields.io/badge/Rails-D30001?style=flat&logo=rubyonrails&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=flat&logo=react&logoColor=black)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat&logo=nextdotjs&logoColor=white)

**Tools**

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)
![VS Code](https://img.shields.io/badge/VS%20Code-007ACC?style=flat&logo=visualstudiocode&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF?style=flat&logo=githubactions&logoColor=white)
```

- [ ] **Step 3: Verify badges render**

Open `README.md` in a markdown preview (or push to GitHub) and confirm all 15 badges load with correct logos and colors. Check that:
- Each badge shows the technology name and its logo
- Colors match official brand colors
- No broken image links

- [ ] **Step 4: Commit**

```bash
git add README.md
git commit -m "feat: rewrite profile README with header and tech stack"
```

---

### Task 2: Add featured projects section

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Append the featured projects section**

Append to `README.md` after the tech stack section:

```markdown

---

### Featured Projects

**[gma2-mcp](https://github.com/chienchuanw/gma2-mcp)** -- Remote control grandMA2 lighting consoles via MCP. Python-based Telnet automation for lighting workflows.

**[gma2-vscode-extension](https://github.com/chienchuanw/gma2-vscode-extension)** -- VS Code language support for grandMA2 command scripts. Syntax highlighting and hover docs.

**[asset-manager](https://github.com/chienchuanw/asset-manager)** -- Personal finance platform with portfolio tracking, cash flow management, and FIFO cost calculations. Built with Go.

**[vocab-hero](https://github.com/chienchuanw/vocab-hero)** -- Japanese vocabulary learning app with spaced repetition (SM-2), six study modes, and progress tracking. Next.js + Electron monorepo.
```

- [ ] **Step 2: Verify links**

Confirm all four repo URLs resolve correctly:

```bash
gh repo view chienchuanw/gma2-mcp --json url -q '.url'
gh repo view chienchuanw/gma2-vscode-extension --json url -q '.url'
gh repo view chienchuanw/asset-manager --json url -q '.url'
gh repo view chienchuanw/vocab-hero --json url -q '.url'
```

All four should return valid URLs.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "feat: add featured projects section to profile README"
```

---

### Task 3: Add blog posts section with placeholder markers

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Append the blog posts section with markers**

Append to `README.md` after the featured projects section:

```markdown

---

### Latest Blog Posts

<!-- BLOG-POST-LIST:START -->
<!-- BLOG-POST-LIST:END -->
```

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "feat: add blog posts section with auto-update markers"
```

---

### Task 4: Create the blog post auto-update workflow

**Files:**
- Create: `.github/workflows/blog-posts.yml`

- [ ] **Step 1: Create the workflow file**

The site has no RSS feed, so we parse `sitemap.xml` to extract blog post URLs and titles. The workflow fetches the sitemap, extracts English blog post URLs, fetches each page title, and writes them into the README markers.

Create `.github/workflows/blog-posts.yml`:

```yaml
name: Update Blog Posts

on:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:

permissions:
  contents: write

jobs:
  update-blog-posts:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5

      - name: Fetch blog posts from sitemap
        run: |
          # Extract English blog post URLs from sitemap
          URLS=$(curl -s https://www.chienchuanw.com/sitemap.xml \
            | grep -oP 'https://www\.chienchuanw\.com/en/blog/[^<]+')

          # Build markdown list of posts with titles
          POSTS=""
          for url in $URLS; do
            TITLE=$(curl -s "$url" \
              | grep -oP '(?<=<title>)[^<]+' \
              | head -1 \
              | sed 's/ *|.*//;s/^ *//;s/ *$//')
            if [ -n "$TITLE" ]; then
              POSTS="${POSTS}- [${TITLE}](${url})\n"
            fi
          done

          # Keep only the 5 most recent (sitemap order)
          POSTS=$(echo -e "$POSTS" | head -5)

          # Replace content between markers in README
          awk -v posts="$POSTS" '
            /<!-- BLOG-POST-LIST:START -->/ {
              print
              printf "%s\n", posts
              skip=1
              next
            }
            /<!-- BLOG-POST-LIST:END -->/ {
              skip=0
            }
            !skip {print}
          ' README.md > README.tmp && mv README.tmp README.md

      - name: Commit & Push
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com
          git add README.md
          if git diff --staged --quiet; then
            echo "No changes to commit"
          else
            git commit -m "docs: update blog posts"
            git push
          fi
```

- [ ] **Step 2: Verify the workflow syntax**

```bash
cat .github/workflows/blog-posts.yml | head -5
```

Expected: the `name:` and `on:` fields appear correctly.

- [ ] **Step 3: Commit**

```bash
git add .github/workflows/blog-posts.yml
git commit -m "feat: add GitHub Action to auto-update blog posts from sitemap"
```

---

### Task 5: Add GitHub stats section

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Append the GitHub stats section**

Append to `README.md` after the blog posts section:

```markdown

---

### GitHub Stats

![GitHub Stats](https://github-readme-stats.vercel.app/api?username=chienchuanw&show_icons=true&theme=github_dark&hide_border=true)
![Top Languages](https://github-readme-stats.vercel.app/api/top-langs/?username=chienchuanw&layout=compact&theme=github_dark&hide_border=true)
```

The two images will render side by side on wide screens and stack on narrow screens -- standard markdown image behavior.

- [ ] **Step 2: Verify stat cards load**

Open the two URLs in a browser to confirm they return valid SVG images:
- `https://github-readme-stats.vercel.app/api?username=chienchuanw&show_icons=true&theme=github_dark&hide_border=true`
- `https://github-readme-stats.vercel.app/api/top-langs/?username=chienchuanw&layout=compact&theme=github_dark&hide_border=true`

Both should render dark-themed stat cards with actual data.

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "feat: add GitHub stats section to profile README"
```

---

### Task 6: Add Beyond Code and Get in Touch sections

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Append the Beyond Code section**

Append to `README.md` after the GitHub stats section:

```markdown

---

### Beyond Code

I design concert lighting. See my work on [Instagram](https://www.instagram.com/chienchuanw/).
```

- [ ] **Step 2: Append the Get in Touch section**

Append to `README.md`:

```markdown

---

### Get in Touch

[Blog](https://www.chienchuanw.com/en/) / [LinkedIn](https://www.linkedin.com/in/chienchuanw/) / [Instagram](https://www.instagram.com/chienchuanw/) / [Facebook](https://www.facebook.com/chienchuan.wang/) / [Email](mailto:chienchuanwww@gmail.com)

Discord: chienchuanw
```

- [ ] **Step 3: Commit**

```bash
git add README.md
git commit -m "feat: add Beyond Code and Get in Touch sections to profile README"
```

---

### Task 7: Final review and cleanup

**Files:**
- Modify: `README.md` (if adjustments needed)

- [ ] **Step 1: Read the complete README and verify structure**

```bash
cat README.md
```

Verify the following in order:
1. Header: name, dual title, "Call me Chuan." -- no emoji
2. Tech Stack: 15 badges across 3 categories, all rendering
3. Featured Projects: 4 projects with working links
4. Latest Blog Posts: markers present (`START`/`END` comments)
5. GitHub Stats: 2 stat card images
6. Beyond Code: one sentence with Instagram link
7. Get in Touch: 5 links on one line, Discord below

- [ ] **Step 2: Verify no emoji exist anywhere in the file**

```bash
grep -P '[\x{1F300}-\x{1F9FF}]|[\x{2600}-\x{27BF}]|[\x{FE00}-\x{FE0F}]|[\x{1F600}-\x{1F64F}]' README.md || echo "No emoji found -- good"
```

Expected: "No emoji found -- good"

- [ ] **Step 3: Verify the old 3D contribution graph reference is removed**

```bash
grep -c "profile-3d-contrib" README.md
```

Expected: `0`

- [ ] **Step 4: Verify the old HTML comment block is removed**

```bash
grep -c "chienchuanw/chienchuanw.*special.*repository" README.md
```

Expected: `0`

- [ ] **Step 5: Push to GitHub and verify the profile renders**

```bash
git push
```

Then visit `https://github.com/chienchuanw` in a browser and confirm the profile renders correctly.

- [ ] **Step 6: Trigger the blog post workflow manually**

```bash
gh workflow run blog-posts.yml
```

Wait for it to complete, then pull and verify blog posts appeared between the markers:

```bash
gh run watch --exit-status
git pull
grep -A 10 "BLOG-POST-LIST:START" README.md
```

Expected: 5 blog post links between the START and END markers.
