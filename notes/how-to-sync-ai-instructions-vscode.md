# How to — Use Custom Instructions in VS Code — Sync Copilot, Claude & Gemini"
**Single video transcript**  
**Channel:** AI Technology Simplified  
**Estimated runtime:** ~11–12 minutes  
**Target audience:** Developers with basic VS Code knowledge, new to custom instructions

---

## INTRO (0:00 – 0:45)

Hey everyone, welcome to AI Technology Simplified. I'm Dele.

If you're working with AI in VS Code, you're probably using more than one tool. Maybe you start with GitHub Copilot, then bring in Claude Code for a deeper conversation, or use Gemini CLI from the terminal. The moment you do that, a problem appears: each tool has its own instructions file. So you end up writing the same project context three times, in three different places. Your stack, your database name, your coding standards — all duplicated.

In this video I'm going to show you how to maintain those instructions in one place and have all three tools read from it. One update, everywhere. No duplication, no drift.

Let's get into it.

---

## SECTION 1 — The Problem With Repetition (0:45 – 2:15)

Let's say you have a LAMP stack project. You write custom instructions for Copilot so it understands your stack, your database structure, and your coding standards. Good.

Then you open Claude Code — and it has no idea about any of that. So you copy your instructions into a new file for Claude. Now you maintain two files.

Then you want to use Gemini CLI. That's a third instructions file.

Three months later you decide to upgrade from PHP 7.4 to PHP 8.2. You edit the instructions for Copilot. You remember to edit Claude's file. You forget about Gemini until a month later when you're working in the CLI and get nonsensical suggestions because the instructions are stale.

This is a problem developers call **repeating yourself**. And there's a principle in software development that says you should avoid it. That principle is **DRY** — Don't Repeat Yourself.

The idea is simple: if a piece of information exists in more than one place, it will eventually fall out of sync. You update it here, forget to update it there, and now you have contradictions.

The solution is to maintain that information in **one place** and have every tool read from it. This video shows you exactly how.

---

## SECTION 2 — The Architecture (2:15 – 3:30)

Here's what we're building:

```
my-project/
├── AGENTS.md                              ← source of truth (you edit this)
│
├── CLAUDE.md                              ← references the agent (you edit this)
├── GEMINI.md                              ← references the agent (you edit this)
│
└── .github/
    ├── copilot-instructions.md           ← copy of AGENTS.md (you edit this)
    └── instructions/
        ├── general.instructions.md       ← applyTo: ** (you edit this)
        ├── php.instructions.md           ← applyTo: **/*.php (you edit this)
        └── database.instructions.md     ← applyTo: **/*.sql (you edit this)
```

Three simple principles here:

**One:** `AGENTS.md` is your single source of truth for project context. Everything else either copies it or references it.

**Two:** Copilot gets two files — one always-on file with your project context, and scoped files that load based on what you're working on.

**Three:** Claude and Gemini each get a single file. That file contains your project context plus a reference to a shared agent definition that both tools can read.

---

## SECTION 3 — Creating AGENTS.md (3:30 – 4:30)

Create a file called `AGENTS.md` in your project root. This is the only file you manually maintain for project context:

```markdown
# Project: My LAMP Stack App

## Stack
- OS: Linux (Ubuntu)
- Web Server: Apache
- Database: MySQL
- Backend: PHP 8.2
- Frontend: HTML, CSS, vanilla JavaScript

## Project Structure
- `/var/www/html/` — Apache web root
- `/etc/apache2/` — Apache configuration
- Database name: `my_app_db`

## General Standards
- Use meaningful variable names — avoid single-letter names except in short loops
- Add a comment above every function explaining what it does
- Keep functions small and focused on one task
- Flag any security concerns immediately
- Suggest the simplest solution first
- Explain what code does, don't just write it

## What I'm Building
A web application with user authentication and a dashboard.
This is a learning project — prioritise clarity over cleverness.
```

This file is the baseline. Every AI tool will read from it.

---

## SECTION 4 — Setting Up Copilot (4:30 – 5:45)

Copilot reads from `.github/copilot-instructions.md`. Create that file and copy the full content of `AGENTS.md` into it.

Your `.github/instructions/` scoped files stay exactly as they are — no changes needed there. Copilot will read the always-on file for context, then automatically load the right scoped file based on what you're working on.

To prevent the two files drifting out of sync, add a reminder comment at the top of `AGENTS.md`:

```markdown
<!-- Source file. Copy content to .github/copilot-instructions.md when updated. -->
```

---

## SECTION 5 — Setting Up Claude and Gemini (5:45 – 7:15)

Claude and Gemini work differently from Copilot. They don't have scoped loading — they each read one flat file. So that file needs to contain both your project context and your scoped rules combined.

Rather than duplicating everything, we use a reference. Create `CLAUDE.md` in your project root:

```markdown
# Claude Instructions

Follow the instructions and conventions inside `.vscode/agents/sync-instructions.md`.

---

[Full content of AGENTS.md here]

---

## Scoped Rules

Apply the relevant section depending on the file type being discussed.

### General (all files)
[rules from .github/instructions/general.instructions.md]

### PHP (applies to .php files)
[rules from .github/instructions/php.instructions.md]

### SQL (applies to .sql files)
[rules from .github/instructions/database.instructions.md]
```

Create `GEMINI.md` using the exact same structure — just replace the Claude header with a Gemini header.

The key line is at the top: **"Follow the instructions and conventions inside `.vscode/agents/sync-instructions.md`."** When Claude or Gemini reads this, it knows to treat that agent file as part of its instructions. You tested this and it works exactly as expected.

---

## SECTION 6 — Creating the Shared Agent Definition (7:15 – 8:45)

Now create the agent definition that both Claude and Gemini reference. Create this folder and file:

```
.vscode/agents/sync-instructions.md
```

This file contains the detailed coding conventions that both tools should follow. Here's an example:

```markdown
---
name: Sync Instructions
description: >
  Shared conventions and standards for Claude and Gemini in this project.
---

## Code Standards

### Always use PDO for database queries
- Use prepared statements with named placeholders
- Example: `:user_id`, `:email`
- Never concatenate user input directly into SQL strings

### Comments above functions
Every function must have a docblock comment explaining:
- What the function does
- What parameters it accepts and their types
- What it returns

### Variable naming
- Use camelCase for variables: `$userId`, `$userName`
- Use UPPER_SNAKE_CASE for constants: `DB_HOST`, `MAX_RETRIES`
- Avoid single letters except in short loops

### Security
- Always validate and sanitise user input before using it
- Use prepared statements — never string concatenation for SQL
- Check return values and handle errors explicitly
- Flag any potential security issues immediately
```

This file is the single source of truth for your detailed conventions. Both Claude and Gemini read it automatically when they see the reference at the top of their instructions files.

---

## SECTION 7 — Your Maintenance Workflow (8:45 – 10:00)

With this setup in place, here's your day-to-day:

**When your project context changes** — new database, updated PHP version, different project description:
- Edit `AGENTS.md`
- Copy the updated content into `.github/copilot-instructions.md`
- All three tools are updated

**When you update a general coding convention** — tighter security rules, new naming standards:
- Edit `.vscode/agents/sync-instructions.md`
- Both Claude and Gemini read the updated version automatically
- Copilot's behaviour is driven by its scoped files, so update those too if needed

**When you add or update a scoped rule** — new PHP standards, updated SQL conventions:
- Edit the relevant file in `.github/instructions/`
- Copy those updated rules into the relevant section in `CLAUDE.md` and `GEMINI.md`
- Copilot picks up the scoped file change automatically

It's not a fully automated system — there are a couple of deliberate manual copy steps. But they're small and keep you aware of exactly what each tool is reading, rather than hiding it behind automation that might silently break.

---

## SECTION 8 — The Test (10:00 – 11:15)

Let's verify the whole setup is working. Send this exact prompt to all three tools — Copilot chat, Claude Code, and Gemini CLI:

---

> **Prompt:**
>
> "Create a PHP function called `get_user_by_id` that accepts a user ID as an integer, queries the `users` table in the database, and returns the user's data as an associative array. Follow our project's coding standards."

---

All three tools should give you a response that:

- Uses **PDO** with a **prepared statement** and a named placeholder like `:user_id`
- Includes a **docblock comment** above the function
- Uses **meaningful variable names** — `$userId`, `$statement`, not `$s` or `$id`
- Is clear and readable, not unnecessarily clever

The fact that all three tools produce a consistent, standards-compliant response — from the same prompt, without any extra explanation — is proof that your custom instructions are working.

Each tool got there differently. Copilot used its always-on file and scoped loading. Claude and Gemini read their flat files and the shared agent definition. Different mechanisms, same outcome.

---

## OUTRO (11:15 – 12:00)

And that's it — one source of truth, three AI tools, all reading from the same project context.

To recap what we built:

- **AGENTS.md** — your single source of truth for project context
- **`.github/copilot-instructions.md`** — a copy of AGENTS.md that Copilot reads as its always-on file
- **`.github/instructions/`** — scoped files that Copilot loads automatically based on what you're working on
- **`CLAUDE.md` and `GEMINI.md`** — each contains the project context plus scoped rules, with a reference to the shared agent definition
- **`.vscode/agents/sync-instructions.md`** — the shared agent definition where detailed conventions live, readable by both Claude and Gemini
- **DRY** — the principle tying it all together: one piece of information, one place to maintain it

It requires a small amount of manual upkeep when things change. But the payoff is consistency — all three tools understand your stack, your standards, and your project goals from day one.

If you found this useful, hit like and subscribe — it really does help the channel grow. Drop a comment if you set this up in your own project or if you have questions while following along.

See you in the next one.

---

*End of Transcript*
