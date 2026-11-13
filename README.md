# Thriftline

A playbook that teaches Claude to do everyday business work with the least reading and writing that still gets the job right. It is built for sales teams, general office work, and companies turning repeat work into automations.

Thriftline is a Claude skill: a folder of instructions Claude loads when a matching task comes up. Nobody has to remember to invoke it.

## What it does

Usage limits are spent on four things: what Claude reads, what it writes, how long the conversation is, and rework. Thriftline cuts all four with five habits it applies to every task:

1. **Scope it.** Settle what "done" looks like before starting.
2. **Narrow at the source.** Have the CRM or other system filter and total the data.
3. **Compute, do not read.** Use code on files instead of reading rows.
4. **Write once, write short.** Answer first, and no recap.
5. **Reuse, do not redo.** Pull once, template once, keep findings.

It also flags, once per conversation, when a task would suit a lighter or heavier model than the one in use. It cannot switch the model itself. The person picks the model.

It deliberately does not economise on contracts, payments, customer-facing messages, hiring decisions, or anything that cannot be undone. Those get full care and a human approval step.

## What is inside

```
thriftline/
├── README.md                          This file
├── dist/
│   └── thriftline.skill               Packaged skill, ready to install
└── skills/
    └── thriftline/
        ├── SKILL.md                   Core rules, loaded whenever the skill triggers
        └── references/                Playbooks, loaded only when the task matches
            ├── sales.md               Leads, CRM, pipeline, cadences, calls, proposals
            ├── marketing.md           Campaigns, content, email, social, audiences
            ├── operations.md          Inbox, calendar, meetings, SOPs, vendors, research
            ├── finance.md             Invoices, expenses, reconciliation, KPIs, commissions
            ├── people-and-support.md  Tickets, help articles, hiring, onboarding, policies
            ├── data-work.md           Cleaning, de-duplicating, merging, summarising files
            └── automation.md          Scheduled tasks, pipelines, what to automate
```

About 800 lines in total, covering more than 50 common tasks plus methods for data work and automation.

### Why it is split into several files

A skill's main file is read in full every time the skill triggers. A single 800-line file would spend several thousand tokens on every task, which defeats the purpose. So the core rules live in a short main file, and each playbook is read only when the task calls for it. A pipeline report loads the core plus the sales playbook and nothing else.

### About the file name `SKILL.md`

Claude only recognises a skill whose main file is named exactly `SKILL.md`. That one name cannot be changed. Everything else (the repo, the skill's name, the package, the playbooks) is named for Thriftline.

## Installing

**Claude apps (web, desktop, mobile).** Upload `dist/thriftline.skill` in the skills area of Claude's settings. Skill uploads depend on your plan and on what your organisation's admin allows, and the exact menu changes over time, so check Anthropic's current help pages if you cannot find it.

**Claude Code.** Copy the `skills/thriftline` folder into your skills directory:

```bash
cp -r skills/thriftline ~/.claude/skills/
```

**A whole team.** An admin can add the skill for the organisation so every member gets it, where the plan supports that.

## Trying it

Once installed, give Claude ordinary tasks without mentioning tokens:

- "Pull this week's pipeline report from the CRM."
- "Clean up this lead list and remove duplicates."
- "Set up a daily digest of new leads."
- "Summarise these call transcripts and tell me what to update in the CRM."

The answers should lead with the result, show exceptions instead of every record, and hand back files for anything large.

## Adapting it to your company

- **Thresholds.** The row limits (50 rows to read directly, 20 records to list in a reply) are starting points. Change them in `SKILL.md` and `references/data-work.md`.
- **Model table.** Edit the tier table in `SKILL.md` to match how your team assigns work.
- **Your own playbook.** Add a file to `references/` in the same format (task, lean approach, what to deliver) and add a row for it in the "Which playbook to read" table in `SKILL.md`.
- **House formats.** If you have fixed templates for reports or notes, name them in the relevant playbook entry.

After editing, rebuild the package by zipping the `skills/thriftline` folder and renaming the zip to `thriftline.skill`, with the `thriftline` folder at the top level of the zip.

## Status and limits

- This is a first version and has not been measured against real workloads. Run it on a few real tasks and adjust.
- Savings depend on the work. Data-heavy and recurring tasks gain the most. Short one-off questions gain little.
- It is a set of instructions, not software. It needs no API keys and sends nothing anywhere.

## Putting this on GitHub

**In the browser (no tools needed):**

1. Sign in to GitHub and choose **New repository**.
2. Name it `thriftline`, choose Private or Public, and create it without adding a README.
3. On the new repository's page, choose **uploading an existing file**.
4. Unzip `thriftline-repo.zip`, open the `thriftline` folder, and drag everything inside it into the upload area.
5. Add a short message such as "Initial version" and commit.

**From the command line:**

```bash
cd thriftline
git init
git add .
git commit -m "Initial version"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/thriftline.git
git push -u origin main
```

If you make the repository public, add a licence file first so others know how they may use it. GitHub offers a licence picker under **Add file**.
