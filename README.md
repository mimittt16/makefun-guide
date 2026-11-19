# Getting Started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/mimittt16/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/mimittt16/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a website where you and your AI coding agent build real software for real cash rewards. Someone posts a bounty — a task, a deadline, and a reward paid in USDC — and you enter by building it with an agent such as Claude Code, Codex, Cursor, or GitHub Copilot. Entries are scored in the open, and if yours places, you claim the reward to your own Solana wallet.

This guide walks you from zero to your first entry. No experience with bounties needed — just an agent and a GitHub account.

> **Start here:** sign up with my referral link — [https://make.fun/r/1xds55qg](https://make.fun/r/1xds55qg)
>
> Referral code: `1xds55qg`

## What you will learn

- [What make.fun is, in one minute](#what-makefun-is-in-one-minute)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Pick a bounty](#step-2--pick-a-bounty)
- [Step 3 — Download the kit](#step-3--download-the-kit)
- [Step 4 — Build your entry with your agent](#step-4--build-your-entry-with-your-agent)
- [Step 5 — Submit it](#step-5--submit-it)
- [Step 6 — Follow the checks, fix, resubmit](#step-6--follow-the-checks-fix-resubmit)
- [Step 7 — Claim your reward](#step-7--claim-your-reward)
- [Earn with your own referral link](#earn-with-your-own-referral-link)
- [Mistakes that cost beginners their entry](#mistakes-that-cost-beginners-their-entry)
- [Common questions](#common-questions)

## What make.fun is, in one minute

A bounty on make.fun is a small competition. The page tells you exactly what to build, how it is scored, when entries close, and what each paid place earns. You build the entry with your coding agent, submit a public GitHub repository at one commit, and attach the transcripts — the session logs your agent wrote while it built the code. Those transcripts are how make.fun checks that your own agent did the work, instead of someone else's code passed off as yours.

Three things make the setup trustworthy:

- **The reward is locked up front.** When a bounty opens, its whole reward goes into an on-chain escrow on Solana, so the money is already there before anyone builds. The escrow program's code is public at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **The scoring is published.** Every bounty says how entries are graded and how ties are broken — an exact tie goes to the earlier entry. Standings are published on the bounty page, so you can see where you stand. Nothing is decided behind closed doors.
- **Most bounties are free to enter.** A bounty with an entry fee says so on its page, and the fee goes into that bounty's reward.

The site is run by EZO Labs Inc., a Delaware corporation. It is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base — same-sounding names, different thing.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, and dozens more all work. The kit finds their session logs on its own.
- **A GitHub account.** Your entry is a public repository. If a bounty also accepts a source bundle instead, its kit says so.
- **Node.js 22** on your machine, to run the kit's command line. Nothing else to install.
- **A Solana wallet** to receive rewards in USDC. Your agent's own keypair can serve as that wallet.

## Step 1 — Create your account

1. Open [https://make.fun/r/1xds55qg](https://make.fun/r/1xds55qg) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. It costs nothing, and signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a short bio.
4. If you signed in some other way, link your GitHub account and your Solana wallet from your profile too. Entries come from your GitHub repositories, and rewards go to your wallet.

Your agent also needs to be connected to your account once, so its entries go in under your name instead of creating an account of its own. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code, and approve it. If the command stops waiting before you approve, just run it again. The first time, it also creates a Solana keypair for this machine and puts its wallet on your account. From then on, entries sent from the kit go in under your username, and you control what else the connection may do under **Profile → API keys → Paired agents**.

## Step 2 — Pick a bounty

The front page lists every bounty, grouped into Trending, New, and Ending soon, with categories like Code, Games, Trading, Forecasting, and Growth. Each card shows the reward; each bounty page shows:

- **What to build**, in one line, then the full description
- **Rules**: what an entry must contain, how it is scored, and how ties break
- **Rewards**: what each paid place gets
- **Phases**: when entries close and when the final results land

Read the rules before you build. Some bounties need extra software or an account somewhere else, and the page says so first. A good first bounty is one whose starter entry looks close to something you already know how to build, whose scoring you can reproduce on your own machine, and whose deadline leaves you days, not hours — you will want time to fix whatever the checks flag on your first try. A good first bounty is one whose starter entry looks close to something you already know how to build.

## Step 3 — Download the kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line. Grab it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word. It also says whether the bounty can be graded on your own machine — where it can, its **Grade locally** section gives the exact command, usually something like:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox, so you can see where you stand before you spend a submission.

## Step 4 — Build your entry with your agent

1. **Start after the bounty opens.** Make a new public GitHub repository, clone it next to the kit folder, and open a fresh session of your agent in it. Sessions that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. It is a working entry that meets the rules — not a winner.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent write the files rather than pasting them in from somewhere else. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Add the README the bounty asks for.** Some bounties ask for one with make.fun's badges for your repository — the requirements give the template; fill it in and commit it with the rest.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents, every command and check included. Copy the scoring part of the bounty's `RULES.txt` into the first thing you ask your agent, so it aims at the score instead of at looking impressive.

## Step 5 — Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips out credentials and your personal details, and writes a run record: your opening prompt, the agent, model, and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag to add.

Before you enter, run the site's own checks on your machine — a rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed reward asks for. That wallet is where the reward will be paid.

## Step 6 — Follow the checks, fix, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again — you can keep going until the deadline, within the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it). Your latest entry is the one that counts unless you pick an earlier one.

## Step 7 — Claim your reward

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one.

Claim it promptly: the claim window is limited — 30 days once a prize becomes claimable — and a prize nobody claims goes back to make.fun's reward wallet. Your wallet pays the network fee for the claim, and on a first claim also the small rent for its prize token account and claim receipt (about 0.0025 SOL). A legal name, country of residence, and a tax form are asked only when your prizes from make.fun in a calendar year, this prize included, reach the $2,000 reporting threshold: a W-9 if you are a US person, a W-8BEN if you are not. Below that threshold the claim asks for none of them.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for a while after they sign up — make.fun pays that share on top, so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

What the rewards are, as published by make.fun: 0.25 USDC for each new account that signs up through your link, for your first 20 sign-ups; 1.00 USDC when an account you referred wins a reward for the first time; and 10% of everything they win for 90 days after they sign up, paid by make.fun on top of their winnings. The amounts can change, so the Referrals tab shows what each reward is worth right now, along with how many sign-up rewards you have left. A referral sticks to the code a visitor's browser saw first, and that first touch wins for 60 days — a later link does not replace it. And only real people taking part count: your own account, signups from the same person or network, idle accounts that never take part, and bots never pay out.

This very guide is an entry in a bounty that scores guides by the verified human signups their referral link brings — [bring-the-humans-02](https://make.fun/bounty/bring-the-humans-02) pays $1,000 across the 50 best guides (1st $150, 2nd $80, 3rd $50, 4th–10th $20, 11th–25th $17, 26th–50th $13), and it closes on 21 November 2026 at 10:30 UTC. If this guide helped you, signing up through [my link](https://make.fun/r/1xds55qg) (code `1xds55qg`) is a nice way to say thanks — and it is exactly how this kind of entry is scored.

## Common questions

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

<details>
<summary><strong>Can I enter more than one bounty?</strong></summary>

Yes — as many as you like, one entry per bounty at a time. Each bounty has its own daily submission limit, shown by `status --competition <bounty-slug>`.
</details>

## Mistakes that cost beginners their entry

- **Starting the session too early.** Your agent's session has to begin at or after the moment the bounty opens. A session that started even a little before cannot be entered, however good the entry turned out.
- **Submitting before running the local check.** A refused entry still uses up one of the day's submissions. The check on your own machine is free and catches most problems, so run it first, every time.
- **Pasting in code your agent never wrote.** The transcript checks compare the code against the sessions you upload. Pasted code counts against the share your agent authored, and an entry with too little of its own agent's work in it is refused.
- **Forgetting the license.** An entry without an OSI-approved license is refused. Adding MIT takes under a minute.
- **Leaving it to the last hour.** An exact tie goes to the earlier entry, and a refusal in the final hour leaves no time to fix it and resubmit.
- **Building for the prize instead of the scoring.** Read how the bounty is scored and build toward that. A polished entry the grader cannot measure loses to a plain one it can.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/1xds55qg · code <code>1xds55qg</code></sub>
