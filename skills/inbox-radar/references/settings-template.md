# Settings template

Settings are YAML. They live in one of two places (`home`):

- `local` (default): the file `~/.claude/inbox-radar/settings.yaml`. Digests go to `~/.claude/inbox-radar/digests/`.
- `notion`: the YAML block under "Settings" on a Notion page titled exactly **Inbox Radar – Settings & Digests**.

Either way, each digest is the run's message in Claude, with a local copy in `~/.claude/inbox-radar/digests/`. The list of still-open items, which keeps the newest digest complete, lives in `~/.claude/inbox-radar/open-items.json`. With `digest_in_notion: true` it's also saved as a child page of the Notion settings page.

## Core settings (email only)

Fill these from the setup interview. Example values are for a VC; replace them:

```yaml
home: local                    # local | notion
role: vc                       # vc | angel | founder | sales | operator | other
my_email: you@yourfund.com
my_name: Alex                  # used to sign reply drafts
voice_guide: none              # optional: Notion page URL or file path describing how you write

# People you must never miss
must_see:
  - group: Colleagues
    domains: [yourfund.com]
    addresses: []
    top: true                  # always listed first

# What to capture (shown in your digest)
trackers:
  - name: Deals
    key: company
    counts: founder pitches; intros or co-invest invitations from other investors; companies forwarded by colleagues
    not: portfolio company updates; other funds pitching for an LP commitment; vendors; event invites; job applications
    extract: [website, founders, round, round_size, geography, sector]

# Never surface these
ignore: newsletters; event marketing; recruiters

# Behaviour
notify: needs_you_and_new      # needs_you | needs_you_and_new | off
digest_in_notion: false        # true = also save each digest as a Notion page
first_run_lookback_hours: 24
open_items_max_days: 14        # how long an unanswered item stays in the digest

# Privacy (defaults apply when these are missing)
attachments: redact            # redact = never opened, never named anywhere | names = file names may appear
confidential_terms: [confidential, vertraulich, strictly private, under NDA, subject to NDA, do not forward, do not share, nicht weiterleiten, privileged]
keep_digests_days: 30          # local digest copies older than this are deleted

# Updated automatically after each run. Don't edit.
last_run: never
```

## Optional add-on: log to your lists

Add a `destination` block to any tracker whose matches should be logged. Leave it out to keep that tracker digest-only.

```yaml
    destination:
      crm:
        system: affinity       # affinity | attio | hubspot | salesforce
        list: "Deal Flow"
        stage: New
        owner: you@yourfund.com  # optional: set as owner on new entries
        currency: EUR          # optional: amounts in another currency aren't written to number fields
        field_map:             # optional, skill field: your CRM column (list fields)
          stage: Status
          owner: Owners
          pitch: Short Pitch   # one line on what the company does
          round: Stage         # e.g. Seed, Series A
          round_size: Round Size
      notion:
        database: collection://...
        owner_user_id: ...
        stage: Intake
        field_map:             # skill field: your Notion column
          name: Startup
          stage: Stage
          owner: Owner
          website: Website
          source: Source
          summary: AI summary
          round: Round
          round_size: Round Size
          geography: Geography
          sector: Sector
      # Optional: also copy entries you add to Notion by hand into the CRM (one-way)
      sync_notion_to_crm:
        enabled: false
        stage: Intake          # only Notion pages at this stage
        since: 2026-01-01      # only pages created on or after this date
        owners: []             # optional: only pages owned by these Notion user IDs (empty = everyone's)
```

## Notion page layout (only for `home: notion`)

````
Settings for Inbox Radar. Edit the values below; keep the key names. To change your priorities, edit here or tell Claude "change my inbox radar priorities".

## Settings

```yaml
<the YAML above>
```

## Digests

New digest pages appear below.
````

## Keys

| Key | Meaning |
|---|---|
| `home` | Where settings and digests live: `local` or `notion`. |
| `role` | Which preset setup started from. Informational. |
| `my_email` | The user's address. Their own emails are skipped; used to find their replies. |
| `my_name` | The first name used to sign reply drafts. |
| `voice_guide` | Optional. A Notion page or file describing how the user writes, built from their sent emails (setup step 6c). Reply drafts follow it. |
| `digest_in_notion` | Also save each digest as a Notion page (needs `home: notion` or a Notion connection). Default `false`. |
| `must_see` | Groups of people never to miss, by `domains` and/or exact `addresses`. `top: true` lists a group first. |
| `trackers` | Kinds of incoming opportunity to capture. See below. |
| `ignore` | Kinds of email never to surface, in plain words. |
| `notify` | When to send a notification. |
| `first_run_lookback_hours` | How far back the very first run looks. |
| `open_items_max_days` | How many days an unanswered email keeps showing in the digest. Default 14. |
| `attachments` | `redact` (default): attachments are never opened and only counted, never named, in the digest, CRM or Notion. `names`: file names may appear. Attachments are never opened either way. |
| `confidential_terms` | Words that, when the sender uses them about the email itself (subject or their own text), mark it as confidential. Signature disclaimers, an NDA still to come ("data room after an NDA") and attachment labels don't count. Such emails are shown in the digest but not logged to the CRM or Notion unless you say "log 4 anyway". Default: confidential, vertraulich, strictly private, under NDA, subject to NDA, do not forward, do not share, nicht weiterleiten, privileged. Add project code names or other words. |
| `keep_digests_days` | Local digest copies older than this many days are deleted. Default 30. |
| `last_run` | Timestamp of the last successful run. Written by the skill. |

### Tracker keys

| Key | Meaning |
|---|---|
| `name` | Shown as the digest section ("New deals", "New leads"). |
| `key` | What one item is: `company`, `person`, `firm` or `subject`. Used for dedupe. |
| `counts` / `not` | What qualifies and what doesn't, in the user's words. |
| `extract` | Extra fields to fill, beyond name, contact, website, source, summary and pitch: from the email, and for company facts (`geography`, `sector`, `thesis_tags`, `biz_model`, `technology`, `market`, `usp`, `team_details`) also from the company's website. |
| `destination` | **Add-on.** Leave out for digest-only. |
| `destination.crm` | CRM system, list or pipeline, starting stage, owner, and optionally `currency` and a `field_map` from skill fields (`stage`, `owner`, `pitch`, `round`, `round_size`, or anything in `extract`) to the list's column names. The pitch and other company facts come from the email plus a short look at the company's own website. |
| `destination.notion` | Database, owner, starting stage, and `field_map` from skill fields to Notion column names. |
| `destination.sync_notion_to_crm` | When `enabled`, each run copies Notion pages at `stage` created on or after `since` (and owned by `owners`, if set) into the CRM list if they're missing there. One-way; Notion is never changed. |
