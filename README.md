# VS Code Set-up With Custom Instructions  - For Multiple AI Agents
*Trainer: Dele Oke*
---
![VS Code Logo](notes/vscode.svg)



## Objectives
This project serves as a template for setting up VS Code workspace custom instructions for multiple AI assistants.

## Important Terminology

**Visual Studio Code** is a source-code editor that Microsoft developed for Windows, Linux, and macOS. It supports debugging, syntax highlighting, and intelligent code completion. Users can customise the editor with extensions to add new languages and tools.

**AI assistants** are software systems that use artificial intelligence to help people complete tasks, answer questions, generate content, analyse information, and automate parts of their work.
Examples include:

- ChatGPT
- GitHub Copilot
- Google Gemini
- Microsoft Copilot
- Claude (Anthropic)
- Gemini

**Large Language Models** (LLMs) are AI systems trained on very large amounts of text so they can understand and generate human-like language. They learn patterns in words, sentences, and context. This allows them to answer questions, summarise information, write content, explain ideas, and assist with tasks such as coding.
Examples include:

- GPT-4o (OpenAI)
- Gemini (Google)
- Claude (Anthropic)

**Tokens and Tokenisation** are the foundation of how language models read and process text. Tokenisation is the process of breaking text into smaller units called tokens. For example, the word "tokenisation" may split into three tokens: "token", "isa", and "tion". Models measure their input and output length in tokens, not words.

**Context Window** is the maximum amount of text a language model can process in a single interaction. It includes your input, any prior conversation history, and the model's response. When the total number of tokens exceeds this limit, the model can no longer access the earlier content. Larger context windows allow the model to handle longer documents and more complex, multi-step conversations.

**Markdown** a lightweight plain text format for writing structured content. It lets you add elements like headings, lists, links, and emphasis with simple symbols. It is te main language that AI understands when creating instructions for it.[markdown Guide](https://www.markdownguide.org/cheat-sheet/)

**Prompts** are the specific instructions or questions users give an AI to perform a particular task. They tell the AI *what* to do and are written in natural language.

**Vibe Coding** was first used by Andrej Karpathy in a [post on X](https://x.com/karpathy/status/1886192184808149383?lang=en) on 2 Feb 2025. It has since evolved to reflect something more substantial. [Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/introduction-vibe-coding/1-introduction) defines Vibe Coding as:
> Vibe coding is an innovative approach to software development that leverages AI tools to generate applications.

## BASIC VS CODE CUSTOM INSTRUCTIONS

```
Workspace-Name/
├── AGENTS.md                              ← source of truth (you edit this)
│
├── CLAUDE.md                              ← references the agent (you edit this)
├── GEMINI.md                              ← references the agent (you edit this)
├── prds/                                  ← project requirement docs
├── notes/                                 ← notes and learning materials
├── workarea/                              ← sample and experimental files
│
└── .github/
    ├── copilot-instructions.md           ← copy of AGENTS.md (you edit this)
    └── instructions/
        ├── writing.instructions.md       ← applyTo: ** (you edit this)
        ├── php.instructions.md           ← applyTo: **/*.php (you edit this)
        └── database.instructions.md     ← applyTo: **/*.sql (you edit this)
```
All **Custom Instructions** in  VS Code can be grouped under two headings

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



## References

**VS Code**

- [Use custom instructions in VS Code](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)

- [Best practices for using AI in VS Code](https://code.visualstudio.com/docs/copilot/best-practices)

**Claude Code**

- [Claude Code terminal installation](https://code.claude.com/docs/en/setup#system-requirements)

- [Claude Code VS Code Installation](vscode:extension/anthropic.claude-code)

- [Claude Code Docs - CLAUDE.md](https://code.claude.com/docs#customize-with-instructions-skills-and-hooks)

- [Claude Code in the Terminal](https://code.claude.com/docs/en/quickstart)

**Gemini CLI**

- [Gemini CLI Installation](https://geminicli.com/docs/get-started/installation/)

- [Gemini CLI documentation](https://geminicli.com/docs/)

- [Gemini CLI - GEMINI.md](https://geminicli.com/docs/cli/gemini-md/)

 