# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the documentation site for **KMPShip** — a Kotlin/Compose Multiplatform boilerplate for building Android and iOS apps. The documentation is built with **MkDocs Material** and deployed to Vercel.

The site serves as the main resource for developers using KMPShip, covering setup, architecture foundations, features, tutorials, and customization.

## Development Commands

### Local Development

Start the MkDocs development server with live reload:

```bash
mkdocs serve
```

The site will be available at `http://127.0.0.1:8000/` and automatically reload on file changes.

### Build

Build the static site (outputs to `site/` directory):

```bash
mkdocs build
```

### Dependencies

Install or update Python dependencies:

```bash
pip install -r requirements.txt
```

The project uses `mkdocs-material==9.6.14` as the primary dependency.

## Project Structure

### Documentation Organization

Documentation is organized in the `docs/` directory following this structure:

- **docs/index.md** - Getting started guide (cloning KMPShip, Firebase setup, project renaming, running the app)
- **docs/customization.md** - Theme, app icons, welcome screen customization
- **docs/store-presence.md** - App store presence guidelines
- **docs/ui-components.md** - UI component documentation
- **docs/releases.md** - Release notes and changelog
- **docs/foundations/** - Core architecture concepts (clean architecture, DI, networking, image fetching)
- **docs/features/** - Feature documentation (authentication, notifications, payments, in-app reviews, etc.)
- **docs/features/publishing/** - App Store and Google Play publishing guides
- **docs/tutorials/** - Step-by-step tutorials (Firebase setup, Firestore activation, store account creation)
- **docs/extras/** - Additional resources (launchers, etc.)

### Navigation Structure

The site navigation is configured in `mkdocs.yml` under the `nav` section. When adding new pages:

1. Create the markdown file in the appropriate `docs/` subdirectory
2. Add an entry to the `nav` section in `mkdocs.yml` to make it discoverable

### Theme Customization

- **Custom theme overrides**: `overrides/partials/` (currently contains `footer.html`)
- **Custom CSS**: `docs/stylesheets/extra.css` (loaded via `extra_css` in mkdocs.yml)
- **Assets**: `docs/assets/` (logo, favicon, images)

The theme supports both light and dark modes with custom color schemes.

## Configuration Files

### mkdocs.yml

Main configuration file containing:

- Site metadata (name, description, URL, repo)
- Theme configuration (Material theme with custom colors, dark mode support)
- Markdown extensions (code highlighting, tabs, mermaid diagrams, emoji, admonitions)
- Navigation structure
- Social links (GitHub, X/Twitter)

### vercel.json

Vercel deployment configuration with:

- Automatic deployments disabled for the `main` branch
- Redirect rules ensuring trailing slashes on all documentation URLs (required for proper navigation)

## Content Guidelines

### Markdown Extensions Available

The site uses several pymdownx extensions:

- **Admonitions** - Info boxes, warnings, tips (e.g., `!!! info`, `!!! warning`, `!!! tip`)
- **Code highlighting** - With line numbers and copy button
- **Tabbed content** - For platform-specific instructions (e.g., Android vs iOS)
- **Mermaid diagrams** - For flowcharts and diagrams
- **Emoji** - Using Material emoji shortcodes

### Writing Style

- Target audience: Kotlin Multiplatform developers using KMPShip
- Tone: Technical but approachable, developer-friendly
- Include code examples for technical concepts
- Use admonitions to highlight important information (Complete Plan features, warnings, tips)
- Provide step-by-step instructions for tutorials
- Reference specific file paths and locations when documenting code changes

## Important Notes

- The site references the main KMPShip product repository at `github.com/TweenerLabs/kmp-ship`
- Documentation distinguishes between Starter Plan and Complete Plan features (marked with `<small>(Pro Plan)</small>`)
- Firebase is a core dependency of KMPShip - most features require Firebase configuration
- The documentation covers both Android Studio and Xcode workflows
