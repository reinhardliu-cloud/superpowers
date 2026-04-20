# Superpowers for GitHub Copilot in VS Code

Guide for using Superpowers with GitHub Copilot Chat in VS Code via project-local instructions, skills, and custom agents.

## What This Installs

- `.github/copilot-instructions.md` - project-wide instructions that tell Copilot when to use Superpowers workflows
- `.copilot/skills/` - project-local Superpowers skills copied from this repository
- `.github/agents/code-reviewer.agent.md` - optional local review agent used by `requesting-code-review`

This installation model is repository-local and intended to be committed to the target project, so the whole team gets the same behavior.

## Quick Install

Clone Superpowers somewhere on your machine:

```bash
git clone https://github.com/obra/superpowers.git ~/.copilot/superpowers
```

From the root of the project you want to enhance:

```bash
mkdir -p .copilot/skills .github/agents
cp -R ~/.copilot/superpowers/skills/. .copilot/skills/
cp ~/.copilot/superpowers/agents/code-reviewer.md .github/agents/code-reviewer.agent.md
```

Create `.github/copilot-instructions.md`:

```markdown
# Superpowers For This Repo

Use the project skills installed in `.copilot/skills` whenever they apply.

Priority guidance:

- Use `brainstorming` before designing new features, behavior changes, or larger refactors.
- Use `writing-plans` after design approval and before multi-step implementation.
- Use `systematic-debugging` before proposing or applying bug fixes.
- Use `test-driven-development` before writing implementation code for features and bug fixes.
- Use `requesting-code-review` and the `code-reviewer` agent in `.github/agents` after major changes.
- Use `verification-before-completion` before claiming work is done, tests pass, or a bug is fixed.

Rules:

- Check whether a skill applies before acting.
- Follow direct user instructions and repository constraints over skill defaults.
- Keep changes minimal and consistent with the existing project structure.
- Prefer targeted verification before broader test or build runs.
```

Then:

1. Open the project root in VS Code.
2. Start a new GitHub Copilot chat session.
3. If Copilot was already open before the files were added, reload the VS Code window and start a fresh chat.

Commit the copied `.copilot/skills/`, `.github/agents/`, and `.github/copilot-instructions.md` files into the target project repository, not into your local Superpowers clone.

## Windows (PowerShell)

Clone Superpowers somewhere local:

```powershell
git clone https://github.com/obra/superpowers.git $env:USERPROFILE\.copilot\superpowers
```

From the root of the target project:

```powershell
New-Item -ItemType Directory -Force -Path .copilot\skills | Out-Null
New-Item -ItemType Directory -Force -Path .github\agents | Out-Null
Copy-Item -Recurse $env:USERPROFILE\.copilot\superpowers\skills\* .copilot\skills\
Copy-Item $env:USERPROFILE\.copilot\superpowers\agents\code-reviewer.md .github\agents\code-reviewer.agent.md
```

Create `.github/copilot-instructions.md` with the same contents shown above.

## How It Works

This setup uses three different GitHub Copilot customization mechanisms together:

1. **Project instructions** in `.github/copilot-instructions.md` provide always-on repository guidance.
2. **Project-local skills** in `.copilot/skills/` make the Superpowers workflows available inside the repository.
3. **Local custom agents** in `.github/agents/` provide the review agent used by some Superpowers workflows.

The instructions file tells Copilot which workflows should be preferred. The local skills provide the actual workflow content. The local review agent fills in the code-review step used by `requesting-code-review`.

## Verification

After installation, start a new Copilot chat and ask questions that should produce repository-specific answers.

Examples:

- `In this repository, what must you do before fixing a bug?`
- `In this repository, what must you do before implementing a multi-step feature?`
- `Before claiming work is complete here, what must you do?`

Expected behavior:

- Bug-fix answers should mention `systematic-debugging`.
- Multi-step feature answers should mention `brainstorming` and `writing-plans`.
- Completion answers should mention `verification-before-completion`.

If the answer is only generic advice and does not mention the repository-specific workflows, start a fresh chat or reload the window and try again.

## Updating

Update the Superpowers clone:

```bash
cd ~/.copilot/superpowers && git pull
```

Then re-copy the project-local assets into the target repository:

```bash
rm -rf .copilot/skills
mkdir -p .copilot/skills .github/agents
cp -R ~/.copilot/superpowers/skills/. .copilot/skills/
cp ~/.copilot/superpowers/agents/code-reviewer.md .github/agents/code-reviewer.agent.md
```

If you customized `.github/copilot-instructions.md`, keep your repository-specific rules and only update them intentionally.

## Uninstalling

From the target repository root:

```bash
rm -rf .copilot/skills
rm -f .github/agents/code-reviewer.agent.md
rm -f .github/copilot-instructions.md
```

## Troubleshooting

### Instructions do not seem to load

1. Confirm the file exists at `.github/copilot-instructions.md`.
2. Open the repository root in VS Code, not a subfolder.
3. Start a new Copilot chat instead of reusing an old one.
4. Reload the VS Code window if the file was added after Copilot chat was already open.
5. Make sure `github.copilot.chat.codeGeneration.useInstructionFiles` is enabled.

### Skills do not seem to load

1. Confirm `.copilot/skills/using-superpowers/SKILL.md` exists in the target repository.
2. Confirm the skill directories were copied recursively, not just the top-level folder names.
3. Start a fresh Copilot chat after installation.
4. Update to a recent GitHub Copilot Chat build if your VS Code setup is old.

### Review agent is missing

1. Confirm `.github/agents/code-reviewer.agent.md` exists.
2. Make sure the file extension is `.agent.md`, not just `.md`.
3. Restart the chat session after adding the file.

## Getting Help

- Report issues: https://github.com/obra/superpowers/issues
- Main documentation: https://github.com/obra/superpowers
- GitHub Copilot customization docs: https://code.visualstudio.com/docs/copilot/customization/custom-instructions