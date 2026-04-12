# GitHub Profile README Redesign

## Identity

Chien Chuan Wang (Chuan) -- concert lighting designer and full stack engineer. The profile should communicate that these are not two separate identities but one person who lives at the intersection of art and technology.

## Design Principles

- No emoji anywhere
- Clean, structured, GitHub-native aesthetic
- Sans-serif typographic feel (system-ui where possible, shields.io badges for tech stack)
- Linear top-to-bottom flow -- no tables-as-layout, no collapsible sections
- Rich and detailed content, but every section earns its place

## Sections (in order)

### 1. Header

```markdown
## Chien Chuan Wang

Concert Lighting Designer / Full Stack Engineer

Call me Chuan.
```

Factual and tight. The dual title does the storytelling. No tagline, no narrative.

### 2. Tech Stack

Shields.io badges in flat style, grouped by category with bold labels. No emoji before category names.

**Languages:** Python, TypeScript, JavaScript, Go, Lua, Ruby

**Frameworks:** Django, FastAPI, Rails, React, Next.js

**Tools:** Docker, Git, VS Code, GitHub Actions

Each badge uses `shields.io/badge/{name}-{color}?style=flat&logo={logo}&logoColor=white` or similar. Color choices should be the official brand colors for each technology. Dark-themed badges to match GitHub dark mode.

### 3. Featured Projects

Four hand-picked repos, each with a linked repo name and a one-line description. These are chosen to represent the intersection of lighting and engineering:

1. **gma2-mcp** -- Remote control grandMA2 lighting consoles via MCP. Python-based Telnet automation for lighting workflows.
2. **gma2-vscode-extension** -- VS Code language support for grandMA2 command scripts. Syntax highlighting and hover docs.
3. **asset-manager** -- Personal finance platform with portfolio tracking, cash flow management, and FIFO cost calculations. Built with Go.
4. **vocab-hero** -- Japanese vocabulary learning app with spaced repetition (SM-2), six study modes, and progress tracking. Next.js + Electron monorepo.

Each project name links to the repo. Descriptions are plain text, no badges.

### 4. Latest Blog Posts

Auto-populated via GitHub Action using `gautamkrishnar/blog-post-workflow`. Pulls from the RSS feed at `https://www.chienchuanw.com/en/`.

```markdown
### Latest Blog Posts

<!-- BLOG-POST-LIST:START -->
<!-- BLOG-POST-LIST:END -->
```

A new workflow file (`.github/workflows/blog-posts.yml`) runs on a schedule (e.g., daily) and on `workflow_dispatch`. It updates the README between the markers. Display the 5 most recent posts.

### 5. GitHub Stats

Two `github-readme-stats` widgets displayed side by side (using markdown image syntax on the same line or an HTML approach if needed for alignment):

1. **GitHub Stats card** -- shows total stars, commits, PRs, issues, contributions
2. **Top Languages card** -- shows most-used languages by repo

Both use `theme=github_dark` to match the profile. No streak widget.

### 6. Beyond Code

```markdown
### Beyond Code

I design concert lighting. See my work on [Instagram](https://www.instagram.com/chienchuanw/).
```

One sentence. One link. The repos already proved the intersection; this just gives people a way to see the visual side.

### 7. Get in Touch

```markdown
### Get in Touch

[Blog](https://www.chienchuanw.com/en/) / [LinkedIn](https://www.linkedin.com/in/chienchuanw/) / [Instagram](https://www.instagram.com/chienchuanw/) / [Facebook](https://www.facebook.com/chienchuan.wang/) / [Email](mailto:chienchuanwww@gmail.com)

Discord: chienchuanw
```

Same links as current profile. Single line with slash separators. Discord on its own line below.

## Implementation Notes

### New files to create
- `.github/workflows/blog-posts.yml` -- GitHub Action for auto-pulling blog posts

### Files to modify
- `README.md` -- full rewrite with the new structure

### Dependencies (external services)
- `shields.io` -- badge images (no API key needed)
- `github-readme-stats` (anuraghazra) -- stats widgets (uses GitHub API via Vercel deployment, no setup needed)
- `gautamkrishnar/blog-post-workflow` -- blog post auto-pull action (needs RSS feed URL)

### Things to verify during implementation
- RSS feed URL at `https://www.chienchuanw.com/en/` -- confirm it serves valid RSS/Atom
- shields.io badge colors and logo names for each technology
- github-readme-stats theme rendering in dark mode

### Things to remove from current README
- The 3D contribution graph image (`profile-grayscale.svg` reference). The graph files and workflow remain in the repo for potential future use, but the README will no longer display them.
