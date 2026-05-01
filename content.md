<!--
  Prem's Claude Code Quickstart — content source
  Last updated: May 2026

  STRUCTURE NOTES (for the build script, not the reader):
  - Each section starts with `## Section N — Title`
  - Immediately under each section header: `<!-- tag: TAG -->` where TAG is one of:
      cc-only          (collapse for Lovable readers)
      also-lovable     (works in Lovable too — show with subtle marker)
      lovable-handles  (Lovable handles automatically — show with subtle marker)
      meta             (no Lovable label needed, never collapsed)
  - Each Part is `# Part N — Title` (H1)
  - Install commands per OS use blocks wrapped in `<!-- os: macos -->` / `<!-- os: linux -->` / `<!-- os: windows -->` markers
  - Code blocks use standard fenced syntax. The build script wraps each in a copy-button container.
-->

# Prem's Claude Code Quickstart

*The opinionated path I'd take if I were starting today.*

*Last updated: May 2026*

---

Hi, I'm Prem. I've been doing AI work for eight years and these days I run three projects almost entirely on Claude Code — a clinical trial forecaster, a roleplaying game called Aurea, and a meditation platform called Yantra. This guide is what I wish I'd had on day one.

If you're using a full-service builder like Lovable or Bolt instead of Claude Code, much of this still applies — the prompting techniques are universal. Toggle the **"On Lovable instead?"** switch at the top of the page to hide the sections that won't help you.

**Suggestions, corrections, additions welcome — open an issue or send a PR.**

---

# Part 1 — Deciding

## Section 1 — Should you use Claude Code at all?
<!-- tag: meta -->

There are two ways to build apps with AI right now. Pick the one that fits how you want to work.

### The two paths

**Full-service builders** — Lovable, Bolt, v0, Replit Agent. You describe what you want in a chat box on a website. They build it, host it, and handle the database, login, and deployment for you. You can use them with zero technical setup. The catch: you're working inside the limits of what a single chat session can express, on the platform they've designed.

**Claude Code** — a tool that runs on your own computer. You install it, point it at a folder, and it works inside that folder like a contractor would. There's a learning curve at the start — terminal, files, git — and you assemble your own pieces (where to host, which database, how to deploy). The upside: no ceiling. You can run multiple sessions, build anything, switch tools whenever you want, and own everything you create.

### When each one wins

Use **a full-service builder** if any of these apply:

- You want a working, polished, deployed app this weekend, end of story.
- You don't want to learn what a terminal is, and you don't need to.
- What you're building is a fairly standard pattern — landing page, dashboard, marketplace, simple SaaS.
- You value the visual editor (click-and-modify) more than fine control over the code.
- You're testing an idea fast and might throw it away.

Use **Claude Code** if any of these apply:

- You want to learn how this stuff actually works, not just get an output.
- You're building something unconventional — a game with custom mechanics, a research tool, a multi-agent system, anything that doesn't fit a standard template.
- You want multiple AI sessions running at once on the same project (one researching, one implementing) — this isn't possible in full-service builders today.
- You want a real native mobile app. Lovable produces web apps that run in a browser; for App Store distribution you'd need a separate wrapping tool. Claude Code can build native iOS, Android, or cross-platform apps (using frameworks like Flutter or React Native) — you'll need to pick the approach, but the path exists and you control it.
- You expect to keep building on this project for months or years and don't want to be locked into one platform's limits.
- You want full control: any database, any hosting, any framework, any model.

### The honest cost-benefit

Full-service builders get you to a working app in a single afternoon. That's real. Many people read the previous list, see they fit the "fast and standard" path, and Lovable is genuinely the right answer for them.

What full-service builders do not handle well, today:

- Anything that needs more than one AI agent collaborating on the same project.
- **Native mobile apps.** Lovable, v0, and Bolt produce web applications. They run in a mobile browser and look fine on a phone, but they are not native iOS or Android apps. To get into the App Store you'd need to wrap the output with another tool (Capacitor, Median) — which often defeats the "no setup, no configuration" promise.
- Highly customized logic that's hard to express in chat ("when X happens, do Y unless Z, and also write to this third-party API in this specific way").
- Long-running projects where you'll want to switch databases, hosts, or frameworks later.
- Building skills you can carry to other tools — what you learn in Lovable mostly stays in Lovable.

What Claude Code asks of you in exchange for no ceiling:

- A terminal-and-folders mental model (this guide teaches it; about 15 minutes).
- A small amount of git (this guide teaches that too).
- Patience for the first hour, when you're slower than you'd be in Lovable.
- Ongoing taste decisions about hosting, database, deployment.

### My take

I went straight to Claude Code when it came out, but I'm an experienced software engineer — that's not a useful data point for most readers. The more useful answer: I'd recommend Lovable to a friend who wants a tool to ship a specific, fairly conventional app this month and never think about it again. I'd recommend Claude Code to a friend who wants to *become someone who builds things*, who has multiple ideas, or who wants to build something that doesn't fit a template.

For the rest of this guide, I'll assume you've picked Claude Code. If you went with Lovable, much of what follows still applies — prompts, the four-phase loop, specs, redteam — and sections that are Claude-Code-specific are clearly marked. Toggle the switch at the top to hide them.

### Quick decision aid

| Your situation | Probably the right tool |
|---|---|
| "I want my idea live this weekend" | Full-service builder |
| "I'm not technical and don't want to be" | Full-service builder |
| "I'm building a standard pattern (dashboard / landing page / marketplace)" | Either; full-service is faster |
| "I want a real iOS or Android app in the app stores" | Claude Code (Flutter is often the easiest cross-platform path) |
| "I want to learn how this all works" | Claude Code |
| "I'm building something weird" | Claude Code |
| "I want multiple agents working on this" | Claude Code |
| "I'm in this for the long haul" | Claude Code |

---

# Part 2 — Foundations

*If you've used a terminal before, skip to Part 3.*

## Section 2 — The terminal and folders
<!-- tag: cc-only -->

Two foundations before anything else, because most people coming to Claude Code are not coming from a developer background.

**The terminal** is a window on your computer where you type commands instead of clicking. Programmers use it because typing is faster than clicking once you know what you want. It looks intimidating but it's actually simpler than most apps — no menus, no settings buried in tabs. You type a command, hit enter, something happens.

On a Mac, the app is called Terminal (open Spotlight with Cmd+Space and type "terminal"). On Windows, it's PowerShell or Command Prompt. On Linux, you already know.

We'll use the terminal for two things in this guide: starting Claude Code, and watching what Claude does.

**Folders** are everything else. Every program on your computer lives in a folder. Folders contain files and other folders, all the way down. When you open Finder on a Mac or Explorer on Windows, you're looking at folders. The terminal sees the same folders, just with prettier-than-icons output.

Why this matters for Claude Code: **Claude doesn't know about your whole computer. It only knows about one folder at a time** — the folder you started it in. Files in that folder, Claude can read and edit. Files outside it, Claude can't see. So the very first habit, before anything else: pick the folder for your project, walk into it, then start Claude.

The command for "walk into a folder" is `cd` (change directory). Two commands you'll use constantly:

```bash
cd ~/Documents/my-app    # walk into this folder
pwd                      # show me where I am right now
```

That's enough to get going. We'll do this together in section 4.

## Section 3 — Your project folder, and why git is your safety net
<!-- tag: lovable-handles -->

Before you install Claude Code, decide where your projects will live and set up your safety net.

**Pick a parent folder for all your projects.** I use `~/Projects/`. Inside it, each project gets its own folder. So Aurea lives at `~/Projects/aurea/`, Yantra lives at `~/Projects/yantra/`. Pick a structure you like and stick with it.

Make your first project folder now:

```bash
mkdir -p ~/Projects/my-first-app
cd ~/Projects/my-first-app
```

**Now, git.** Git is a tool that takes snapshots of your project as it grows. Every snapshot is a recovery point. If Claude does something you don't like — deletes a working file, refactors in a way you hate, "improves" something that wasn't broken — you can roll back to a snapshot in one command.

This isn't optional. **If you're using Claude Code, you're using git.** Not "you should." You must. Two reasons:

1. Claude will eventually do something destructive. Without git, that work is gone. With git, it's `git reset --hard` and you're back where you were.
2. When something works, you want to lock it in. Commit small, commit often. The blast radius of any bad session is bounded by your last commit — keep the radius small.

Initialize git in your new folder:

```bash
git init
git add -A
git commit -m "baseline before Claude"
```

You'll do this once per project, before you ever start Claude there. Some prompts for later in the guide will remind you to commit before risky operations — take those reminders seriously.

If you don't have git installed, on Mac it comes with Xcode Command Line Tools (`xcode-select --install`); on Windows, install Git for Windows from `git-scm.com`; on Linux, your package manager has it.

## Section 4 — Install Claude Code
<!-- tag: cc-only -->

Pick your platform with the toggle at the top of this section, then run the install command.

<!-- os: macos -->
**macOS**

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Or, if you prefer Homebrew:

```bash
brew install --cask claude-code
```
<!-- /os -->

<!-- os: linux -->
**Linux / WSL**

```bash
curl -fsSL https://claude.ai/install.sh | bash
```
<!-- /os -->

<!-- os: windows -->
**Windows (PowerShell)**

```powershell
irm https://claude.ai/install.ps1 | iex
```
<!-- /os -->

The native binary is the recommended path on every platform — it auto-updates, so you stay on the latest model and features without thinking about it. Avoid the older npm install if you have a choice.

**Plan tier:** you'll need a Claude subscription. Start on **Pro at $20/month** — it's plenty for learning and for any reasonable side project. When you find yourself hitting your token limit and waiting, that's the upgrade signal. The **Max plan at $100/month** pays for itself in saved hours once you're using Claude Code daily. Don't pre-upgrade; let the workflow tell you when.

Once installed, walk into your project folder and start Claude:

```bash
cd ~/Projects/my-first-app
claude
```

First time, you'll be prompted to log in. Follow the link, authorize, come back. Now run your very first prompt — just to confirm everything works:

```
> what files are in this directory and what does this project do?
```

Claude will tell you the folder is empty (which it is). That round-trip — your prompt, Claude reading the directory, Claude responding — is the whole loop with one tool call. Everything from here is variations on that pattern.

---

# Part 3 — Build Your First App

## Section 5 — Pick what you're building
<!-- tag: also-lovable -->

The single biggest predictor of whether someone sticks with Claude Code is having a project they actually want to use. Don't pick a tutorial project. Pick something you'd open every day.

If you're not sure where to start, here are five categories that work well for first projects. Each comes with a starter prompt you can adapt — the bracketed parts are yours to fill in. Each prompt asks Claude to propose a plan *before* writing code, so read the plan in chat and push back before you say go. (You'll meet plan mode proper in section 6.)

**Personal tracker.** Habits, journaling, mood, meditation streaks. Single user, simple data, immediate daily value.

```
> I want to build a personal [tracker type] web app. Single page,
  works offline, saves data in the browser. Plain HTML and JS,
  no build step. Specifically: [what gets tracked, how often,
  what you want to see at a glance]. Read what's here, then
  propose a plan.
```

**Content site.** Newsletter, blog, portfolio, teaching page. Mostly read-only, lots of typography, a clear voice.

```
> I want to build a [content site type] for [audience]. The
  voice should feel [adjective]. Pages I need: [home, about,
  archive, individual posts]. Static site — generate HTML I
  can host on Vercel or Netlify. Read what's here, then propose
  a plan.
```

**Small game.** Text adventure, card game, daily puzzle. Limited scope, fun feedback loop, naturally invites iteration.

```
> I want to build a [game type] in the browser. Core mechanic:
  [describe the loop in one sentence]. Win condition: [what
  ends a round]. Plain HTML and JS. Read what's here, then
  propose a plan.
```

**Productivity tool.** Todo list, notes, time tracker, expense log. Solves a personal annoyance, low ambiguity, easy to verify it works.

```
> I want to build a [tool type] for myself. The thing I want
  it to do that other apps don't: [your specific itch]. Single
  user, browser-only for now. Read what's here, then propose
  a plan.
```

**Community app.** Group ritual tracker, prayer wall, shared journal, study group dashboard. Multi-user, shared data, lower polish needs because the audience is small and known.

```
> I want to build a small community app for [your group].
  Core feature: [what people do together]. Roughly [N] users.
  Web app, will need a real database for shared data. Read
  what's here, then propose a plan.
```

**A note on adapting these.** Don't agonize over the wording. Claude is good at asking clarifying questions when something's vague. The more important move is to type something specific to *you* into the brackets — "habit tracker for daily meditation streaks" lands harder than "tracker app."

## Section 6 — The four-phase loop: build the first version
<!-- tag: also-lovable -->

The canonical Claude Code workflow has four phases. Most beginners skip from prompt straight to implementation and wonder why the output is mediocre. The four phases are:

1. **Explore** — Claude reads the code, you ask questions, you both build understanding.
2. **Plan** — Claude proposes an approach. You read it, edit it, approve it.
3. **Implement** — Claude writes the code. You watch.
4. **Verify** — You run it, click around, push back on anything that isn't right.

Repeat. The loop is the work.

For your first project, run the loop end-to-end. Pick a starter prompt from section 5, fill in the brackets, paste it. Claude will explore the (empty) folder and come back with a plan.

**A detail that compounds:** before Claude starts writing code, press **Shift+Tab** to enter plan mode. In plan mode, Claude can read but not write. It produces a plan you can edit and approve. Press Shift+Tab again to switch back to execute mode and let Claude implement.

For tiny tasks like fixing a typo, plan mode is overhead. For anything that touches multiple files, *always plan first*. The cost of a bad plan caught early is zero. The cost of a bad implementation is an hour of cleanup.

Once you've approved the plan and Claude has written code, **verify** it. Don't just read the diff — ask Claude to walk you through what it did:

```
> walk me through how the data flow works in what you just
  built, as if you were onboarding a new engineer.
```

Forcing Claude to explain its own changes catches subtle problems you'd miss by just reading code.

**When the output is wrong** — and it will be — don't start a new conversation. Iterate. Point at the specific thing that's broken, describe what *correct* looks like, let Claude fix in place. You lose all the built-up context if you restart. Iteration is the workflow.

```
> the [specific thing] is rendering [specific wrong behavior].
  It should [specific correct behavior]. Fix and retest.
```

By the end of one loop you should have something running. Open the file in your browser, click around, make sure it works. Then commit:

```bash
git add -A
git commit -m "first working version"
```

You now have a working app and a recovery point. The rest of this guide is layering capability on top of that.

## Section 7 — Add a feature to whatever you built
<!-- tag: also-lovable -->

The prompts in this section work on any of the starter templates. They're written generically — "your app" rather than "your habit tracker" — because the technique transfers regardless of what you built.

Pick one or two features to add. Don't try all four at once. The goal is practice running the loop on something more complex than the first version.

**Login and registration.** Adds the concept of users — your app stops being "single-machine" and starts being "real." *(This one needs a backend — easier to tackle after section 9 if your app is still localStorage-only.)*

```
> I want to add login to my app. Email and password is fine
  for now. After login, the app should show only the data
  belonging to that user. Plan first: what files change, how
  do we store users, what's the simplest secure approach?
  Don't implement yet.
```

**Theming (light/dark mode).** Teaches CSS variables and persistent preferences without database complexity.

```
> I want to add a light/dark mode toggle. The button should
  live in [where]. The choice should persist when I reload.
  Default to whatever the OS prefers. Plan first.
```

**Search.** Forces you to think about your data shape and how users want to find things.

```
> I want to add search across [whatever your data is]. As I
  type in the search box, results should filter live — no
  separate "search" button. Match against [which fields].
  Plan first.
```

**Notifications.** Introduces async, scheduling, and the browser's permission model — a real bump in sophistication.

```
> I want to add browser notifications when [trigger condition].
  Ask for permission the first time. Let me turn them off in
  settings. Plan first — and tell me what's tricky about
  notifications I might not be thinking of.
```

For each feature, run the full loop: plan, approve, implement, verify, commit. The "plan first" line is the load-bearing word in each prompt. Drop it and Claude will barrel ahead. Keep it and you'll catch problems before they're code.

## Section 8 — Ship it to the world: deploy with Vercel
<!-- tag: lovable-handles -->

This is the moment your app stops being a thing on your laptop and becomes a real URL you can text to a friend. For most people, this is the most emotionally significant step in the whole journey.

We'll use **Vercel** because it's free for personal projects, has the most forgiving setup, and integrates with GitHub so every code push automatically deploys.

**Step 1 — Get your project on GitHub.** If you haven't already, create a public repository on github.com (the website), then in your project folder run the commands GitHub shows you. Claude Code can do this whole step for you:

```
> I want to push this project to GitHub. My GitHub username
  is [your-username]. Walk me through what to do, then do
  the parts you can do without new credentials from me.
```

**Step 2 — Connect Vercel.** Go to vercel.com, sign in with your GitHub account, click "Add New Project," pick your repository, accept the defaults. Vercel auto-detects most setups. Click Deploy.

That's it. Vercel gives you a URL like `my-first-app.vercel.app`. Open it. Share it.

**The teaching moment:** every time you push code to GitHub, Vercel automatically redeploys. So your git habit isn't just for safety anymore — it's also your shipping mechanism. This locks in the "commit constantly" rhythm because every commit is potentially a release.

A useful prompt for after deployment:

```
> fetch my deployed app at [URL]. Read the HTML and tell me
  what you'd improve about the structure, content, or
  accessibility. (For a visual review, paste a screenshot.)
  Be specific.
```

You can use Claude as a first reviewer on your live site — surprisingly useful for catching the obvious things you stopped seeing because you stared at them too long.

## Section 9 — Make it real: from local storage to a real database
<!-- tag: lovable-handles -->

Up to this point, your app probably stores data in the browser's local storage. That works for a personal tracker on one device, but it has real limits: data doesn't sync across phones and laptops, two users can't share anything, and clearing your browser cache nukes everything.

Moving to a real database is the moment your app crosses from "toy" to "real." We'll use **Firebase Firestore** for this guide — the free tier is generous (50K reads/day, 20K writes/day, 1GB storage), it doesn't auto-pause when your app is idle, and the document data model fits the kinds of apps you're likely to build.

**A note on Supabase**, the other obvious choice: also great, more powerful for SQL-relational data, but the free tier pauses projects after 7 days of inactivity. For a beginner who builds something one weekend and comes back three weeks later, that "why is my app broken?" experience is enough reason to start with Firestore.

**Run the migration prompt:**

```
> I want to migrate my app's data from browser localStorage
  to Firebase Firestore. The data shape is: [briefly describe
  what you store]. Plan first: what changes in the code, what
  Firebase setup do I need to do in the browser, what stays
  the same. Show me the plan before any code changes.
```

The plan should walk you through:

1. Creating a Firebase project at console.firebase.google.com (free)
2. Enabling Firestore in test mode for now
3. Adding the Firebase SDK to your app
4. Replacing localStorage reads/writes with Firestore equivalents
5. (If you have login already) tying data to users with security rules

**Commit before you start the migration.** This is the kind of change where having a recovery point matters.

After migration, verify by:

- Opening your app on a different device or browser, logging in, and seeing the same data
- Checking the Firestore console to confirm data is showing up
- Asking Claude: `> read through my Firestore code and tell me where the security holes are`

Once it works, commit again. Push. Vercel redeploys. You now have a real app with real data.

---

# Part 4 — Going Deeper

*The techniques that make Claude Code dramatically better. Read these as you start to outgrow the basics.*

## Section 10 — CLAUDE.md: your project's memory
<!-- tag: cc-only -->

The most leveraged file in your project is `CLAUDE.md`. It lives in your project root. Claude Code reads it at the start of every session — it's how you tell Claude what this project is and how to behave inside it.

Without a CLAUDE.md, you re-explain your project every time. With a good one, Claude shows up already knowing your stack, your conventions, and your hard rules.

Generate yours now:

```
> create a CLAUDE.md for this project. Ask me 5 questions first
  about my preferences and conventions, then draft the file.
  Keep it under 30 lines for now.
```

Claude will ask things like *"What's your preferred testing framework?"* or *"Are there directories I should never touch?"* Answer honestly. The output should look something like:

```markdown
# Project: HabitTracker
- Stack: vanilla HTML + JS + Firebase Firestore
- Style: small functions, named exports, no frameworks
- Always run the test suite before claiming a task is done
- Never edit anything in /vendor or /generated
- Database migrations require explicit human approval
- Prefer composition over inheritance
```

**Keep it short.** Aim for 30 lines to start, and cap at around 100 even after years of use. This is a contract, not a textbook. The discipline: when you find yourself explaining the same thing twice in different sessions, that's a CLAUDE.md entry.

A great CLAUDE.md is the single most compounding investment in this whole guide. The difference between explaining yourself every session and never explaining yourself again.

## Section 11 — Plan mode: think before you build
<!-- tag: cc-only -->

You met plan mode briefly in section 6. Now the full picture.

Press **Shift+Tab** to toggle plan mode. In plan mode, Claude reads everything it needs but writes nothing. It produces a plan you can edit and approve. Press Shift+Tab again to flip back to execute mode and let Claude implement what you signed off on.

The rule of thumb: if you could describe the change in one sentence, skip the plan. Otherwise, plan first.

A good plan-mode prompt:

```
> [Shift+Tab to enter plan mode]
> I want to [feature description]. What files would change,
  what's the approach, what could go wrong, and what tests
  should I write? Be specific about file paths.
```

What makes plan mode work isn't the mode itself — it's that you take the plan seriously. Read it. Push back on anything that smells off. Ask for alternatives. The plan is cheap to revise; the implementation is expensive to redo.

A common failure mode: you scan the plan, type "looks good," let Claude implement, and then realize halfway through that the approach was wrong. The plan deserves the same care as a code review. Two minutes spent on the plan saves an hour of cleanup.

## Section 12 — Tests: how Claude knows when it's done
<!-- tag: cc-only -->

Quick definition for anyone new: a **test** is code whose only job is to check that your real code does what it should. You write a sentence like *"after I add a habit called 'read,' the list should contain 'read'"* — and the test fails loudly if that's not true.

Why this matters for Claude Code specifically: Claude is great at writing code that *looks* right but doesn't actually work. Tests catch that. They're also how Claude knows when it's done — instead of you eyeballing every change, the test suite either passes or it doesn't.

**The single most useful habit:** ask Claude to write a test *before* it writes the feature.

```
> before you implement [feature], write a test that checks
  [specific expected behavior]. Run the test to confirm it
  fails (because the feature doesn't exist yet). Then implement
  the feature. Then run the test again to confirm it passes.
```

This is a slightly slower path than "just implement it." It's also dramatically more reliable. You'll catch a category of bugs that's hard to catch any other way — the kind where the code looks reasonable and runs without error but produces the wrong answer.

For your first projects, you don't need a sophisticated test framework. A few asserts in a separate file you can run from the terminal is enough. Claude can set this up in two minutes:

```
> set up a minimal test runner for this project. I want to
  write tests as plain JS files I can run from the terminal.
  No framework if possible.
```

## Section 13 — Specs: writing down what "done" looks like
<!-- tag: also-lovable -->

A **spec** is a short markdown file describing what success looks like for a feature, before any code gets written. Same idea as a test, but at the requirements level.

Specs do two things. First, Claude reads the spec and self-checks against it — which catches "solved the wrong problem" early. Second, when you come back to the project next week or next month, the spec tells you (and Claude) what was actually intended.

A spec doesn't need to be long. Five bullets is often plenty:

```markdown
# Feature: Daily streak counter

- Shows current streak (consecutive days with at least one
  habit checked off) at the top of the page
- Resets to 0 if a day is skipped
- Counts today as part of the streak only if at least one
  habit is checked off today
- Shows a "longest streak" stat below the current streak
- No notifications, no sounds, no animations — just the number
```

The pattern is **spec first, tests second, code third**. Yes, it's slower upfront. Yes, it's massively faster overall. The shortcut paths cost more than the chain.

A useful prompt for any non-trivial feature:

```
> I want to add [feature]. Before any code, ask me 5 clarifying
  questions about scope, edge cases, and constraints. Then
  draft a spec.md.
```

---

# Part 5 — The Serious-Projects Toolkit

*The techniques that separate "I tried it once" from "I ship with this." You'll know when you need them.*

## Section 14 — Redteam: have Claude attack its own plan
<!-- tag: also-lovable -->

Borrowed from security: a **redteam** is the practice of attacking your own work to find holes, before someone else does. In Claude Code, the technique is to spawn parallel sub-agents with adversarial personas, each reading the same spec but with a different lens, each running in its own fresh context.

The fresh-context part is the load-bearing trick. If you do the review in your own conversation, you'll rationalize toward the position you've already convinced yourself of. Sub-agents don't share your conversation history — they can't be primed by what you've been thinking. That's where the independent eyes come from.

**The bare-bones redteam prompt:**

```
> Do an adversarial review of [doc-or-spec-path].

  Spawn 3 sub-agents in parallel. Each reads the same doc
  but with a different lens. Run them in fresh contexts
  (Task tool / sub-agents) — do NOT do the review yourself
  in this conversation.

    Persona 1 — Adversary: Find ways this fails. What breaks
      under load, edge cases, misuse, scale? What's the worst
      plausible outcome?

    Persona 2 — Skeptic: Find unverified assumptions. What
      does this claim that isn't proven? What's stated as
      fact but is actually a guess? What "obviously" works
      that hasn't been measured?

    Persona 3 — Librarian: Find what this contradicts or
      duplicates. Does it conflict with existing code, specs,
      or earlier decisions? Is it reinventing something that
      already exists?

  Each sub-agent returns 3-5 concrete findings. Every finding
  must have:
    - A one-line title
    - Why it matters (concrete scenario, not "feels wrong")
    - Severity (critical / major / minor)

  Then aggregate the 9-15 findings into one report grouped
  by severity. Do NOT deduplicate aggressively — overlapping
  findings from different angles are signal that something
  is genuinely load-bearing.
```

**Common pitfalls:**

- If you do the review in your own context, you'll rationalize toward your existing position. The fresh-context spawn is what gives you independent eyes.
- "Vague vibes" findings ("feels off," "could be cleaner") are the noise that drowns the signal. The per-finding scenario requirement is what filters them out.
- Resist the urge to add seven personae on day one. Three good ones beat seven mediocre ones, and three is enough to feel the technique work.

Use redteam before you implement anything serious. Five minutes of redteam saves you days of "oh, we never thought about that."

## Section 15 — The full chain
<!-- tag: also-lovable -->

For any feature serious enough to ship, run the full chain:

1. **Idea** — rough concept, sentence or two.
2. **Spec** — write it down formally as `spec.md`. (Section 13.)
3. **Redteam** — three parallel personae attack the spec, find gaps, fold findings back in. (Section 14.)
4. **Plan** — implementation plan, in plan mode, against the updated spec. (Section 11.)
5. **Tests** — Claude writes tests against the spec *before* implementation. (Section 12.)
6. **Implement** — Claude writes the code. You watch.
7. **Review** — a separate Claude session reviews the diff. Works because that session has no ego in the implementation. (Section 16.)

Heavy? Yes. But this is what serious software engineering looks like with AI in the loop, and beyond a certain project size you need this discipline. The shortcuts always cost more than the chain.

Use the four-phase loop (section 6) for small things. Use the chain for things you actually want to ship.

## Section 16 — Running parallel sessions: research vs. implementation
<!-- tag: cc-only -->

When I'm working seriously, my screen looks like this:

```
┌──────────────────────────┬──────────────────────────┐
│  PROJECT A — ENGINEER    │  PROJECT A — RESEARCHER  │
│  implementing features   │  investigating, planning │
│  writing tests           │  reading docs, no code   │
├──────────────────────────┼──────────────────────────┤
│  PROJECT B — ENGINEER    │  PROJECT B — RESEARCHER  │
│  implementing features   │  investigating, planning │
│  writing tests           │  reading docs, no code   │
└──────────────────────────┴──────────────────────────┘
```

Two projects in flight, two sessions per project. Four terminals, one me.

**The Engineer–Researcher split per project.** While the Engineer is implementing, the Researcher is two steps ahead — reading docs, drafting the next spec, redteaming the next feature. By the time the Engineer finishes, the next plan is ready to hand off.

**Why two projects, not one.** Claude sessions have dead time — long tool runs, tests running, you reading the diff. Switching to a different project fills that gap productively. Why not three? Because *I'm* the bottleneck — I can only hold so much state in my head. Two is the sweet spot for me. You'll find your own number.

**Start small.** Don't open four terminals on day one. Start with one project, two roles. Add the second project only when you notice yourself idle.

**The git blast radius is real.** With multiple sessions on the same codebase, two Claudes editing the same file at the same time is a disaster. Two rules:

- Use **git worktrees** so each session has its own checkout. (`git worktree add ../my-app-research` creates a parallel working directory at the same project, on its own branch.)
- Commit constantly. Every passing test, every working feature.

**A real warning.** You'll wait a lot, even with parallel sessions. Resist the temptation to fill every moment with another Claude task. Sometimes the right move is to step away and let one finish.

## Section 17 — Dispatch agents: hard limits keep them honest
<!-- tag: cc-only -->

Once you're comfortable spawning sub-agents (section 16), you'll hit the next problem: sub-agents drift. They loop on errors, retry the same broken approach with slight variations, decide a failing safety gate is "the problem" and disable it, and at the end of two hours of wall time return a confident summary that bears no relation to what actually happened.

The fix is not better agents. The fix is **hard limits + isolation**, written into every dispatch prompt. The boilerplate below is mechanical, not stylistic — every line earned its place from a real incident.

### Before you spawn

Write down on a scratch doc:

- **Worktree path** — must NOT be your current working directory. Make a separate git worktree for the agent so its writes can't pollute your main checkout. (`git worktree add ../my-app-agent-1` is the one-liner.)
- **Files it's allowed to touch** — three to five globs, no broader.
- **Success criterion** — one line. "Tests green and a commit on branch X."
- **Honest wall-time estimate.**

If two sub-agents would share a worktree, stop and reassign one. If the task needs physical hardware — a phone, a hardware key, a human in the loop — an agent cannot do it. Surface and reassign.

### The dispatch prompt

In the agent's prompt, paste this verbatim:

```
HARD LIMITS (do not violate):
• Wall clock: 30 minutes maximum. Stop and write a status
  report if you hit it, even mid-task.
• Retries: ONE retry per pre-commit / lint / test failure.
  If the same gate fails twice, stop and report what failed —
  do not keep trying variations.
• No bypass flags (--no-verify, --skip-tests, --force, etc.)
  without explicit user approval first.
• Stuck = 5 minutes on the same error with no progress. When
  stuck, stop and report. Do not investigate the hook source
  code to work around it.
• Stay inside <worktree-path>. Never write files outside it.

SUCCESS = <one-line criterion>. Before exiting, write a status
file at <worktree>/.status.md with one of: done | blocked |
stuck, followed by 2-3 sentences on what happened.
```

### After the agent returns

Every time, no exceptions:

- **Read `.status.md`.** Trust the file, not the agent's chat summary.
- If "done": review the actual git diff (not the agent's word for it), merge or open a PR, then delete the worktree.
- If "blocked" or "stuck": read the report, pick up where it stopped. Do not respawn blind.
- Worktree gets deleted either way. Lingering worktrees accumulate and become a different kind of bug.

### Why each line is there

- **The 30-minute cap** feels arbitrary until the first time an agent burns two hours retrying the same pre-commit hook with slight variations. Then it feels obvious.
- **"Read `.status.md`, not the chat summary"** is the most-skipped line. Agents are optimistic narrators of their own work — the file is the receipt.
- **The "no bypass flags" line** is what stops the failure mode where an agent hits a hook, decides the hook is the problem, and disables it. Hooks exist for reasons; if one needs changing, that's a human decision.
- **The worktree-isolation step** prevents the silent corruption case where two parallel sub-agents write to the same directory and last-write-wins.

This pattern is more discipline than technique. The first time it saves you — and it will, probably within your first week of dispatching agents — you'll never skip the boilerplate again.

---

# Part 6 — Where to go from here

## Section 18 — Closing checklist
<!-- tag: meta -->

If you take one thing from this guide, take this checklist.

1. **Install Claude Code.** (Section 4.)
2. **Pick a project you'd actually use yourself.** Not a tutorial. Something you want to exist.
3. **`git init` before anything else.** (Section 3.)
4. **Write a 30-line CLAUDE.md.** (Section 10.)
5. **Run the four-phase loop.** Explore → Plan → Implement → Verify. Commit between rounds.
6. **Ship something this weekend.** Vercel, public URL, share with a friend.

Don't pick a learning project. Pick something you want to exist. Motivation does the rest.

### When you're ready to go deeper

- **Add tests** to whatever you built (section 12). Catch the bugs you can't see.
- **Write your first spec** for the next feature (section 13). Notice how much clearer Claude's output gets.
- **Try a redteam** on a spec before you implement (section 14). The first time the sub-agents catch something real, you'll never skip it again.
- **Run a parallel research session** alongside your engineer (section 16). The first time you finish a feature and the next one is already specced and waiting, you'll know why.

### Where to ask for help

- Found a bug in this guide? Open an issue on the repo.
- Have a better prompt? Send a PR.
- Disagree with an opinion here? Tell me why; I'll update.

— Prem

---

*This guide is meant to evolve. Last updated May 2026.*
