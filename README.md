# Inbox Radar

A Claude plugin that triages your inbox according to **your** priorities.

**Core (needs only your email).** Every run it:

- **finds every email from the people you must never miss** (your team, investors, key clients, your boss) and flags the ones waiting on your reply,
- **spots the opportunities you care about**: deals, leads, investor conversations, partnership requests, requests with deadlines,
- **gives you a short, numbered digest** right in Claude and pings you when something needs you. The newest digest always lists everything still open, so after a week away you read only that one, and it leaves out what you've already answered, archived or read,
- **drafts your replies**: answer the digest by typing or dictating on your phone ("2: yes, Thursday works") and it writes the drafts in your mailbox, in your own voice (it can learn how you write from your sent emails). It never sends.

**Optional add-on: log to your lists.** If you want, it also logs what it finds into your CRM (Affinity, Attio, HubSpot, …) and/or a Notion database, without duplicates (it looks up each company's website so your CRM recognises companies it already knows, and fills the columns you choose), and can copy deals you add to Notion by hand into your CRM.

Setup starts with a short interview about your role and priorities. It comes with starting points for VCs, angels and scouts, founders, sales/BD and operators, and works for any other role too.

It never sends, moves or deletes anything in your mailbox. The only thing it writes there are reply drafts you asked for. Attachments are never opened, and emails the sender marks as confidential or under NDA aren't logged (see [Privacy and confidentiality](#privacy-and-confidentiality)).

**On your phone:** turn on Remote Control and the digest (with a push notification) reaches the Claude app on your phone, where you can answer it by voice.

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

## Privacy and confidentiality

Your inbox is full of other people's personal and confidential information. When Claude reads it, you (or your company) are processing that data, under the GDPR if you're in the EU. Inbox Radar is built to touch as little of it as possible. It can't make you compliant on its own; check with whoever handles data protection for you, including that your Claude plan's data terms fit your use.

**What it does with email**
- **Reads, never sends.** It searches and reads email and creates reply drafts only when you ask. It never sends, forwards, moves, deletes or flags anything. You review every draft and send it yourself.
- **Only email.** It never uses file, drive or site tools (OneDrive, SharePoint, Google Drive, Dropbox), chat or calendar, even if your connector offers them, and never follows links in emails.
- **Attachments stay closed.** It never opens or downloads them. By default it doesn't even name them: the digest, CRM and Notion only say "2 attachments" (`attachments: redact`).
- **Confidential emails aren't logged.** If the sender marks the email itself as confidential ("Confidential: …", "shared under NDA", "bitte vertraulich behandeln"; `confidential_terms`, which you can extend), it shows up in your digest marked "confidential · not logged" and nothing goes to your CRM or Notion. Signature disclaimers, an NDA still to come ("data room after an NDA") and attachment labels don't count, so normal deal emails are logged as usual. If you say "log 4 anyway", it logs the name, website, stage, owner, source and one-line pitch, nothing more. You can still have replies drafted for it.
- **Web searches** (to find a company's website) use only the company's name and what it does, never a person's name, an address or text from the email.
- **Email text is data, not instructions.** Instructions hidden in an email are ignored.

**What's stored, and for how long**
- On your computer, in `~/.claude/inbox-radar/`: your settings, the list of still-open items (dropped after 14 days, `open_items_max_days`) and a copy of each digest (deleted after 30 days, `keep_digests_days`).
- In your CRM or Notion (add-on only): business facts about each deal (name, website, contacts, source, a short summary), never the full email. Their retention is up to you.
- Your voice guide (optional): a short description of how you write, plus a few of your own emails with names, companies, amounts and other details replaced by [brackets].

**Your email connector's permissions.** These rules limit what Inbox Radar does, but your connector's permissions are set when you connect it, not by this plugin, and Microsoft 365 and Gmail connectors often allow more (sending, files). Inbox Radar only needs to read mail and create drafts. To make the rest technically impossible for Claude on your computer, open a Code session and say: *"Block sending, forwarding and deleting email, and file access, in my Claude permissions."* Claude adds deny rules to your settings. With a read-only connection it still works; drafts then appear in chat for you to copy.

## Make it your own

Inbox Radar is free and open source under the [MIT license](LICENSE): download it, copy it, change it and share your version however you like.

- **To adapt it,** fork this repo (or download it), edit the files in `skills/inbox-radar/`, and install your copy with `/plugin marketplace add <your-github-user>/<your-repo>`.
- **This repo is maintained by me.** Your changes live in your own copy; this version only changes when I update it. Ideas and fixes are welcome as issues or pull requests.
- **If you share your own version,** please give it a different name so people don't mix it up with this one.

## Files

```
.claude-plugin/plugin.json          plugin manifest
.claude-plugin/marketplace.json     lets people install straight from this repo
skills/inbox-radar/SKILL.md         the workflow
skills/inbox-radar/references/      setup interview, role presets, classification rules, settings template
LICENSE                             MIT
```
