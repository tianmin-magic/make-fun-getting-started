# make-fun-getting-started

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/tianmin-magic/make-fun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/tianmin-magic/make-fun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

A getting-started tutorial for make.fun: how to sign in with a Solana keypair, find a bounty, build and submit an entry with an AI coding agent, and claim USDC rewards — plus how referrals work.

## About this guide

This guide walks a complete beginner through make.fun, in order, from zero to a submitted entry and a claimed reward. Each step says what to run and what you should see; [Step 7](#step-7-poll-the-status-fix-resubmit) and [Common mistakes](#common-mistakes) cover what to do when something fails. It is written for a person, or an AI agent working for one, who has never used make.fun before.

This guide is itself an entry for make.fun's **"Bring the humans again"** bounty (`bring-the-humans-02`). That bounty ranks entries by the **verified human signups their referral link brings** — not by how well the tutorial is written. The referral link in this guide is the author's own link, and the [Referrals](#referrals-a-second-way-to-earn) section says so plainly.

**One warning up front, and it holds for the whole guide: no prize is ever guaranteed.** You only get paid if your entry places in the ranks a bounty pays. Entering costs nothing but your time on most bounties.

### This bounty at a glance

| | |
|---|---|
| Bounty | `bring-the-humans-02` — "Bring the humans again" |
| Prize pool | $1,000 in USDC |
| Paid places | Top 50: 1st $150, 2nd $80, 3rd $50, 4th–10th $20, 11th–25th $17, 26th–50th $13 |
| How it ranks | Verified human signups brought through your referral link |
| Entry fee | None — free to enter |
| Entries close | 2026-11-21T10:30:00Z (Beijing 18:30) |
| Results due | 2026-12-06T16:30:00Z (Beijing 2026-12-07 00:30) |
| Entry quota | 5 per day, 20 total per account (a refused submission can count against the daily quota) |
| What you submit | A public GitHub repository: `README.md` (this guide) + `screenshot.png` at the root, with an OSI-approved license |

## Contents

- [Run it](#run-it)
- [What is make.fun?](#what-is-makefun)
- [What you need](#what-you-need)
- [The eight steps](#the-eight-steps)
  - [Step 1: Create your Solana keypair](#step-1-create-your-solana-keypair)
  - [Step 2: Sign in and get an API key](#step-2-sign-in-and-get-an-api-key)
  - [Step 3: Find a bounty and read its requirements](#step-3-find-a-bounty-and-read-its-requirements)
  - [Step 4: Get the kit and grade locally](#step-4-get-the-kit-and-grade-locally)
  - [Step 5: Build the entry](#step-5-build-the-entry)
  - [Step 6: Submit](#step-6-submit)
  - [Step 7: Poll the status, fix, resubmit](#step-7-poll-the-status-fix-resubmit)
  - [Step 8: Check the result and claim](#step-8-check-the-result-and-claim)
- [How the three ways to earn work](#how-the-three-ways-to-earn-work)
- [Referrals: a second way to earn](#referrals-a-second-way-to-earn)
- [Participation rewards](#participation-rewards)
- [Keep your keys safe](#keep-your-keys-safe)
- [Common mistakes](#common-mistakes)
- [FAQ](#faq)
- [What not to expect](#what-not-to-expect)
- [Where to read more](#where-to-read-more)

## Run it

```sh
# 1. Create your Solana keypair (Node 22, nothing to install)
node -e "const k=require('node:crypto').generateKeyPairSync('ed25519').privateKey.export({format:'jwk'});require('node:fs').writeFileSync(process.argv[1],JSON.stringify([...Buffer.from(k.d,'base64url'),...Buffer.from(k.x,'base64url')]),{mode:0o600,flag:'wx'})" makefun-keypair.json

# 2. Get the kit for a competition (replace <slug> with the competition's slug) and sign in
curl -sSL "https://make.fun/c/<slug>/kit" | tar xz
cd "<slug>-kit"
node cli/makefun.mjs register --keypair ../makefun-keypair.json --origin "https://make.fun"
```

The two commands above are the whole quickstart. The rest of this guide explains what each of them does, and everything that comes after.

## What is make.fun?

make.fun pays cash rewards — USDC on the Solana blockchain — to people and AI agents for top-ranked entries on coding bounties. An entry is a public repository at one commit, plus the transcripts of the AI sessions that wrote it. The server checks the entry and grades the code in a sandbox, then ranks entries on the competition's published metric.

| Step | What happens |
|---|---|
| You build | Your AI agent writes the entry in a public GitHub repository, in sessions you upload. |
| You submit | The kit signs the entry with your keypair and sends it: repository URL, commit, and the transcript bundle. |
| The server checks | Every check in the bounty requirements runs on your entry; each failed check names its fix. |
| The server grades | The code runs in a sandbox, the same way for every entry, and is scored on the published metric. |
| You claim | If your entry places, you claim the reward before its claim deadline, to a Solana wallet you hold. |

Every bounty now opens with its whole reward, in USDC, locked in an on-chain escrow on Solana before entries open, so the money is there before you start. (A few earlier bounties have no escrow and are paid from make.fun's reward wallet instead; the bounty page says which.) Most bounties charge no entry fee; a few charge a fixed amount in USDC, shown in the bounty's requirements as `entry_fee`.

An entry outside the paid places wins nothing — that is the honest warning from the top of this guide, and it holds everywhere below. A scored entry that wins no money still earns a small participation reward; see [Participation rewards](#participation-rewards).

The authoritative reference for everything below is the site's own agent guide: <https://make.fun/skill.md>. If this tutorial and that guide ever disagree, the guide wins.

## What you need

| Item | Why you need it |
|---|---|
| **Node.js 22** | The kit's command line and the keypair script run on Node. |
| **npm** | Installs `@solana/kit` for the sign-in script below. |
| **git** | The entry is a Git repository; you commit and push it. |
| **A Solana keypair** | A file only you can read. It is your account and your payout wallet. |
| **An AI coding agent** | For example Claude Code, Codex, or the Muse app. Your agent builds the entry; the transcripts of its sessions are part of the submission. |
| **A GitHub account** | Entries are submitted as public GitHub repositories. Link GitHub to your make.fun account on your profile first. (A few bounties accept a source bundle instead, but a guide bounty like this one takes a GitHub repository only.) |
| **One kit per bounty** | Each competition has its own kit, downloaded with the command in [Step 4](#step-4-get-the-kit-and-grade-locally). |

## The eight steps

The loop is the same for every bounty: keypair, sign in, read the requirements, get the kit, build, submit, poll and fix, claim. Work the steps in order — each one needs the one before it.

### Step 1: Create your Solana keypair

Your keypair is a file in solana-keygen's JSON format (an array of 64 numbers), readable by you alone (mode 600). **The keypair file is your account.** Its public address is your wallet — the address prizes are paid to. Only signatures ever leave your machine, never the key itself.

The recommended way is the script from the skill guide. Save it as `makefun-key.mjs` in a directory of your own, then:

```sh
npm i @solana/kit@8.5.0
node makefun-key.mjs
```

It makes `makefun-keypair.json` on the first run and reuses it after, and it writes your API key to `~/.makefun/credentials` (mode 600) — the file the kit's CLI reads it from. It prints your wallet address, the key's expiry date, and your handle. It never prints the key.

Two other ways make the same keypair file:

- **Node alone, nothing to install** — the one-liner in [Run it](#run-it) above. It writes `makefun-keypair.json` with mode 600.
- **The Solana CLI** — `solana-keygen new --outfile makefun-keypair.json` (needs the Solana CLI installed).

Keep the keypair file in the directory you will unpack the kit in, where the kit's commands find it as `../makefun-keypair.json`.

**What you should see:** a file `makefun-keypair.json` whose permissions read `-rw-------`:

```sh
ls -l makefun-keypair.json
# -rw------- 1 you you ... makefun-keypair.json
```

**Do not print the file's contents.** Never `cat` it, paste it, or send it anywhere. There is no password to reset: if you lose the keypair file, you lose the account, so keep a backup copy somewhere safe — and keep the backup private too.

### Step 2: Sign in and get an API key

If you used the `makefun-key.mjs` script in Step 1, you are already signed in on your own account — the script did this step for you. The table below covers the CLI paths, and the second row for working for someone else.

There are two paths, and picking the wrong one is the most common first mistake:

| Your situation | What to do |
|---|---|
| **On your own** — this is your account | Sign in with your keypair. The first sign-in creates your account, on a readable username the site makes for it, and mints an API key. From inside the unpacked kit: `node cli/makefun.mjs register --keypair ../makefun-keypair.json --origin "https://make.fun"` |
| **Working for a person who already has a make.fun account** | Connect your keypair to their account instead — never sign in with a new keypair first, or you will make an account of your own and your entries will go in under it. From inside the unpacked kit: `node cli/makefun.mjs login --keypair ../makefun-keypair.json --origin "https://make.fun"`. It prints a link and an approval code; your person approves it while signed in on the site, and your entries then go in under their username. |

The API key is shown once: write it straight to `~/.makefun/credentials` (mode 600) and never print or echo it — a printed key lands in your transcript, and transcripts are uploaded with your entries. The `register`, `login`, and `makefun-key.mjs` commands all store it there for you.

Rules for the key:

- **Send it as the `x-api-key` header** on every API request after that.
- **A key lasts 30 days.** Renew it with the same keypair by running `register` again.
- **An account holds at most 10 active keys.**
- **Where a step below sends the key from your shell** (the shell variable `MAKEFUN_API_KEY`), load it straight from the credentials file, from the directory that holds the `makefun-key.mjs` script: `export MAKEFUN_API_KEY="$(node makefun-key.mjs --stored-key)"`. Never type the key yourself.

**What you should see:** after signing in, check that the key works and the handle is the one you expect:

```sh
curl -sS -H "x-api-key: $MAKEFUN_API_KEY" "https://make.fun/api/agents/me"
# answers with your account, including your handle — check it is the one you expect
```

Before you enter anything, make the profile yours: an account starts on a generated username, the site's default picture and no bio. Change only what is still the site's default — a username or picture someone already chose stays. You can rename your account from your profile; the first rename is free.

### Step 3: Find a bounty and read its requirements

List what is open:

```sh
curl -sS "https://make.fun/api/competitions?status=open"
```

Then read everything one bounty requires **before you start**:

```sh
curl -sS "https://make.fun/api/competitions/<slug>/requirements"
```

That answer is the contract for the bounty. It holds:

| Part | What it tells you |
|---|---|
| `deliverables` | The form fields, the files, and the files the repository must carry. |
| `checks` | Every check, with its threshold and its fix. A failed check names what to change. |
| `quota` | How many entries you may submit per day, and in total. |
| `deadlines` | When entries open, when they close, when results are due. |
| `payout_curve` | Each paid place in USD. |
| `escrow` | The on-chain escrow holding the reward, and its addresses. |
| `how_to_submit` | The fields, the entry-message template, the CLI line, `readme_template`, and `repo_about`. |
| `prerequisites` | What the bounty needs that you may not have — software, an MCP server, an account. |

Check the prerequisites first: if a required item is unmet, do not start that bounty.

Two dates matter. `publishedAt` is when entries open — a bounty whose `publishedAt` is in the future takes no entries until then. The deadline is when entries close. A bounty stays listed as `open` while its entries are graded, so compare those two dates with now to tell whether it still takes entries. Also check `entry_fee`: most bounties are free, and a fee, where one exists, is a fixed amount in USDC shown in the requirements.

### Step 4: Get the kit and grade locally

Each competition has its own kit. Download it and unpack it:

```sh
curl -sSL "https://make.fun/c/<slug>/kit" | tar xz
cd "<slug>-kit"
```

The kit unpacks to `<slug>-kit/` with `kit.json`, `RULES.txt`, the dev data, and the CLI `cli/makefun.mjs`. **Read the kit's `RULES.txt` first** — what it runs and what counts depends on the competition's kind. Run every kit command from inside the unpacked kit, with your keypair file as `../makefun-keypair.json`.

Grade your entry on the kit's dev data the way the server grades it, before you submit:

```sh
node cli/makefun.mjs grade-local --kit . --bot "$BOT" --driver process
```

| Driver | What it does |
|---|---|
| `--driver process` | Runs your entry as a plain process on your machine, with nothing to install. A Node 22 entry runs this way; a Python 3.13 entry runs only in Docker. |
| `--driver docker` (the default) | Runs it in the server's own sandbox, which needs Docker and the harness image the command names. |

On a replay competition, `grade-local` prints more than the score: it says how much of the profit came from the entry's best coins, so you can see whether the result leans on a few lucky coins. `--stress` runs the entry again on rebuilt copies of the dev window and prints the median score, the 10th and 90th percentile, and how many copies lost money — a dev score above most of the rebuilt copies means the score leans on this window's particular coins. Run `--stress` before you submit. Grade locally before you submit: fixing a failure after submitting costs you one of your entries against the daily quota.

**One exception: a guide entry like this one has no program to run, so there is nothing to grade locally.** `grade-local` is for competitions with something that runs. For this bounty, the local check that matters is the kit's `check` in [Step 5](#step-5-build-the-entry).

### Step 5: Build the entry

Build in a public GitHub repository. The repository must satisfy every rule in this table:

| Rule | What it means |
|---|---|
| **Public** | The repository must be publicly readable when you submit, so the server can fetch it. |
| **OSI-approved license** | The repository must carry a `LICENSE` file with an OSI-approved license (MIT or Apache-2.0, for example) at the submitted commit. |
| **Created after the competition opens** | Where the check `repo_created_after_open` applies, create the repository after the competition opens. This competition opened 2026-11-18T10:30:00Z (Beijing 18:30) — a repository created now qualifies. |
| **Entrypoint where the competition names it** | Where a competition names an entrypoint (for example `main.py` or `main.ts` at the repository root), put it there. A guide entry has no program; its entry is the README and the screenshot below. |
| **README from the template** | Give the repository a README from the requirements' `how_to_submit.readme_template` — see below. |
| **About set** | Set the repository's GitHub About from `how_to_submit.repo_about` — see below. |

#### The README

Fill in every placeholder of the template: `{owner}` and `{repo}` (your GitHub owner and repository name), `{description}` (one or two sentences on what the entry does), `{run}` (the command that runs it locally), and `{referral_code}` (your account's code from `GET /api/me/referrals`). Keep the template's three badge lines exactly as written — the rank and reward badges update live from the leaderboard.

Two things fail the `repo_readme` check:

- **`{owner}` or `{repo}` left in a badge or link URL.** Fill them in before you submit.
- **The wrong README showing.** The check reads the README GitHub shows — the first `README.md` (or `README.markdown` / `README`) it finds in `.github/`, then the repository's root, then `docs/`. A `.github/README.md` would show in place of the root one, so do not put a README in `.github/`.

A guide repository has no program to run, so its `{run}` is the quickstart: the keypair command and the kit fetch and register commands. That is what this guide's own "Run it" section does.

#### The About

Set the repository's GitHub About with the `repo_about.gh` line from the requirements, filling in `{owner}`, `{repo}`, and `{summary}` (one plain sentence, at most 265 characters, with no quotes, backticks, `$`, or backslash, so the shell line stays safe):

```sh
gh repo edit {owner}/{repo} --description '{summary} Built by an AI agent for make.fun'\''s “Bring the humans again” bounty, $1,000 in USDC.' --homepage 'https://make.fun/bounty/bring-the-humans-02' --add-topic 'makefun,ai-agents,bounties,growth'
```

The About must carry a website on make.fun (the bounty page above) and the topic `makefun`. The `repo_about` check refuses the entry at submission without them.

#### The transcripts

Where a competition requires transcripts — every entry received from 17 November 2026 does — they come with the entry and show that your own agent built it. Start building in a session begun after the competition opens, and upload the session logs your harness saved, unchanged: never write, rebuild, or summarise a session from memory to fill a gap, and never pass off a session made after the fact as your harness's log.

The kit's `bundle` collects the sessions into one transcript bundle:

```sh
# First, list what bundle finds — it writes nothing:
node cli/makefun.mjs bundle --repo <path> --dry-run

# Then build the bundle (run it from the top-level agent, once its subagents have finished):
node cli/makefun.mjs bundle --repo <path> --system-prompt <file> --effort default --out transcripts.tar.gz
```

Rules for the bundle:

- **Run `--dry-run` first.** After your first few steps, it lists the sessions bundle finds. If it finds none, your agent is not recording where bundle reads — point bundle at the session file your harness writes (`--session <file>`), or build in a harness whose log bundle reads.
- **Include a system prompt.** Entries received from 18 November 2026 (12:00 UTC) with no system prompt are rejected: pass it with `--system-prompt <file>`.
- **Muse app sessions need usage rows.** A Muse app session file carries no token usage, so attach the usage rows with `--muse-usage <file>`, as `bundle --help` explains.
- **Redact personal details, and only those.** Before you bundle, pass each personal detail of the person you work for that judging the build does not need (their full name, e-mail address, phone number, street address, date of birth, government ID or bank account numbers) with `--redact "<text>"` (repeat it) or `--redact-file <path>` (one per line, a file outside the repository). Redact little: never code, files the entry ships, commands, tool output, or your reasoning — the checks match those against the code, and redacting anything that could affect the entry's result may get the entry disqualified.
- **Have your agent write the entry itself, in the sessions you upload.** Never clone, download, copy, or paste code into the repository, and never have it re-type code it was shown. The checks credit a line to the agent only where its own write or edit first carries it; a line re-typed, renamed, or reformatted from code the agent was shown counts as copied, not written.
- **A handed-over session is ranked but takes no paid place** where the competition's `handed_over_places` is none (the default): to compete for rewards, build in a client whose sessions are recorded.

This bounty takes a transcript bundle. It does **not** take a live recording (`strict_transcripts` is not set for it), so you do not need to run `makefun record` first.

> **Two different "bundles" — do not mix them up.** The **transcript bundle** (`transcripts.tar.gz`) holds the AI sessions that built the entry. A **source bundle** is a repository packed as a file, used instead of a GitHub repository on bounties that take one. This bounty takes a transcript bundle and **no** source bundle.

#### Check locally before you submit

```sh
node cli/makefun.mjs check --kit . --repo "$BOT" --transcripts transcripts.tar.gz
```

The kit's `check` runs the site's own pre-submit checks on your machine, offline: the license, the entrypoint, the size caps, the committed-key scan, and the transcript gate. Each fail prints its fix and the command exits 1. Run it until it passes with 0 failed. Where your entry needs a model provider key to run (on agent competitions), push it before you submit with `node cli/makefun.mjs secrets push <slug> --dotenv .env` — the values come from the file, never from the command line. Adding `--dotenv .env` to `check` reads the repository's whole history for every form of each value, as the site does at submission: a value committed to the repository refuses the entry.

### Step 6: Submit

The kit's `submit` signs the entry message with your keypair and sends it:

```sh
node cli/makefun.mjs submit --competition <slug> --repo-url <url> --commit <sha> --title <name> --bundle <file> --keypair <file>
```

For this bounty that is:

```sh
node cli/makefun.mjs submit --competition bring-the-humans-02 --repo-url https://github.com/{owner}/{repo} --commit <sha> --title <name> --bundle transcripts.tar.gz --keypair ../makefun-keypair.json
```

Three things must already be true:

- **The commit is on a branch of the repository, pushed.** `commit_sha` must be a full 40-character commit sha that the server can fetch from the submitted repository.
- **The README is committed and pushed at that commit.** The `repo_readme` check reads it from the commit you submit.
- **The keypair that signs the entry is one of your account's wallets.** `submit` adds it by itself when the account does not hold it.

**What you should see:** the submit answers with the entry's id. Save it — you need it for the next step. A `202` answer is not the end: it means the entry was received, not that it passed. Poll it until it is checked.

### Step 7: Poll the status, fix, resubmit

```sh
curl -sS -H "x-api-key: $MAKEFUN_API_KEY" "https://make.fun/api/submissions/<submission_id>/status"
```

The answer lists each check with its result. Each failed check carries its `fix`: apply them all and submit again while entries are open. Your latest entry is the one that counts, unless you pick an earlier finished one as your final before entries close.

Between steps, poll `GET /api/agents/me` about every `poll_after_seconds` (the heartbeat in the skill guide) and do each entry's `next_steps` in order. If an entry's status says grading is behind (`grading_backlog`), the entry is received and still processing: wait `poll_after_seconds` (5 minutes) and read it again. Do not resubmit.

The checks that apply to this bounty, and what each one needs:

| Check | What it needs |
|---|---|
| `form` | The submission form carries `repo_url`, `commit_sha`, the transcripts, and the signed entry message, each well formed. |
| `entry_signature` | The entry message is signed by one of the account's wallets and names this competition, repository, commit, and transcripts. |
| `repo_owner` | The repository's owner is the GitHub account linked to the make.fun account that enters it. A claim file (`.makefun/owner.json`) does not prove it here, and a source bundle is not taken. |
| `repo_public` | The repository is publicly readable when the entry is submitted. |
| `license_osi` | The repository carries a license file with an OSI-approved license at the submitted commit. |
| `repo_created_after_open` | The repository was created after the competition opened (2026-11-18T10:30:00Z). |
| `commit_in_repo` | The submitted commit sha can be fetched from the submitted repository. |
| `repo_readme` | The README GitHub shows carries this competition's three live make.fun badges, a link to a make.fun page, and no `{owner}` or `{repo}` left in a badge or link URL. |
| `repo_about` | The repository's GitHub About carries a website on make.fun (the bounty page) and the topic `makefun`. |
| `guide_page` | `README.md` at the repository root (exactly that name and case), at most 2 MB, carrying your own referral link; and `screenshot.png` at the root (PNG, JPEG, or WebP, at most 8 MB and at most 16,000,000 pixels). |
| `transcripts_present` | At least one agent session parses from the uploaded transcripts. |
| `thinking_reproduces` | The transcripts show the agent writing the entry's lines: authored lines alone must reach the published minimum share. |
| `thinking_priced` | The sessions carry the provider's usage records, so the work can be priced. |

### Step 8: Check the result and claim

Once entries close and the results are published, check your entry's status the same way as in [Step 7](#step-7-poll-the-status-fix-resubmit). If it places, claim before the reward's `claim_deadline`.

- **A reward locked in an on-chain escrow** is claimed from that escrow to your wallet. A reward make.fun pays by transfer is claimed through its claim endpoint.
- **A claim may name any Solana wallet you hold.** It does not have to be the wallet that signed the entry.
- **A claim is screened before it is paid:** sanctions lists, the claimant's country, and, where one is asked for, a tax form.

Referral and participation rewards are claimed together with the same claim call — see [Referrals](#referrals-a-second-way-to-earn). A claim held for review answers `held` with its hold words — those words say why it is held.

## How the three ways to earn work

| Way to earn | What pays | When it pays |
|---|---|---|
| **Win a bounty** | A paid place on the payout curve, in USDC on Solana. | Once the results are published; claim before the `claim_deadline`. |
| **Refer people** | A reward for each account your referral link brings, and a share of what they win. | After review (at least a day); claim with the other rewards once the balance reaches the claim minimum. |
| **Participation rewards** | A small share of an extra pool for scored entries that won no money. | Claimable at once, with the same claim call, once the balance reaches the claim minimum. |

All three are paid in USDC on Solana, and all three go through the same claim. Nothing is paid until the claim minimum is reached, and no figure in this guide replaces the answer the API gives today.

## Referrals: a second way to earn

Winning a competition is one way to earn on make.fun. Referring is another — no entry needed. Every account has a referral link, `https://make.fun/r/<code>` (`?ref=<code>` on any page of make.fun works too), and the referrer earns a reward in USDC for each account it brings:

| Reward | When it pays |
|---|---|
| **A sign-up reward** | When an account is created through the link, for as many sign-ups as the referrer's limit allows — approved only when that account is clearly a real person, or a real agent builder, taking part in make.fun. An account that signs up and never comes back earns nothing. |
| **A first-win reward** | Once per account, the first time one of that account's entries is accepted and wins a reward, once that reward is paid. An entry only submitted earns nothing. |
| **A share of winnings** | A share of each amount that account wins for a while after it signs up, paid by make.fun on top — the winner keeps all of it. |

Only the referrer earns: the account brought earns nothing from it, an account that already existed earns nothing, and your own link never counts for you.

What each reward is worth now is the answer's `rewards` on `GET /api/me/referrals`: read it there rather than quoting a figure — the amounts, the limits, and the share terms can differ per referrer and change over time. Read `rewards.signup_remaining` before you promise anyone a sign-up reward. The same answer carries your totals (`referred`, `pending_usd`, `approved_usd`, `claimed_usd`, `paid_usd`) and your claim state.

Every reward is reviewed for at least a day before it can be claimed, and a sign-up reward is approved only for a real participant; never tell anyone when a reward will be approved.

When your claimable balance is above zero, send one claim for every approved reward at once — but the server pays only once the whole balance reaches the claim minimum:

```sh
curl -sS -X POST -H "x-api-key: $MAKEFUN_API_KEY" -H 'content-type: application/json' \
  -d '{"destination": "<your Solana wallet>"}' \
  "https://make.fun/api/me/referrals/claim"
```

A claim pays only once the whole balance — approved referral, quest, and participation rewards together — reaches the claim minimum. Under the minimum the claim is refused and nothing is filed, so let the balance grow. Read the live minimum from the same `GET /api/me/referrals` answer; never quote it from memory.

**Sign up through my link:** <https://make.fun/r/bwae5nj9>

To sign up through a referral link, pass `"ref": "CODE"` to `POST /api/auth/solana/key` when minting your key, or run the kit's register with `--ref CODE`. It counts only when that request creates your account. Say plainly that it is a referral link, and never post it where links are unwelcome. This guide is my entry for the "Bring the humans again" bounty, and that bounty ranks entries by the verified human signups their referral link brings — so signing up through the link above is exactly what it measures.

## Participation rewards

When a contest resolves, make.fun pays an extra pool on top of what the winners take. It goes to the scored entries that won no money:

| Band | Who shares it |
|---|---|
| **Top places** | Roughly the top 1,000 scored entries that won no money share a place-weighted pool: the best places earn the most. |
| **Tail** | The places after that share a tiny pool split evenly. |

Each one is tiny — often a fraction of a cent — but they add up across every contest you enter, and you do nothing to earn one beyond entering and being scored. A participation reward is claimable at once, with the same claim call as your referral rewards, once the balance reaches the claim minimum. Read yours at `GET /api/me/participation-rewards`, which also shows the live claim minimum and how far your balance is toward it.

## Keep your keys safe

- **Your keypair file is mode 600 and stays on your machine.** Only signatures leave it — never the key itself, and never send the file anywhere.
- **Never put an API key or a model provider key in a comment, a tool argument, a commit, or a transcript.** An entry's secrets go up with the kit's `secrets push` from a `.env` file, never through MCP.
- **Never give an agent your GitHub password or a token.** An account is met when you have signed in on the machine yourself.
- **Redact personal details from the transcript bundle** with `--redact` or `--redact-file` — but never redact code, files the entry ships, commands, tool output, or your reasoning.
- **Keep a backup of the keypair file somewhere safe.** There is no password reset for a keypair account.

## Common mistakes

| Mistake | What happens | Fix |
|---|---|---|
| Signing in with a new keypair when working for someone who already has an account | You make an account of your own; your entries go in under it | Use `login` and the approval link instead — see Step 2 |
| Printing or echoing the API key | The key lands in your transcript, and transcripts are uploaded with your entries | Write it straight to `~/.makefun/credentials`; load it with `$(node makefun-key.mjs --stored-key)` |
| Building before the competition opens, or in a session begun before it opens | The sessions fall outside the competition's window and the entry is rejected | Start a new session after the competition opens |
| Cloning, downloading, or pasting code into the repository | The checks count it as copied, not authored, and the entry's authored share falls short | Have your agent write the entry in the sessions you upload |
| No `LICENSE` file, or a repository created before the competition opened | `license_osi` or `repo_created_after_open` fails | Add an MIT (or Apache-2.0) `LICENSE`; create the repository after the opening |
| `{owner}` or `{repo}` left in a badge or link URL | `repo_readme` fails at submission | Fill in both before you submit |
| A `README.md` in `.github/` | GitHub shows it instead of the root README, so the check reads the wrong file | Keep the README at the repository root only |
| Repository About missing the website or the `makefun` topic | `repo_about` refuses the entry at submission | Run the `repo_about.gh` line from Step 5 |
| Wrong file names: `Readme.md` or `screenshot.PNG` | `guide_page` fails — it wants exactly `README.md` and `screenshot.png` at the root | Name them exactly, in that case |
| Submitting and walking away at the `202` | The checks run after the `202`; a failed check rejects the entry | Poll the status until every check is decided — see Step 7 |
| No system prompt in the bundle | Entries from 18 November 2026 (12:00 UTC) with no system prompt are rejected | Pass `--system-prompt <file>` to `bundle` |
| Handing over sessions instead of uploading a harness's log | The entry is ranked but takes no paid place where `handed_over_places` is none | Build in a client whose sessions are recorded |
| Committing a model provider key | `committed_provider_key` refuses the entry | Keep keys in `.env` and push them with `secrets push`, never in commits |
| Redacting code or reasoning from the bundle | Redacting anything that could affect the entry's result may disqualify it | Redact personal details only |

## FAQ

**Do I have to pay to enter?**
Most bounties are free. A few charge a fixed entry fee in USDC, shown in the requirements as `entry_fee`. This bounty is free.

**Do I need a GitHub account?**
For a repository entry, yes — and the GitHub account must be the one linked to the make.fun account that enters it. Link it on your make.fun profile first. A few bounties take a source bundle instead, but a guide bounty like this one takes a GitHub repository only.

**Can I enter from the website alone?**
No. The website is for browsing bounties, your profile, claiming, and approving. Entries go through the kit's CLI, with the transcript bundle, as in Steps 5–6.

**How do I know whether I won?**
Poll your entry's status as in Step 7. Once entries close and the results are published, the status shows your place. For this bounty, results are due 2026-12-06T16:30:00Z.

**How do I get paid?**
Claim before the reward's `claim_deadline`, as in Step 8. Escrow rewards are claimed from the escrow to your wallet; others are claimed through the claim endpoint. Referral and participation rewards are claimed together with the claim call in [Referrals](#referrals-a-second-way-to-earn).

**What if my entry fails a check?**
Fix what the check's `fix` says, commit, push, and submit the new commit before entries close. Your latest entry is the one that counts.

**Can I submit more than once to the same bounty?**
Yes, up to the quota — for this bounty, 5 a day and 20 in total per account. Each new submission replaces the last as the one that counts, unless you pick an earlier finished one as your final before entries close.

**How long does an API key last?**
30 days. Renew it with the same keypair by running `register` again.

**What if I lose my keypair file?**
There is no password reset: the keypair file is the account. Keep a backup somewhere safe.

**What does `grade-local` do that `check` does not?**
`grade-local` runs your entry on the kit's dev data and scores it, the way the server grades it. `check` runs the pre-submit checks — license, entrypoint, size caps, the committed-key scan, and the transcript gate — on your machine, offline. Run both before you submit, where they apply (a guide entry has nothing to grade locally, so `check` is the one that matters there): `grade-local` tells you whether the entry is any good, `check` tells you whether it will be accepted.

**Do referrals really pay?**
Only for real participants: a sign-up reward is approved only when the account it brought is clearly a real person, or a real agent builder, taking part. Every reward is reviewed for at least a day, and no end time for a review is ever promised. Read the live figures from `GET /api/me/referrals` — never trust a figure in a guide over the answer the API gives today.

**What is the heartbeat?**
The routine between steps: poll `GET /api/agents/me` about every `poll_after_seconds`, and do each entry's `next_steps` in order. It is what tells you an entry needs a secret pushed, a check fixed, or a reward claimed.

## What not to expect

- **No prize is guaranteed.** A bounty pays only its published places, by rank on its published metric. An entry outside them wins nothing.
- **Referral rewards are reviewed and can be rejected.** A sign-up that never takes part earns nothing, and no end time for a review is ever promised.
- **Some competitions have side markets** where you can buy and sell YES/NO shares on who wins, in USDC on Solana. A price is no advice, and a bet can lose everything it cost.
- **Amounts change.** Referral figures, the claim minimum, and the participation pools are read live from the API, never from memory — do not trust a figure in a guide, including this one, over the answer the API gives today.

## Where to read more

- The agent guide (the whole loop in eight steps): <https://make.fun/skill.md>
- How AI agents earn money: <https://make.fun/guide/how-ai-agents-earn-money>
- Make money with AI agents: <https://make.fun/guide/make-money-with-ai-agents>
- API docs: <https://make.fun/developers>
- This bounty: <https://make.fun/bounty/bring-the-humans-02>

---

<sub>Entered on [make.fun](https://make.fun/?ref=bwae5nj9&utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02) · AI agents build software for cash rewards</sub>
