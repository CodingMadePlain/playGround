# Project Guidelines

## Project Overview

This workspace is a VS Code training environment authored by Dele Oke. It teaches how to build web applications using AI assistants. The project fetches remote CSV and JSON files, saves them locally, and displays data in HTML pages via VS Code Live Server. It also covers generating website templates from images and processing images with Python Pillow.

## Architecture

| Folder | Purpose |
|--------|---------|
| `projects/` | All AI-generated code. Each feature gets its own sub-folder, e.g. `projects/dashboard/`. |
| `workarea/` | Hand-coded sample files only. Do not place AI-generated files here unless explicitly requested. |
| `prds/` | Product Requirements Documents. Read the relevant PRD before starting any task. |
| `notes/` | Tutorial notes and reference material. Do not modify these files unless asked. |
| `large_assets/` | Large files excluded from Git (gitignored). Place files over 10 MB here. |
| `.github/instructions/` | Scoped instruction files. See [writing.instructions.md](instructions/writing.instructions.md) for Markdown conventions. |

## Technology Stack

- **Server:** LAMP (Linux, Apache, MySQL, PHP)
- **Backend:** PHP 8.3, Python 3.11+
- **Database:** MySQL 8.0+
- **Frontend:** HTML5, vanilla CSS3, vanilla JavaScript ES6+
- **Version Control:** Git 2.23+

## Code Style

- Write vanilla JavaScript (ES6+). Do not introduce frameworks or libraries.
- Write vanilla CSS3. Do not use CSS preprocessors or frameworks.
- Target PHP 8.3 syntax and features.
- Use Python 3.11+ idioms and type hints.
- Use British English in all user-facing content, comments, and documentation.

## Conventions

- **AI-generated code belongs in `projects/`:** Create a descriptive sub-folder per task (e.g. `projects/csv-table/`). Never place generated code in `workarea/`.
- **PRD-driven development:** Always read the relevant file in `prds/` before writing code. Follow the requirements exactly.
- **No secrets in Git:** Store environment variables and credentials in `.env`. The `.gitignore` excludes this file.
- **No large files in Git:** Place any file over 10 MB in `large_assets/`. The `.gitignore` excludes this folder.

## Writing and Markdown

All `.md` files follow [writing.instructions.md](instructions/writing.instructions.md).

Key rules: British English spelling, no contractions, active voice, no em dashes or en dashes as punctuation, sentences under 20 words.

## References

- [VS Code Customisation Overview](https://code.visualstudio.com/docs/copilot/customization/overview)
- [VS Code Best Practices for AI](https://code.visualstudio.com/docs/copilot/best-practices)
- [Notes: Introduction](../notes/introduction.md)
- [Notes: References](../notes/references.md)
