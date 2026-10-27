# Inbox Radar

A Claude plugin that triages your inbox according to **your** priorities.

**Core (needs only your email).** Every run it:

- **finds every email from the people you must never miss** (your team, investors, key clients, your boss) and flags the ones waiting on your reply,
- **spots the opportunities you care about**: deals, leads, investor conversations, partnership requests, requests with deadlines,
- **gives you a short digest** and pings you when something needs you.

**Optional add-on: log to your lists.** If you want, it also logs what it finds into your CRM (Affinity, Attio, HubSpot, …) and/or a Notion database, without duplicates, and can copy deals you add to Notion by hand into your CRM.

Setup starts with a short interview about your role and priorities. It comes with starting points for VCs, angels and scouts, founders, sales/BD and operators, and works for any other role too.

It only reads your email. It never sends, replies to, moves or deletes anything.

## What you need

- The **Claude desktop app** (Inbox Radar is built for it; it doesn't run in the browser)
- An email connector: **Outlook (Microsoft 365)** or **Gmail**
- Only for the add-on: **Notion** and/or your **CRM** connector

## Install

In the Claude desktop app, open a **Code** session and type:

```
/plugin marketplace add ChrissyVS/inbox-radar
/plugin install inbox-radar@inbox-radar
```

## Set up

Say **"set up inbox radar"**. Claude asks about your role, who you must never miss and what you want captured, then connects your email, saves your settings, does a dry run and offers to schedule itself.

Want logging into your CRM or Notion? Say **"add logging to inbox radar"**, during setup or anytime later.

## Use

- `/inbox-radar` or "run my inbox radar": normal run
- "inbox radar dry run": preview, writes nothing
- "change my inbox radar priorities": rerun part of the interview
- "add logging to inbox radar": set up the optional add-on

## Files

```
.claude-plugin/plugin.json          plugin manifest
.claude-plugin/marketplace.json     lets people install straight from this repo
skills/inbox-radar/SKILL.md         the workflow
skills/inbox-radar/references/      setup interview, role presets, classification rules, settings template
```
