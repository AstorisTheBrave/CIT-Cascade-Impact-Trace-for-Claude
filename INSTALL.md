# Installing CIT: Complete Guide for Windows, Mac, and Linux

Find your platform and follow the steps in order. Everything from first download to confirming CIT is working is covered here.

---

## What You Need Before Starting

- A Claude account on **Pro, Max, Team, or Enterprise** plan. Skills are not available on the free tier.
- One of these: Claude Desktop, claude.ai in a browser, or Claude Code in a terminal.

Not sure which one you have? If you go to claude.ai in a browser, that's the web version. If you downloaded a standalone app to your computer, that's Claude Desktop. If you have run `claude` as a terminal command before, that's Claude Code.

---

## Which Platform and OS Are You Using?

Jump straight to your section:

- [Claude Desktop on Mac](#claude-desktop-mac)
- [Claude Desktop on Windows](#claude-desktop-windows)
- [Claude Desktop on Linux](#claude-desktop-linux)
- [claude.ai in a browser (all platforms)](#claudeai-browser)
- [Claude Code on Mac](#claude-code-mac)
- [Claude Code on Windows](#claude-code-windows)
- [Claude Code on Linux](#claude-code-linux)

---

## Download the Files

### For Claude Desktop or claude.ai (ZIP files)

Download the individual skills you need. Find them in the `downloads/` folder of this repo, or click below:

- [cit-config.zip](./downloads/cit-config.zip) - Shared config, recommended first install
- [cit-chat.zip](./downloads/cit-chat.zip) - CIT for conversations
- [cit-code.zip](./downloads/cit-code.zip) - CIT for coding
- [cit-design.zip](./downloads/cit-design.zip) - CIT for design work

Or download everything at once: click the green **Code** button at the top of the repo page, then **Download ZIP**. The skill zips are inside the `downloads/` folder.

### For Claude Code (folder-based)

Clone the repo or download the ZIP and unzip it anywhere you can find it.

```bash
git clone https://github.com/AstorisTheBrave/CIT-Cascade-Impact-Trace-for-Claude.git
```

---

<a name="claude-desktop-mac"></a>
## Claude Desktop on Mac

### Step 1: Install Claude Desktop (skip if already installed)

1. Go to [claude.ai/download](https://claude.ai/download)
2. Download the macOS version
3. Open the `.dmg` file
4. Drag Claude to your Applications folder
5. Open Claude from Applications and sign in

### Step 2: Enable Skills

1. Open the Claude Desktop app
2. Click the settings icon (bottom left of the sidebar)
3. Go to **Customize** then **Skills**
4. If you see a toggle to enable Skills, turn it on

### Step 3: Upload the Skill Files

1. In the Skills section, click **Add skill** or the **+** button
2. Upload `cit-config.zip` first
3. Repeat for each skill: `cit-chat.zip`, `cit-code.zip`, `cit-design.zip`
4. Make sure each skill is toggled **on** after uploading

### Step 4: Test It

Open a new conversation and type:
```
cit setup
```
Claude should ask you three setup questions. If it does, you're good to go.

**Troubleshooting:**
- If you don't see the Skills section, check your plan at [claude.ai/settings/billing](https://claude.ai/settings/billing). Skills require Pro or higher.
- If a skill uploads but doesn't respond, toggle it off and back on then start a fresh conversation.
- If your Mac asks whether to trust the file when opening the DMG, click Open. This is normal for apps not downloaded from the App Store.

---

<a name="claude-desktop-windows"></a>
## Claude Desktop on Windows

### Step 1: Install Claude Desktop (skip if already installed)

1. Go to [claude.ai/download](https://claude.ai/download)
2. Download the Windows version (`.exe` installer)
3. Run the installer. If Windows Defender shows a warning, click **More info** then **Run anyway**. This is expected for new apps.
4. Follow the installer steps
5. Launch Claude from the Start menu and sign in

### Step 2: Enable Skills

1. Open the Claude Desktop app
2. Click the settings icon (usually bottom left or accessible from the menu)
3. Go to **Customize** then **Skills**
4. Turn on Skills if there is a toggle to enable them

### Step 3: Upload the Skill Files

1. Click **Add skill** or the **+** button
2. Upload `cit-config.zip` first
3. Repeat for each skill: `cit-chat.zip`, `cit-code.zip`, `cit-design.zip`
4. Make sure each skill is toggled **on**

### Step 4: Test It

Open a new conversation and type:
```
cit setup
```

**Troubleshooting:**
- If Windows Defender or your antivirus blocks the installer, this is common with new apps. Check your antivirus quarantine folder and allow the installer.
- Skills require a Pro plan or higher. If the Skills section is missing, check your plan.
- After uploading, start a fresh conversation. Skills load at the beginning of a session.

---

<a name="claude-desktop-linux"></a>
## Claude Desktop on Linux

Claude Desktop is not available for Linux at this time. This is a platform limitation from Anthropic, not a CIT issue.

Your options on Linux are:

- **claude.ai in a browser** - works on any Linux distro with a modern browser. See the [claude.ai section](#claudeai-browser) below.
- **Claude Code** - fully supported on Linux. See the [Claude Code on Linux section](#claude-code-linux) below.

---

<a name="claudeai-browser"></a>
## claude.ai in a Browser (Mac, Windows, Linux)

This works the same on all three operating systems. Any modern browser works: Chrome, Firefox, Safari, Edge.

### Step 1: Upload the Skills

1. Go to [claude.ai](https://claude.ai) and sign in
2. Click your profile icon in the top right corner
3. Go to **Settings**
4. Click **Customize** then **Skills**
5. Click **Add skill** or the **+** button
6. Upload `cit-config.zip` first
7. Repeat for each skill: `cit-chat.zip`, `cit-code.zip`, `cit-design.zip`
8. Make sure each skill is toggled **on**

### Step 2: Test It

Start a new conversation and type:
```
cit setup
```

**Troubleshooting:**
- Skills require Pro or higher. If you don't see the Skills section, check your plan at [claude.ai/settings/billing](https://claude.ai/settings/billing).
- Skills load at the start of a session. If you just installed one, open a fresh conversation.
- CIT-Chat rules won't persist between conversations unless Memory is on. Go to Settings and make sure Memory is enabled.

---

<a name="claude-code-mac"></a>
## Claude Code on Mac

### Step 1: Install Claude Code (skip if already installed)

The easiest way is the native installer:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Or with Homebrew:

```bash
brew install claude-code
```

Verify it worked:
```bash
claude --version
```

Then authenticate:
```bash
claude
```
Follow the browser prompts to sign in.

### Step 2: Create the Skills Folder

```bash
mkdir -p ~/.claude/skills
```

### Step 3: Copy the CIT Skill Folders

From inside the cloned or downloaded CIT repo:

```bash
cp -r cit-config ~/.claude/skills/
cp -r cit-chat ~/.claude/skills/
cp -r cit-code ~/.claude/skills/
cp -r cit-design ~/.claude/skills/
```

Confirm it worked:

```bash
ls ~/.claude/skills/
# Should show: cit-config  cit-chat  cit-code  cit-design
```

Each folder must contain a `SKILL.md` file directly inside it. Double check:

```bash
ls ~/.claude/skills/cit-code/
# Should show: SKILL.md
```

### Step 4: Test It

Start a new Claude Code session in your terminal and type:
```
cit setup
```

**Troubleshooting:**
- If `claude` is not found after install, close and reopen your terminal. The PATH update needs a fresh shell.
- If skills aren't loading, make sure each skill folder contains `SKILL.md` directly. A common mistake is nesting: `~/.claude/skills/cit-code/cit-code/SKILL.md` won't work. The path must be `~/.claude/skills/cit-code/SKILL.md`.
- Permission errors: run `chmod -R 755 ~/.claude/skills/` to fix.

---

<a name="claude-code-windows"></a>
## Claude Code on Windows

Claude Code runs best on Windows through **WSL2** (Windows Subsystem for Linux). This gives you a full Linux terminal inside Windows and avoids compatibility issues with file paths, line endings, and shell commands. The steps below cover WSL2 setup if you don't have it yet.

If you already have WSL2 and Ubuntu installed, skip to Step 3.

### Step 1: Install WSL2 (run this in PowerShell as Administrator)

Open PowerShell as Administrator: right click the Start button, click **Windows PowerShell (Admin)** or **Terminal (Admin)**.

```powershell
wsl --install -d Ubuntu
```

Restart your computer when prompted. After restarting, open **Ubuntu** from the Start menu. You will be asked to create a Linux username and password. These are separate from your Windows login.

### Step 2: Install Claude Code inside Ubuntu

Open your Ubuntu terminal and run:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Verify:
```bash
claude --version
```

Authenticate:
```bash
claude
```
Follow the browser prompts to sign in with your Claude account.

### Step 3: Create the Skills Folder

Inside your Ubuntu terminal:

```bash
mkdir -p ~/.claude/skills
```

### Step 4: Get the CIT Files into Ubuntu

**Option A: Clone directly in Ubuntu (recommended)**

```bash
cd ~
git clone https://github.com/AstorisTheBrave/CIT-Cascade-Impact-Trace-for-Claude.git
cd CIT-Cascade-Impact-Trace-for-Claude
```

**Option B: Downloaded the ZIP to Windows first**

Your Windows Downloads folder is accessible from Ubuntu at `/mnt/c/Users/YourWindowsUsername/Downloads/`. Unzip from there:

```bash
cd /mnt/c/Users/YourWindowsUsername/Downloads/
unzip CIT-Cascade-Impact-Trace-for-Claude-main.zip
cd CIT-Cascade-Impact-Trace-for-Claude-main
```

Replace `YourWindowsUsername` with your actual Windows username.

### Step 5: Copy the Skill Folders

```bash
cp -r cit-config ~/.claude/skills/
cp -r cit-chat ~/.claude/skills/
cp -r cit-code ~/.claude/skills/
cp -r cit-design ~/.claude/skills/
```

Confirm:

```bash
ls ~/.claude/skills/
# Should show: cit-config  cit-chat  cit-code  cit-design
```

### Step 6: Test It

In your Ubuntu terminal, type:
```
cit setup
```

**Troubleshooting:**
- If `claude` gives a "command not found" error after install, run `source ~/.bashrc` to reload your shell config, then try again.
- If you get permission errors copying files, run `chmod -R 755 ~/.claude/skills/`.
- Skills must live in the WSL filesystem (`~/.claude/skills/`), not on the Windows drive (`/mnt/c/...`). Storing them on `/mnt/c/` will work but is slower.
- Double-check nesting: the path must be `~/.claude/skills/cit-code/SKILL.md`, not `~/.claude/skills/cit-code/cit-code/SKILL.md`.

---

<a name="claude-code-linux"></a>
## Claude Code on Linux

### Step 1: Install Claude Code (skip if already installed)

**Using the install script (works on most distros):**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Using apt (Debian/Ubuntu):**
```bash
curl -fsSL https://downloads.claude.ai/claude-code/apt/claude-code.asc | sudo tee /etc/apt/keyrings/claude-code.asc
echo "deb [signed-by=/etc/apt/keyrings/claude-code.asc] https://downloads.claude.ai/claude-code/apt stable main" | sudo tee /etc/apt/sources.list.d/claude-code.list
sudo apt update && sudo apt install claude-code
```

**Using dnf (Fedora/RHEL):**
```bash
sudo dnf config-manager --add-repo https://downloads.claude.ai/claude-code/dnf/claude-code.repo
sudo dnf install claude-code
```

Verify:
```bash
claude --version
```

Authenticate:
```bash
claude
```

### Step 2: Create the Skills Folder

```bash
mkdir -p ~/.claude/skills
```

### Step 3: Copy the Skill Folders

From inside the cloned or downloaded CIT repo:

```bash
cp -r cit-config ~/.claude/skills/
cp -r cit-chat ~/.claude/skills/
cp -r cit-code ~/.claude/skills/
cp -r cit-design ~/.claude/skills/
```

Confirm:

```bash
ls ~/.claude/skills/
# Should show: cit-config  cit-chat  cit-code  cit-design
```

### Step 4: Test It

Start a new Claude Code session and type:
```
cit setup
```

**Troubleshooting:**
- If `claude` is not found after the install script, close and reopen your terminal, or run `source ~/.bashrc`.
- Permission errors: `chmod -R 755 ~/.claude/skills/`
- If you installed via apt or dnf, updates won't be automatic. Run `sudo apt upgrade claude-code` or `sudo dnf upgrade claude-code` to update.
- Note: Linux binaries are not individually code-signed when downloaded directly. If you used apt, dnf, or apk, your package manager verifies the signature automatically.

---

## First-Time Setup

After installing on any platform, start a new conversation and type:

```
cit setup
```

Claude will ask three questions:

**1. Visibility** - Show traces or run silently?
Recommended for first-time users: show them. You can see it working and verify the cascade maps look right before trusting them.

**2. Format** - Tree or Domino chain?
Tree handles complex webs better. Domino is more visual but switches to Tree automatically beyond 8 nodes. You can change this later with `cit settings`.

**3. Interrupt behavior** - If you type `cit off` mid-task, finish the current trace first or stop immediately?
Finishing is safer. Stopping is faster but the action proceeds without a complete trace.

Your answers are saved. You won't be asked again unless you type `cit settings`.

---

## Useful Commands

| Command | What it does |
|---|---|
| `cit setup` | Run first-time setup |
| `cit status` | Check if CIT is on or off |
| `cit settings` | Review or change your preferences |
| `cit off` | Disable for this conversation |
| `cit on` | Re-enable |
| `cit rules` | See your saved Persistent User Rules |

---

## Installing Only Some Skills

You don't have to install all four. Each works on its own.

- Coding only: `cit-config` + `cit-code`
- Conversations: `cit-config` + `cit-chat`
- Design work: `cit-config` + `cit-design`

Install `cit-config` first and any other skill you add later will automatically pick up your settings.

---

## Keeping CIT Updated

**Claude Desktop and claude.ai:** Re-upload the new zip files through Settings > Skills. Your config is not affected.

**Claude Code:** Pull the latest repo and copy the updated folders to `~/.claude/skills/`, replacing the old ones. Your CLAUDE.md config and architectural invariants are not affected.

---

## Want to Understand the Full Design?

Read [BLUEPRINT.md](./BLUEPRINT.md). It covers every design decision, what was tried and rejected, and the honest limitations of each choice. If you want to contribute, start there.
