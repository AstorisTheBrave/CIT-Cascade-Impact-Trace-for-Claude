# Installing CIT: Step by Step

This guide walks you through everything from downloading the files to confirming CIT is working. Find your platform below and follow the steps in order.

---

## Which Version of Claude Do You Have?

There are three ways to use Claude and CIT works on all of them:

| Platform | How you access it | Install method |
|---|---|---|
| **Claude Desktop** | Downloaded app on your Mac or Windows PC | Upload .zip files through the app |
| **claude.ai** | Browser at claude.ai | Upload .zip files through settings |
| **Claude Code** | Terminal command (`claude`) | Copy folders to `~/.claude/skills/` |

Not sure which one you have? If you go to claude.ai in your browser, that's the web version. If you downloaded a standalone app, that's Claude Desktop. If you've run `claude` as a terminal command, that's Claude Code.

> **Note:** Skills require a Pro plan or higher on Claude Desktop and claude.ai. Claude Code users on any plan can use skills.

---

## Download the Files

### For Claude Desktop or claude.ai (ZIP files)

Download the individual skill files you need. Right-click each link and save:

- [cit-config.zip](./downloads/cit-config.zip) - Shared config (recommended first install)
- [cit-chat.zip](./downloads/cit-chat.zip) - CIT for conversations
- [cit-code.zip](./downloads/cit-code.zip) - CIT for coding
- [cit-design.zip](./downloads/cit-design.zip) - CIT for design

Or download everything at once using the green **Code > Download ZIP** button on the main repo page, then find the zip files inside the `downloads/` folder.

### For Claude Code (folder-based)

Clone or download the full repo. You will copy the skill folders directly.

```bash
git clone https://github.com/AstorisTheBrave/CIT-Cascade-Impact-Trace-for-Claude.git
```

---

## Installing on Claude Desktop

1. Open the Claude Desktop app on your computer
2. Click the **settings icon** (gear icon, usually bottom left or top right depending on your version)
3. Go to **Customize** then **Skills**
4. Click **Add skill** or the **+** button
5. Upload `cit-config.zip` first
6. Repeat for each skill you want: `cit-chat.zip`, `cit-code.zip`, `cit-design.zip`
7. Make sure each skill is toggled **on** after uploading

**Confirming it worked:**
After uploading, start a new conversation and type:
```
cit setup
```
Claude should respond with three setup questions. If it does, you're good.

**Troubleshooting:**
- If you don't see a Skills section in settings, check your plan. Skills require Pro or higher.
- If a skill uploads but doesn't respond, try toggling it off and back on, then start a fresh conversation.
- Some features roll out region by region. If Skills isn't visible yet, check back in a few days or check the [Claude release notes](https://support.claude.com).

---

## Installing on claude.ai (Browser)

1. Go to [claude.ai](https://claude.ai) and sign in
2. Click your profile icon in the top right corner
3. Go to **Settings**
4. Click **Customize** then **Skills**
5. Click **Add skill** or the **+** button
6. Upload `cit-config.zip` first
7. Repeat for each skill: `cit-chat.zip`, `cit-code.zip`, `cit-design.zip`
8. Make sure each skill is toggled **on**

**Confirming it worked:**
Start a new conversation and type:
```
cit setup
```
Claude should respond with three setup questions.

**Troubleshooting:**
- Skills require a Pro plan or higher. If you don't see the Skills section, check your plan at [claude.ai/settings/billing](https://claude.ai/settings/billing).
- Skills load at the start of a session. If you just installed one, open a fresh conversation.
- If CIT-Chat rules aren't saving between conversations, go to Settings and make sure **Memory** is turned on. CIT-Chat relies on memory to persist your rules.

---

## Installing on Claude Code (Terminal)

Claude Code reads skills from a folder on your machine. You copy the skill folders there directly.

### Find or create your skills folder

```bash
ls ~/.claude/
```

If there's no `skills` folder, create one:

```bash
mkdir -p ~/.claude/skills
```

### Copy the skill folders

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

No restart needed. Claude Code picks up skills automatically.

**Confirming it worked:**
Start a new Claude Code session and type:
```
cit setup
```
Claude should respond with three setup questions.

---

## First-Time Setup

After installing, start a new conversation and type `cit setup`. Claude will ask you three questions:

**1. Visibility**
Should CIT show you the trace before acting, or run it silently?

Recommended for new users: show the trace. This way you can see it working and verify the cascade maps look right before trusting them.

**2. Format**
Tree view or Domino chain?

Tree is easier to read when a change has lots of connections branching in different directions. Domino is more visual and reads left to right, but it auto-switches to Tree when a path gets longer than 8 nodes. Either is fine. You can change this later with `cit settings`.

**3. Interrupt behavior**
If you type `cit off` mid-task, should CIT finish the current trace first or stop immediately?

- Finish first: safer. The action proceeds with full cascade awareness.
- Stop immediately: faster, but the current action proceeds without a complete trace.

Your answers are saved. You won't be asked again unless you type `cit settings` to change them.

---

## Testing That It Works

### Test CIT-Chat

Start a conversation and say:
```
I'm building a todo app. I only have 3 days and no budget for paid services.
```

Then follow up:
```
Actually let's use Firebase for the database.
```

CIT-Chat should trace the cascade impact on your constraints (3 days, no budget) before responding. If you see a trace with anchor types and risk levels, it's working.

### Test CIT-Code

Open a project in Claude Code and say:
```
I want to update the user settings function. Run a CIT trace before touching anything.
```

You should get a triage classification, a tiered cascade map with confidence labels, and any Tier 3 items flagged for manual verification before any code is written.

### Test CIT-Design

In a project with any component file, say:
```
I want to change the primary color token. Run a CIT trace first.
```

You should get a token ownership trace, a state matrix risk check across all 8 interaction states, and a breakpoint risk check before any changes are made.

---

## Useful Commands

Once installed, these work in any conversation:

| Command | What it does |
|---|---|
| `cit setup` | Run first-time setup |
| `cit status` | Check if CIT is on or off |
| `cit settings` | Review or change your preferences |
| `cit off` | Disable CIT for this conversation |
| `cit on` | Re-enable CIT |
| `cit rules` | See your saved Persistent User Rules |

---

## Installing Only Some Skills

You don't have to install all four. Each works on its own.

- Coding only: install `cit-config` and `cit-code`
- Chat and reasoning: install `cit-config` and `cit-chat`
- Design work: install `cit-config` and `cit-design`

If you install `cit-config` first, any other skill you add later picks up your settings automatically. If you skip it, each skill runs its own setup the first time and creates a shared config that later skills will find.

---

## Keeping CIT Updated

When a new version is released, repeat the install steps with the new files. Your saved config and architectural invariants in CLAUDE.md won't be affected by the update.

To check for updates: [github.com/AstorisTheBrave/CIT-Cascade-Impact-Trace-for-Claude](https://github.com/AstorisTheBrave/CIT-Cascade-Impact-Trace-for-Claude)

---

## Want to Understand the Full Design?

Read [BLUEPRINT.md](./BLUEPRINT.md). It covers every design decision, what was tried and rejected, and what limitations each decision carries. If you want to contribute or just want to understand why CIT works the way it does, start there.
