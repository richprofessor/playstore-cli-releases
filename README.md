# Google Play Store Automation CLI

Automate your Google Play Store listing assets generation from any mobile app project.

## Quick Start

```bash
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
- **LLM-powered copy** — App title, short description, long description, keywords, and What's New section
- **Feature graphic** — 1024×500 banner image (AI-generated or procedural with Pillow/OpenCV)
- **Phone screenshots** — 1080×1920 mockup screenshots (up to 8)
- **Tablet screenshots** — 1600×2560 mockup screenshots (up to 8)
- **App icon** — 512×512 icon with gradient and initials
- **HTML preview** — Browse all assets in your browser
- **JSON report** — Machine-readable summary with validation checks

## Mockup Templates

**43 Beautifully Designed Templates** (Phone & Tablet)

Browse the full preview of all templates in the \`showcases/\` folder on our GitHub repo!

| # | Template | Style |
|---|---------------|-------|
| 1 | `gradient_minimal` | Clean vertical gradient, centered device |
| 2 | `dark_neon` | Pure black with neon glow borders |
| 3 | `split_left` | Text left, screenshot right |
| 4 | `split_right` | Screenshot left, text right |
| 5 | `glass_card` | Frosted glass card on dark bg |
| 6 | `bold_headline` | Huge bold headline, screenshot below |
| 7 | `material` | Google Material Design card style |
| 8 | `ios_clean` | Pure white iOS-style minimalism |
| 9 | `retro_wave` | 80s synthwave retro gradient |
| 10 | `neon_lines` | Black bg with neon circuit lines |
| 11 | `pastel_soft` | Soft pastel tones, rounded & friendly |
| 12 | `cinematic` | Cinematic black bars, full bleed |
| 13 | `diagonal_split` | Diagonal color split background |
| 14 | `floating_device` | Floating device, heavy shadow |
| 15 | `magazine` | Magazine cover, image fills top |
| 16 | `minimal_white` | Ultra-minimal white, thin accents |
| 17 | `colorful_blocks` | Geometric colored blocks mosaic |
| 18 | `nature_gradient` | Teal/green/blue nature gradient |
| 19 | `sunset_warm` | Warm coral/orange/purple sunset |
| 20 | `premium_dark` | Luxury dark charcoal with gold accents |

| # | Tablet Template | Style |
|---|----------------|-------|
| 1 | `landscape_focus` | Wide landscape, text above device |
| 2 | `showcase` | Large centered device, dark spotlight |
| 3 | `minimal_white` | Clean white, thin color border |
| 4 | `dark_professional` | Professional dark, enterprise style |
| 5 | `colorful_bold` | Bold vibrant colors, great for games |
| 6 | `magazine` | Editorial magazine layout |
| 7 | `gradient_wave` | Wave-like diagonal gradient |
| 8 | `glass_panels` | Glassmorphism frosted panels |
| 9 | `retro_style` | Vintage retro poster |
| 10 | `premium` | Ultra-premium dark with gold |

**15 fonts available:** `sans`, `sans_bold`, `serif`, `serif_bold`, `mono`, `inter`, `roboto`, `roboto_bold`, `poppins`, `poppins_bold`, `montserrat`, `playfair`, `nunito`, `oswald`, `raleway`

```bash
# List all templates + fonts
playstore mockup list

# Single template
playstore mockup generate -s screenshot.png --template gradient_minimal --font poppins

# Multiple templates
playstore mockup generate -s screenshot.png --template dark_neon --template material --font inter

# All 20 phone templates at once
playstore mockup generate -s screenshot.png --template all --device phone

# All 30 templates (phone + tablet)
playstore mockup generate -s screenshot.png --template all --device both --font playfair

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

### `text` — Generate only text copy
```bash
playstore text --project /path/to/your/app --output ./output
```

### `screenshots` — Generate only screenshots
```bash
playstore screenshots --app-name "MyApp" --color 4338ca --output ./output
```

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
├── preview.html               # Browse all assets in browser
└── playstore_summary.json     # Full JSON report with validation
```

## Supported Project Types

The tool auto-detects metadata from:
- **React Native / Expo** — `package.json`, `app.json`, `app.config.js/ts`
- **Flutter** — `pubspec.yaml`
- **Android Native** — `AndroidManifest.xml`, `build.gradle`
- **Any project** with a `README.md`

## Play Store Limits

| Asset | Limit |
|-------|-------|
| App Title | 50 characters |
| Short Description | 80 characters |
| Long Description | 4,000 characters |
| What's New | 500 characters |
| Feature Graphic | 1024 × 500 px |
| Phone Screenshots | 1080 × 1920 px |
| Tablet Screenshots | 1600 × 2560 px |
| App Icon | 512 × 512 px |
