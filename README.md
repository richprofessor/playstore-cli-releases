# Google Play Store Automation CLI

Automate your Google Play Store listing assets generation from any mobile app project (React Native, Flutter, Android Native, etc).

## Quick Start

```bash
# Install globally via NPM
npm install -g playstore-cli

cd your_project_folder
playstore structure        # Visualize and document your project structure
playstore git              # Smart Git commits and initialization
playstore analyze          # Scan project metadata
export GEMINI_API_KEY=your_key
playstore generate -n "My App" # Generate complete Play Store assets
```

## Features

- **Project Insights** — `playstore structure` visually maps your architecture into Markdown.
- **Git Automation** — `playstore git` interactively handles commits and repos.
- **AI MCP Server** — Includes an MCP server (`playstore-mcp-server`) to integrate with AI IDEs like Cursor and Claude.
- **LLM-powered copy** — App title, short description, long description, keywords, and What's New section.
- **Real Device Screenshots** — `playstore adb` captures screenshots directly from your connected Android device.
- **Feature graphic** — 1024×500 banner image (AI-generated or procedural with Pillow/OpenCV).
- **Phone screenshots** — 1080×1920 mockup screenshots with smart text framing.
- **Tablet screenshots** — 1600×2560 mockup screenshots.
- **App icon** — 512×512 icon with gradient and initials.
- **HTML preview** — Browse all assets in your browser inside a beautiful generated `preview.html`.
- **JSON report** — Machine-readable summary (`playstore_summary.json`) with character limit validation validations.

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

### `screenshots` — Generate only screenshots
```bash
playstore screenshots --app-name "MyApp" --color 4338ca --output ./output
```

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
