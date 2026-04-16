# Creating Custom Instructions for GitHub Copilot

**Trainer: Dele Oke**

![GitHub Copilot](Copilot_Icon_White.svg)



## 1. Custom Instructions - AGENTS.md, copilot-instructions.md ❓

`/init`

This ideally should be the first thing you build.  
Create project-wide rules and conventions with the AI.   
Always-on instructions apply to every request  - `AGENTS.md`;  `.github/copilot-instructions.md`, `CLAUDE`, `GEMINI` 

---

## 2. File based instructions -  ❓

`/create-instructions`

For .github/instructions/*.instructions.md
These are file-based instructions target specific file types or folders. 
They are scope loaded

```bash
/create-instructions Create a file-based custom instruction for HTML that requires all CSS to be in linked .css files and all JavaScript to be in linked .js files only. Place the JavaScript <script> in the <head> with the defer attribute, and require semantic HTML structure such as <header>, <footer>.
```
---

## 3. Prompt files - ❓

 `/create-prompt `

For lightweight, repeatable, single-task prompts. Like explaining code to a beginner or scaffolding a component. 

```bash
/create-prompt Create a prompt that explains the selected code to an absolute beginner by using relatable real-world analogies and avoiding complex technical jargon. The prompt should instruct the AI to act as a patient coding tutor and break down the logic step-by-step.
```
---

## 4. Custom Agents -  ❓

`/create-agent`

Adopt specific personas, such as security reviewer, database admin, or planner. Each agent defines its own behavior, available tools, and language model preferences. Choose different language models for different tasks.

```bash
/create-agent Act as a read-only Code Reviewer evaluating the provided workspace or selected code for quality, security, and best practices. Do not modify any files.

Analyze the code and categorize your findings strictly under the following four headings:
Critical: Must be addressed (security flaws, breaking bugs).
Warning: Significant room for improvement (performance issues, bad practices).
Info: Suggested best practices and minor enhancements.
Good: Well-written and maintainable code.

To ensure another AI agent can easily parse and implement these findings, output the results as a clean Markdown list under each heading, providing the file path, line number, and a concise action item for each point.
```
---

## 5. Agent skill -  ❓

`/create-skills`

Agent Skills are folders of instructions, scripts, and resources that GitHub Copilot can load when relevant to perform specialized tasks. Agent Skills is an open standard that works across multiple AI agents, including GitHub Copilot in VS Code, GitHub Copilot CLI, and GitHub Copilot cloud agent.

```bash
/create-skill
Create a skill that triggers when a user asks to build a web page, component, or layout from an attached image, mockup, or wireframe. The skill should analyze the visual input and generate the corresponding HTML and CSS. Crucial rule: For any image in the design where no asset is provided, generate a placeholder URL from https://placehold.co/ by determining the appropriate width, height, and color hex codes from the mockup. The format must be: <img src="https://placehold.co/{width}x{height}/{background_hex}/{text_hex}/png" alt="{description}">.

```

## References

- [Slash commands to create custom instructions](https://code.visualstudio.com/docs/copilot/customization/overview)