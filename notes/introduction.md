# VS Code Set-up With Custom Instructions  - For Multiple AI Agents
*Trainer: Dele Oke*
---
![VS Code Logo](vscode.svg)

> This acts as the template for all courses that involve VS code. Consult the specific respository for the subject matter e.g `Python 2026`, `VS-Code-Setup-For-Agentic_coding` e.t.c

## Objectives
This project serves as a template for setting up VS Code workspace custom instructions for multiple AI assistants.
---

## Important Terminology

 **1. Visual Studio Code❓** is a source-code editor that Microsoft developed for Windows, Linux, and macOS. It supports debugging, syntax highlighting, and intelligent code completion. Users can customise the editor with extensions to add new languages and tools.

**2. AI assistants❓** are software systems that use artificial intelligence to help people complete tasks, answer questions, generate content, analyse information, and automate parts of their work.
Examples include:

- ChatGPT
- GitHub Copilot
- Google Gemini
- Microsoft Copilot
- Claude (Anthropic)
- Gemini

**3. Large Language Models❓** (LLMs) are AI systems trained on very large amounts of text so they can understand and generate human-like language. They learn patterns in words, sentences, and context. This allows them to answer questions, summarise information, write content, explain ideas, and assist with tasks such as coding.
Examples include:

- GPT-4o (OpenAI)
- Gemini (Google)
- Claude (Anthropic)

**4. Tokens and Tokenisation❓** are the foundation of how language models read and process text. Tokenisation is the process of breaking text into smaller units called tokens. For example, the word "tokenisation" may split into three tokens: "token", "isa", and "tion". Models measure their input and output length in tokens, not words.

**5. Context Window❓** is the maximum amount of text a language model can process in a single interaction. It includes your input, any prior conversation history, and the model's response. When the total number of tokens exceeds this limit, the model can no longer access the earlier content. Larger context windows allow the model to handle longer documents and more complex, multi-step conversations.

**6. Markdown❓** a lightweight plain text format for writing structured content. It lets you add elements like headings, lists, links, and emphasis with simple symbols. It is te main language that AI understands when creating instructions for it.[markdown Guide](https://www.markdownguide.org/cheat-sheet/)

**7. Prompts❓** are the specific instructions or questions users give an AI to perform a particular task. They tell the AI *what* to do and are written in natural language.

**8. Vibe Coding❓** was first used by Andrej Karpathy in a [post on X](https://x.com/karpathy/status/1886192184808149383?lang=en) on 2 Feb 2025. It has since evolved to reflect something more substantial. [Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/introduction-vibe-coding/1-introduction) defines Vibe Coding as:
> Vibe coding is an innovative approach to software development that leverages AI tools to generate applications.

**9. Online AI Assistants & Command Line Interface Tools (CLI)❓** Online AI Assistants are becoming very powerful BUT cannot replace Desktop based IDE's yet. Examples are
Github Copilot is the AI Assitant and the LLM's are supplied by:
- OpenAI (GPT family e.g GPT-4) - ChatGPT
- Anthropic (Claude family e.g Claude 2) - claude.ai
- Google (Gemini family) - Google AI Studio
- Replit: Replit is a cloud-based development platform where you can build, run, and deploy software entirely in your browser — no local setup needed. 

AI Assistants like [Claude Code](https://code.claude.com/docs/en/setup) and [Gemini CLI](https://github.com/google-gemini/gemini-cli) Can run in the command line. They alos intergrate into VS Code with `Claude Code for VS Code Extension` and `Gemini CLI Companion VS Code extension`respectively. Install [node.js](https://nodejs.org/en/download) before installing your CLI's.

**10. Overview of AI Agents in VS Code**
- Install Github Copilot Chat (Extension)
- load TechER (Extension)
- Load Live Server (Extension)
- Create basic HTML file in workarea
- Explore existing Custom files and setup
- use prompt to create an HTML form that submits content to Local server and displays it on next page. Save inside project folder
- Create an html page from prds/prd3_sketch.md
- Explain markdown files

**11. Creating a local Git repository (optional)**

[Installing and Creating Git repository](git_repository.md)
---

## BASIC VS CODE CUSTOM INSTRUCTIONS

All **Custom Instructions❓** in  VS Code can be grouped under two headings

 **Always on instructions**  

The system always loads these files into the *context window* with every prompt.
They can be saved for `workspace`(.github/copilot-instructions.md) or `user`(~/.copilot/instructions).

- GitHub Copilot looks for custom instructions by default inside `.github/copilot-instructions.md`.
- The `AGENTS.md` file provides custom instructions for other AI assistants.
- Direct `copilot-instructions.md` to use the `AGENTS.md `file layout to avoid repetition.
- This follows the `DRY principle` (Don't Repeat Yourself).

**Scope loaded instructions**

The system only loads scope loaded instructions into the context window when relevant files are active. For example:

- In `.github/instructions/writing.instructions.md`, the AI assistant only loads instructions when you edit Markdown files.

### Creating Custom Instructions

*Prompt for custom instructions* 

`/init create a custom instructions for this project`

--

`/create-instructions  Create a file-based custom instruction for HTML that requires all CSS to be in linked .css files and all JavaScript to be in linked .js files only. Place the JavaScript <script> in the <head> with the defer attribute, and require semantic HTML structure such as <header>, <footer>.`



---
## File Structure

```
Workspace-Name/
├── .agents/                               ← global agent skills for all AI models
├── large_assets/                          ← large files not uploaded to GitHub
├── notes/                                 ← notes and learning materials
├── prds/                                  ← project requirement docs
├── workarea/                              ← sample and experimental files
│
└── .github/
    ├── copilot-instructions.md           ← copy of AGENTS.md (you edit this)
    ├── agents/                            ← VS Code agent definition files
    └── instructions/
        ├── writing.instructions.md       ← applyTo: **/*.md (you edit this)
        └── php.instructions.md           ← applyTo: **/*.php (you edit this)
```
---

## References
[References](references.md)

---
## Git
[Creating Local Git Repository](git_repository.md)

 +++