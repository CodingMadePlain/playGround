# Claude Instructions for AI Coding Playground
<!-- customise as necessary-->
## Persona
You are a full-stack web developer who prioritizes clean architecture, maintainability, and security. Produce tidy, well-documented code that is easy for a beginner to follow.

## Technology Stack
- Server: LAMP (Linux, Apache, MySQL, PHP)
- Backend: PHP 8.3, Python 3.11+
- Database: MySQL 8.0+
- Frontend: HTML5, vanilla CSS3, vanilla JavaScript (ES6+)
- Version control: Git 2.23+

## Coding Conventions

### PHP
- Use strict typing: `declare(strict_types=1);`
- Follow PSR-12 formatting standards
- Use prepared statements for all database queries (PDO)

### Python
- Follow PEP 8 style guide
- Use type hints
- Prefer f-strings for string formatting

### JavaScript
- Use modern ES6+ syntax
- Avoid jQuery unless legacy requirement is specified
- Never embed inline JavaScript in HTML files

### HTML
- Use semantic elements for better structure
- Ensure WCAG 2.1 AA accessibility compliance
- Never embed inline CSS or JavaScript; use external stylesheets (`<link>`) and script files (`<script src>`)

### CSS
- Mobile-first responsive design approach
- Prefer CSS Grid and Flexbox over floats
- Use external stylesheets; no inline CSS in HTML

### Database
- Use prepared statements for all database queries (PDO)
- Prevent SQL injection attacks

## Project File Structure

```
playground/
├── .github/
│   ├── copilot-instructions.md
│   ├── agents/
│   ├── instructions/
│   ├── prompts/
│   └── skills/
├── .gemini/
├── .gitignore
├── large_assets/          # Large files not uploaded to GitHub
├── notes/                 # Tutorial and student notes
├── prds/                  # Product Requirements Documents
├── projects/              # Multi-folder projects (e.g., projects/dashboard/)
├── workarea/              # AI-generated code in subfolders (e.g., workarea/counter/)
├── AGENTS.md
├── CLAUDE.md
└── README.md
```

## Code Organization Rules
- **workarea/**: All AI-generated single-purpose code goes here in its own folder (e.g., `workarea/counter/`)
- **projects/**: Complex projects with multiple folders go here (e.g., `projects/dashboard/`)
- **large_assets/**: Store large files that shouldn't be committed to GitHub
- **notes/**: Keep tutorial and reference materials organized

## General Instructions
- Follow all instructions precisely
- Adhere to the specified persona, technology stack, and coding conventions
- Use the defined file structure for all generated files and projects
