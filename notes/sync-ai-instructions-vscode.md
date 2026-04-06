# How to Use Custom Instructions in VS Code for Multiple AI Agents
Sync GitHub Copilot, Claude and Gemini

**Trainer: Dele Oke**
---

## INTRODUCTION

Custom instructions inform the AI tool about the project, preferences, and coding standards. The tool uses this context automatically in every response. This saves time and avoids repetition.

Custom instructions produce consistent and relevant AI responses. They ensure **every tool you use understands the project** from the start.

---

## BASIC VS CODE CUSTOM INSTRUCTIONS

`
my-project/
+-- AGENTS.md                              ? source of truth (you edit this)
¦
+-- CLAUDE.md                              ? references the agent (you edit this)
+-- GEMINI.md                              ? references the agent (you edit this)
¦
+-- .github/
    +-- copilot-instructions.md           ? copy of AGENTS.md (you edit this)
    +-- instructions/
        +-- general.instructions.md       ? applyTo: ** (you edit this)
        +-- php.instructions.md           ? applyTo: **/*.php (you edit this)
        +-- database.instructions.md     ? applyTo: **/*.sql (you edit this)
`
**Always on instructions**  

The system always loads these files into the *context window* with every prompt.

- GitHub Copilot acts as your AI assistant in VS Code.
- You can select any LLM to work with it.
- GitHub Copilot looks for custom instructions by default inside .github/copilot-instructions.md.
- The AGENTS.md file provides custom instructions for other AI assistants.
- Direct copilot-instructions.md to use the AGENTS.md file layout to avoid repetition.
- This follows the DRY principle (Don't Repeat Yourself).

**Scope loaded instructions**

The system only loads scope loaded instructions into the context window when relevant files are active. For example:

- In .github/instructions/writing.instructions.md, the AI assistant only loads instructions when you edit Markdown files.

## Setting Up Claude and Gemini 

You might use multiple AI assistants over time. Claude Code and Gemini CLI are popular alternatives to GitHub Copilot.

- Claude and Gemini read custom instructions from GEMINI.md and CLAUDE.md. The root folder of the project holds both files.
- Direct these files to read instructions from the AGENTS.md file to follow the DRY principle. You can also add file based instructions inside these individual files to provide full context.
- The AGENTS.md file contains sufficient project context. It currently lacks the file based instructions located from .github/instructions. The .agents folder might resolve this limitation in the future.

## A Test Prompt

Verify the setup works. Send the prompt below to GitHub Copilot, Claude Code, and Gemini.

---

> **Prompt:**
>
> "Create a PHP function called get_user_by_id that accepts a user ID as an integer, queries the users table in the database, and returns the user's data as an associative array. Follow the coding standards of the project."

---

All tools should give a response that meets these targets:

- Uses **PDO** with a **prepared statement** and a named placeholder like :user_id.
- Includes a **docblock comment** above the function.
- Uses **meaningful variable names** like $userId, $statement, not $s or $id.
- Is clear and readable, not unnecessarily clever.

The tools produce a consistent, standards-compliant response from the same prompt, without extra explanation. This proves the custom instructions work successfully.

Each tool reaches the outcome differently. Copilot uses the always-on file and scoped loading. Claude and Gemini read their flat files and the shared agent definition. Different mechanisms provide the same result.

---

## RECAP

This approach provides one source of truth. All AI tools read from the identical project context.

To recap the setup:

- **AGENTS.md** gives the single source of truth for project context. GitHub Copilot, Claude Code and Gemini retrieve their context from here.
- **.github/instructions/** holds scoped files. Copilot loads these automatically based on active files.
- **CLAUDE.md and GEMINI.md** link directly to the project context. You can add scoped rules here.

This setup requires minimal manual upkeep when changes occur. The payoff provides consistency. All three tools understand the stack, standards, and goals from day one.

## Conclusion

If you found this guide useful, please like and subscribe to support the channel. Drop a comment if you configure this in your project or have questions.

I will see you in the next video.

---

## References

- [Use custom instructions in VS Code](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)
- [Best practices for using AI in VS Code](https://code.visualstudio.com/docs/copilot/best-practices)
