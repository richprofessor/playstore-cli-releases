# Google Play Store Automation CLI

For vibe coders who want the Play Store page to feel finished before the momentum disappears.

PlayStore CLI turns a mobile app folder into store copy, screenshots, mockups, and MCP tools while keeping the public install tiny and the source out of the published package.

The built-in MCP server keeps that same flow inside Claude, Codex, Cursor, Kilo, Cline, and Antigravity, so your editor can generate copy, inspect project structure, tweak mockup YAML, review templates, and pull ADB screenshots without breaking context.

## Quick Start

```bash
npm install -g playstore-cli

cd your_project_folder
playstore structure
playstore git
playstore analyze
playstore setup
playstore generate -n "My App"
```

## Public Release Model

This public repository is for release-facing assets only:

- GitHub Releases host the compiled binaries downloaded during `npm install`
- `showcases/` contains mockup previews for templates
- the npm package stays tiny and downloads the correct engine bundle at install time
- the source repository can stay private without breaking public installs

Current release asset naming:

- macOS / Linux: `playstore-<platform>-<arch>.tar.gz`
- Windows: `playstore-<platform>-<arch>.zip`
- MCP server uses the same pattern with `playstore-mcp-server-*`

Examples:

- `playstore-darwin-arm64.tar.gz`
- `playstore-linux-x64.tar.gz`
- `playstore-win32-x64.zip`
- `playstore-mcp-server-win32-x64.zip`

## Features

- **Project Insights** - `playstore structure` maps your architecture into readable Markdown.
- **Git Automation** - `playstore git`, `git status`, `git log`, and commit helpers reduce terminal churn.
- **MCP-Native Workflow** - Includes `playstore-mcp-server` plus short aliases like `text`, `generate`, `screenshots`, and `setup`.
- **Local Key Storage** - Save your Gemini key once with `playstore setup`; MCP and CLI both reuse `~/.playstore/config.json`.
- **No-Key Fallback** - Text copy still generates usable output even when no API key is configured.
- **Store Copy** - App title, short description, long description, keywords, and What's New.
- **Real Device Capture** - `playstore adb` pulls screenshots directly from a connected Android device.
- **Mockups and Graphics** - Phone screenshots, tablet screenshots, icons, and feature graphics.
- **HTML Preview** - Browse generated assets in `preview.html`.
- **JSON Report** - Character limits and output metadata are saved in `playstore_summary.json`.

## Mockup Templates

**43 Beautifully Designed Templates** (Phone & Tablet)

Browse the full preview of all templates in the `showcases/` folder on this repo: [Showcases Directory](https://github.com/richprofessor/playstore-cli-releases/tree/main/showcases)

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

### `generate` - Full generation (recommended)
```bash
playstore generate --project /path/to/your/app --output ./output
```

Options:
- `--project` / `-p` - Path to your app project (default: `.`)
- `--output` / `-o` - Output directory (default: `output`)
- `--name` / `-n` - Override app name
- `--color` / `-c` - Theme hex color (e.g. `4338ca` for indigo)
- `--phone-count` - Number of phone screenshots (1-8, default: 8)
- `--tablet-count` - Number of tablet screenshots (1-8, default: 8)
- `--no-ai-images` - Use procedural image generation (faster, no API cost)
- `--skip-screenshots` - Skip screenshot generation
- `--skip-copy` - Skip LLM copy generation
- `--adb` - Use ADB to pull screenshots from a connected Android device

### `text` - Generate only text copy
```bash
playstore text --project /path/to/your/app --output ./output
```

### MCP aliases
If you are calling PlayStore through an MCP client, these short aliases are available too:
- `text` -> generate text copy and save files
- `generate` -> full pipeline
- `screenshots` -> screenshot/mockup generation
- `setup` -> save or update the Gemini API key

The MCP tools also expose the longer names:
- `generate_store_copy`
- `generate_full_pipeline`
- `generate_screenshots`
- `save_gemini_api_key`

When no API key is present, text generation falls back to local generation so files can still be created.

### `screenshots` - Generate only screenshots
```bash
playstore screenshots --app-name "MyApp" --color 4338ca --output ./output
```

### `adb` - ADB Screenshot Fetcher
```bash
playstore adb
playstore generate --adb
```
Connects to your Android device over ADB and pulls live layout screenshots sized for mockups.

### `structure` - Visualize project structure
```bash
playstore structure
```
Generates a readable emoji-based filesystem layout while skipping standard ignore folders such as `node_modules` and `.git`.

### `git` - Smart Git helper
```bash
playstore git
playstore git log
playstore git status
```
Interactive prompts handle standard Git operations safely while keeping common history and status checks simple.

### `analyze` - Analyze project metadata
```bash
playstore analyze --project /path/to/your/app
```

## Output Structure

```text
output/
├── title.txt
├── short_description.txt
├── long_description.txt
├── keywords.txt
├── whats_new.txt
├── feature_graphic.png
├── icon_512x512.png
├── phone/
│   ├── phone_screenshot_01.png  ... phone_screenshot_08.png
├── tablet/
│   ├── tablet_screenshot_01.png ... tablet_screenshot_08.png
├── preview.html
└── playstore_summary.json
```

## Supported Project Types

The tool auto-detects metadata from:
- **React Native / Expo** - `package.json`, `app.json`, `app.config.js/ts`
- **Flutter** - `pubspec.yaml`
- **Android Native** - `AndroidManifest.xml`, `build.gradle`
- **Any project** with a `README.md`
