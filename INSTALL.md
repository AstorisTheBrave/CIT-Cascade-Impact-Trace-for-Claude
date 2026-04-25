# Installing CIT: Step by Step

This guide walks you through everything from downloading the files to confirming CIT is working. Pick your platform and follow the steps in order.

---

## Before You Start

CIT has two versions depending on how you use Claude:

- **Claude Code** - if you use Claude through the terminal (the `claude` CLI). This is the most reliable version with full persistence.
- **claude.ai** - if you use Claude through the website or mobile app.

Not sure which one you have? If you have a terminal and have run `claude` as a command before, you have Claude Code. If you use Claude at claude.ai in your browser, that's the web version.

You can also use both. CIT works on each independently.

---

## Step 1: Download the Files

### Option A: Download the ZIP (easiest)

1. Go to [github.com/AstorisTheBrave/CIT-Cascade-Impact-Trace-for-Claude](https://github.com/AstorisTheBrave/CIT-Cascade-Impact-Trace-for-Claude)
2. Click the green **Code** button near the top right
3. Click **Download ZIP**
4. Unzip the file somewhere you can find it (your Downloads folder is fine)

### Option B: Clone with Git (if you're comfortable with the terminal)

```bash
git clone https://github.com/AstorisTheBrave/CIT-Cascade-Impact-Trace-for-Claude.git
```

---

## Step 2: Install on Claude Code

Claude Code reads skills from a folder on your computer. You need to put the CIT skill folders there.

### Find or create your Claude skills folder

Open your terminal and run:

```bash
ls ~/.claude/
```

You should see a folder called `skills` inside. If it doesn't exist yet, create it:

```bash
mkdir -p ~/.claude/skills
```

### Copy the skill folders

From inside the downloaded/cloned CIT folder, copy all four skill folders:

```bash
cp -r cit-config ~/.claude/skills/
cp -r cit-chat ~/.claude/skills/
cp -r cit-code ~/.claude/skills/
cp -r cit-design ~/.claude/skills/
```

To confirm they copied correctly:

```bash
ls ~/.claude/skills/
```

You should see: `cit-config  cit-chat  cit-code  cit-design`

### That's it for installation

Claude Code automatically picks up skills from that folder. No restart needed.

---

## Step 3: Install on claude.ai

Claude.ai skills are uploaded directly through the Claude interface.

1. Open [claude.ai](https://claude.ai) in your browser
2. Click your profile icon in the top right corner
3. Go to **Settings**
4. Look for the **Skills** section (sometimes listed under **Customization** or **Extensions** depending on your plan)
5. Click **Add skill** or **Upload skill**
6. Upload each `SKILL.md` file one at a time. You'll find them inside the folders you downloaded:
   - `cit-config/SKILL.md`
   - `cit-chat/SKILL.md`
   - `cit-code/SKILL.md`
   - `cit-design/SKILL.md`

> **Note:** The Skills feature requires a Claude Pro or higher plan on claude.ai. If you don't see the Skills section in settings, check your plan at [claude.ai/settings/billing](https://claude.ai/settings/billing).

---

## Step 4: First-Time Setup

Once the skills are installed, start a new conversation with Claude and say:

```
cit setup
```

Claude will ask you three questions:

1. **Visibility** - Should traces be shown to you, or run silently in the background? Shown is recommended if you're new to CIT so you can see it working.

2. **Format** - Tree view or Domino chain? Tree is easier to read for complex webs. Domino is more visual. You can change this later.

3. **Interrupt behavior** - If you type `cit off` mid-task, should Claude finish the current trace first, or stop immediately? Finishing is safer. Stopping immediately is faster.

Claude will save your answers and you won't be asked again.

---

## Step 5: Test That It's Working

### Quick test for CIT-Chat

Start a conversation and say something like:

```
I'm building a todo app. I only have 3 days and no budget for paid services.
```

Then follow up with:

```
Actually I think we should use Firebase for the database.
```

CIT-Chat should trace the cascade impact on your stated constraints (3 days, no budget) before giving you a recommendation. If it does, it's working.

### Quick test for CIT-Code

Open a project in Claude Code and ask:

```
I want to update the user settings function. Run a CIT trace before touching anything.
```

Claude should classify the triage level, map the cascade across files, flag any blind spots, and only then propose changes. If you get the structured trace output with tiers labeled, it's working.

### Quick test for CIT-Design

In a project with any component file, ask:

```
I want to change the primary color token. Run a CIT trace first.
```

Claude should build a token ownership map, trace affected components, flag state and breakpoint risks, and check contrast before making any changes.

---

## Useful Commands

Once CIT is installed, these commands work in any conversation:

| Command | What it does |
|---|---|
| `cit status` | Check if CIT is on or off |
| `cit settings` | Review or change your setup |
| `cit off` | Turn CIT off for this conversation |
| `cit on` | Turn it back on |
| `cit rules` | See your saved Persistent User Rules (CIT-Chat) |

---

## Installing Individual Skills

You don't have to install all four. Each skill works on its own.

- **Just using Claude Code for coding?** Install `cit-config` and `cit-code`.
- **Just using claude.ai for chat?** Install `cit-config` and `cit-chat`.
- **Working on UI/UX?** Install `cit-config` and `cit-design`.

If you install `cit-config` first, any other CIT skill you add later will automatically pick up your settings. If you skip `cit-config`, each skill will run its own setup the first time and create a shared config that later skills will find.

---

## Something Not Working?

**Claude doesn't seem to know about CIT after install:**
- On Claude Code: double check the skill folders are in `~/.claude/skills/` and each folder contains a `SKILL.md` file. Run `ls ~/.claude/skills/cit-code/` to confirm.
- On claude.ai: try starting a fresh conversation. Skills load at the start of a session.

**CIT-Chat rules aren't persisting between conversations:**
- Go to claude.ai Settings and confirm that Memory is turned ON. CIT-Chat relies on Claude's memory system to save your rules between sessions. Without memory enabled, rules reset each conversation.

**The trace isn't showing up:**
- Type `cit status` to check if CIT is enabled
- Type `cit settings` to check if `CIT_SHOW` is set to ON
- If it's set to OFF, your traces are running silently. Type `cit settings` and switch to ON.

**You're on claude.ai and don't see a Skills section in settings:**
- Skills require a Pro plan or higher. Check your plan at [claude.ai/settings/billing](https://claude.ai/settings/billing).

---

## Keeping CIT Updated

When a new version is released, repeat Steps 1 and 2. The new skill files will replace the old ones. Your saved config and invariants in CLAUDE.md won't be affected.

To check the latest version: [github.com/AstorisTheBrave/CIT-Cascade-Impact-Trace-for-Claude](https://github.com/AstorisTheBrave/CIT-Cascade-Impact-Trace-for-Claude)

---

## Want to Understand the Full Design?

Read [BLUEPRINT.md](./BLUEPRINT.md). It covers every design decision made for CIT, what alternatives were rejected, and what limitations each decision carries. If you want to contribute or just want to understand why CIT works the way it does, the blueprint is the place to start.
