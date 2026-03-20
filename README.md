# Google Play Store Automation CLI

For vibe coders who want the Play Store page to feel finished before the momentum disappears.

PlayStore CLI turns a mobile app folder into store copy, screenshots, mockups, and MCP tools while keeping the public install tiny and your Python source out of the published package.

The built-in MCP server keeps that same flow inside Claude, Codex, Cursor, Kilo, Cline, and Antigravity, so your editor can generate copy, inspect project structure, browse repos, fetch exact files, convert PDFs, and reshape content for downstream agents without breaking context.

## Quick Start

```bash
npm install -g playstore-cli

cd your_project_folder
playstore structure
playstore git
playstore analyze
playstore view .
playstore setup
playstore generate -n "My App"
```

## Features

- **Project Insights** - `playstore structure` maps your architecture into readable Markdown.
- **Git Automation** - `playstore git`, `git status`, `git log`, and commit helpers reduce terminal churn.
- **MCP-Native Workflow** - Includes `playstore-mcp-server` plus short aliases like `text`, `generate`, `screenshots`, and `setup`.
- **Repo Context for Agents** - `repo_context` gives an LLM a lightweight tree, top candidate files, format decisions, and a handoff prompt before it asks for exact content.
- **Targeted File Viewing** - `playstore view` and `repo_view` can fetch a single file, heading, symbol, JSON path, line range, or PDF page range instead of dumping an entire repo.
- **Direct Conversion Pipeline** - `playstore convert` and `repo_convert` can turn the selected content into `txt`, `md`, `json`, `yaml`, or `csv` with optional save paths.
- **Inline Content Reshaping** - Convert can transform only the matched block while keeping surrounding text untouched, including markdown tables, structured JSON row output, and key remapping.
- **PDF and OCR Recovery** - Page selectors, paragraph cleanup, page marker preservation, and fuzzy start/end marker matching make messy PDFs much easier to consume.
- **Local Key Storage** - Save your Gemini key once with `playstore setup`; MCP and CLI both reuse `~/.playstore/config.json`.
- **No-Key Fallback** - Text copy still generates usable output even when no API key is configured.
- **Store Copy** - App title, short description, long description, keywords, and What's New.
- **Real Device Capture** - `playstore adb` pulls screenshots directly from a connected Android device.
- **Mockups and Graphics** - Phone screenshots, tablet screenshots, icons, and feature graphics.
- **HTML Preview** - Browse generated assets in `preview.html`.
- **JSON Report** - Character limits and output metadata are saved in `playstore_summary.json`.

## Install and Releases

The public install stays lightweight:

- `npm install -g playstore-cli` installs the launchers and docs
- the correct compiled binaries are downloaded for the current OS during install
- the public releases repo hosts download assets and showcase files
- Python source is not published in the npm package or release assets

Current release asset naming:

- macOS / Linux: `playstore-<platform>-<arch>.tar.gz`
- Windows: `playstore-<platform>-<arch>.zip`
- MCP server uses the same pattern with `playstore-mcp-server-*`

Examples:

- `playstore-darwin-arm64.tar.gz`
- `playstore-linux-x64.tar.gz`
- `playstore-win32-x64.zip`
- `playstore-mcp-server-win32-x64.zip`

## Mockup Templates

**43 Beautifully Designed Templates** (Phone & Tablet)

Browse the full preview of all templates in the `showcases/` folder on our GitHub repo: [Showcases Directory](https://github.com/richprofessor/playstore-cli-releases/tree/main/showcases)

**15 fonts available:** `sans`, `sans_bold`, `serif`, `serif_bold`, `mono`, `inter`, `roboto`, `roboto_bold`, `poppins`, `poppins_bold`, `montserrat`, `playfair`, `nunito`, `oswald`, `raleway`

```bash
# List all templates + fonts
playstore mockup list

# Single template with a specific font
playstore mockup generate -s screenshot.png --template gradient_minimal --font poppins

# Multiple templates at once
playstore mockup generate -s screenshot.png --template dark_neon --template material --font inter

# All phone templates at once
playstore mockup generate -s screenshot.png --template all --device phone

# Batch: apply template to every screenshot in a folder
playstore mockup batch --screenshots-dir output/raw --phone-template glass_card --tablet-template showcase --device both

# Preview all 15 fonts on one template
playstore mockup preview-fonts -s screenshot.png --template material
```

## Commands

### `generate` — Full generation (recommended)
```bash
playstore generate --project /path/to/your/app --output ./output
```

Options:
- `--project` / `-p` — Path to your app project (default: `.`)
- `--output` / `-o` — Output directory (default: `output`)
- `--name` / `-n` — Override app name
- `--color` / `-c` — Theme hex color (e.g. `4338ca` for indigo)
- `--phone-count` — Number of phone screenshots (1-8, default: 8)
- `--tablet-count` — Number of tablet screenshots (1-8, default: 8)
- `--no-ai-images` — Use procedural image generation (faster, no API cost)
- `--skip-screenshots` — Skip screenshot generation
- `--skip-copy` — Skip LLM copy generation
- `--adb` — Use ADB to pull screenshots from connected Android device

### `text` — Generate only text copy
```bash
playstore text --project /path/to/your/app --output ./output
```

### MCP aliases
If you are calling PlayStore through an MCP client, these short aliases are available too:
- `text` → generate text copy and save files
- `generate` → full pipeline
- `screenshots` → screenshot/mockup generation
- `setup` → save or update the Gemini API key

The MCP tools also expose the longer names:
- `generate_store_copy`
- `generate_full_pipeline`
- `generate_screenshots`
- `save_gemini_api_key`
- `repo_context`
- `repo_view`
- `repo_convert`

When no API key is present, text generation falls back to local generation so files can still be created.
The MCP config is written automatically during install, but editors still need a restart or refresh to load the new server entry.

### `screenshots` — Generate only screenshots
```bash
playstore screenshots --app-name "MyApp" --color 4338ca --output ./output
```

### `view` — Inspect a repo, folder, file, or PDF interactively
```bash
playstore view 'https://github.com/toon-format/toon'
playstore view ./docs/ARCHITECTURE.md
playstore view './docs/book.pdf' --format txt --page-start 17 --page-end 20 --no-save-prompt
```

Use `view` when you want a human-readable browse flow first:
- Repo or folder sources open with a structure-first browser so you can choose the next file interactively.
- Local files and remote targets can be narrowed with `--target-path`, `--line-start`, `--line-end`, `--head-lines`, `--tail-lines`, `--symbol`, `--heading`, `--page`, `--page-start`, `--page-end`, `--json-path`, and `--chunk-index`.
- After printing the content, CLI can save it to a suggested `tmp/` path in the source project.

### `convert` — Exact output tool for files, pages, headings, and blocks
```bash
playstore convert 'https://github.com/toon-format/toon' --target-path README.md --format md --no-save-prompt
playstore convert ./docs/ARCHITECTURE.md --format json --preset book_json --no-save-prompt
playstore convert './docs/book.pdf' --format json --page-start 17 --page-end 20 --no-save-prompt
```

Use `convert` when you already know what you want:
- Converts the selected source into `txt`, `md`, `json`, `yaml`, or `csv`.
- Supports the same selectors as `view`, plus `--save`, `--report-json`, and `--allow-directory-conversion`.
- Built for agent workflows where the exact file or page is already known.

Structured transform options:
- `--json-shape default|text_document|chapters|table_rows`
- `--md-shape default|table`
- `--heading-style default|plain|bold|numbered`
- `--preset book_json|chapter_json|table_json|clean_md|report_md`
- `--field-map old=new,...`
- `--table-columns col1,col2,...`
- `--table-delimiter '|'`

Marker-based and fuzzy transforms:
- `--start-text` / `--end-text` to isolate a block
- `--inline-transform` to replace only that block and keep the rest untouched
- `--drop-selection` to remove a matched block
- `--fuzzy-match --fuzzy-threshold 60` when OCR or typos break exact marker matching

### `adb` — ADB Screenshot Fetcher
```bash
playstore adb
# Or run generate with ADB:
playstore generate --adb
```
Connects to your android device over ADB and pulls live layout screenshots perfectly sized for mockups.

### `structure` — Visualize Project Structure
```bash
playstore structure
```
Generates a highly readable emoji-based filesystem layout of your workspace while skipping standard ignore folders (like `node_modules` or `.git`). Let's you save the raw overview straight to a Markdown file.

### `git` — Smart Git Helper
```bash
playstore git
playstore git log
playstore git status
```
Interactive prompt to handle standard git operations safely. Allows simple commits, pushes, and easily outputs Git history.

### `analyze` — Analyze project metadata
```bash
playstore analyze --project /path/to/your/app
```

## Repo Context and MCP Workflow

The repo tooling is split intentionally so an LLM can stay efficient:

- `repo_context` is the guided first pass. It returns a project skeleton, top paths, format hints, and an agent handoff prompt.
- `repo_view` is the targeted read path. It fetches only the file, heading, symbol, JSON path, lines, or PDF pages the model asks for.
- `repo_convert` is the exact-output path. It skips guidance and returns the converted content directly, optionally reshaped or saved.

Typical flow:

```text
repo_context -> repo_view -> repo_convert
```

Examples:

```json
{
  "tool": "repo_context",
  "arguments": {
    "source": "https://github.com/toon-format/toon",
    "goal": "Understand the repo and ask for exact files next"
  }
}
```

```json
{
  "tool": "repo_view",
  "arguments": {
    "source": "https://github.com/toon-format/toon",
    "target_path": "README.md",
    "requested_format": "md"
  }
}
```

```json
{
  "tool": "repo_convert",
  "arguments": {
    "source": "/path/to/book.pdf",
    "requested_format": "json",
    "page_start": 17,
    "page_end": 20,
    "preset": "chapter_json",
    "content_only": true
  }
}
```

## Output Structure

```
output/
├── title.txt                  # App title (max 50 chars)
├── short_description.txt      # Short description (max 80 chars)
├── long_description.txt       # Long description (max 4000 chars)
├── keywords.txt               # 30 SEO keywords
├── whats_new.txt              # What's New section (max 500 chars)
├── feature_graphic.png        # 1024×500 feature graphic
├── icon_512x512.png           # 512×512 app icon
├── phone/
│   ├── phone_screenshot_01.png  ... phone_screenshot_08.png
├── tablet/
│   ├── tablet_screenshot_01.png ... tablet_screenshot_08.png
├── preview.html               # Browse all generated assets in a visual grid!
└── playstore_summary.json     # Detailed JSON report with character limit validations
```

## Supported Project Types

The tool auto-detects metadata from:
- **React Native / Expo** — `package.json`, `app.json`, `app.config.js/ts`
- **Flutter** — `pubspec.yaml`
- **Android Native** — `AndroidManifest.xml`, `build.gradle`
- **Any project** with a `README.md`
