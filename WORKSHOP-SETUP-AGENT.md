# Workshop setup handover for AI assistants

This file is for the AI assistant helping a first-time participant prepare for the workshop. The participant may never have used a terminal. Your job is to make setup predictable, safe, and verifiable.

## Non-negotiable operating rules

1. Follow the setup order in this file. Do not offer alternative installation methods unless the documented method is blocked.
2. Give one action at a time. Explain where to click or where to paste before asking the participant to act.
3. Keep commands, expected output, and explanation visually separate.
4. Never turn prose, a shell prompt, a version label, or expected output into a command.
5. Never invent a command from a screenshot or web page. Use the exact commands in this file.
6. Before executing a command, repeat the exact command and state what it will do. Ask for confirmation if the assistant's execution settings require it.
7. After execution, read the actual output. Do not claim success from what a website says should happen.
8. Do not make broad system changes, install Docker, alter `PATH`, change PowerShell execution policy, or use administrator access unless the documented installer requires an operating-system confirmation.
9. Never run `npm audit fix`, `npm audit fix --force`, or make dependency upgrades during participant setup. The repository owner maintains the lockfile.
10. If a command fails, quote the exact command that ran and the decisive error line. Give one safe next step; do not start a chain of speculative fixes.

## Commands and output are different things

When this file says:

```text
Command:
node --version

Example output:
v24.21.0
```

the only text to enter in the terminal is:

```powershell
node --version
```

`run node v24.21.0`, `node v24.21.0`, and `PS C:\...>` are not valid commands. Never generate or execute them.

## Required setup order

### 1. Confirm the environment

- Identify Windows or macOS and the terminal in use.
- Ask which numbered setup step the participant is following.
- If they provide a screenshot, transcribe the command and error exactly before diagnosing it.

### 2. Install Node.js LTS manually

- Direct the participant to <https://nodejs.org/en/download>.
- They must choose the current **LTS** release and the prebuilt installer for their operating system.
- On Windows, choose **Windows Installer (.msi)**. On macOS, choose **macOS Installer (.pkg)**.
- Do not use or reproduce the Docker command block on the Node.js page.
- Let the participant operate the installer. Do not substitute a package manager or scripted install.
- Close old terminals and open a new one after installation.

Verify with exactly:

```powershell
node --version
```

Success is Node 24, or Node 22.12 or newer. The version number is output, not input.

### 3. Install and verify Git

- Use <https://git-scm.com/downloads> and the standard installer when Git is absent.
- Verify with exactly:

```powershell
git --version
```

### 4. Create the GitHub and Vercel accounts

- The participant completes browser sign-in and account permissions.
- Never ask them to paste a password, access token, recovery code, or other secret into chat.

### 5. Create and clone the workshop repository

- Create a repository from <https://github.com/BigChrisCooke/workshop-template> using **Use this template**.
- Clone the participant's new repository through VS Code's **Git: Clone** command.
- Confirm the cloned project itself is open, not its parent folder.
- Confirm `package.json`, `package-lock.json`, `CLAUDE.md`, and `brand.md` exist before continuing.

### 6. Install the locked project dependencies

From the cloned project folder, use exactly:

```powershell
npm ci
```

On Windows only, if PowerShell says `npm.ps1` cannot be loaded because script execution is disabled, use this equivalent command instead:

```powershell
npm.cmd ci
```

Do not change PowerShell execution policy for this workshop.

`npm ci` may print funding notices or an audit summary. Those lines do not mean installation failed. Success means the process exits normally without an `npm ERR!` failure.

If an audit reports any high or critical vulnerabilities, tell the participant the repository needs a maintainer update and notify Big Chris. Do not run either form of `npm audit fix`.

### 7. Install the matching Playwright browser

Use the project's installed Playwright version:

```powershell
npx playwright install chromium
```

If PowerShell blocks `npx.ps1`, use:

```powershell
npx.cmd playwright install chromium
```

Do not use `npx playwright@latest`; its browser version may not match the lockfile.

### 8. Prove the project works

Build with exactly:

```powershell
npm run build
```

If PowerShell blocks `npm.ps1`, use:

```powershell
npm.cmd run build
```

Only continue after the build reports completion without an error.

Start the local preview with exactly:

```powershell
npm run dev
```

Open the `localhost` URL printed by the terminal. Explain that the terminal remains busy while the server is running. After the participant confirms the page loads, stop the server with `Ctrl + C`.

## Setup completion gate

Do not say the participant is ready until all of these are true:

- Node is 22.12 or newer.
- Git responds with a version.
- The correct cloned project is open.
- `npm ci` completed.
- Playwright Chromium installed from the project version.
- `npm run build` completed successfully.
- The site loaded at the printed `localhost` URL.
- No unresolved high or critical audit findings remain.

If any item is unverified, state which item remains and the single next action.
