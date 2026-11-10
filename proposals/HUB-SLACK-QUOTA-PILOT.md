# Pilot: GitHub Issue #1 → Slack → ChatGPT Work

**Status: ON MAIN (merged as 2b1ad80).** The workflow is live on `main` and handles the `issue_comment` event. This PR fixes two review findings on top of it: an optional @ChatGPT mention (via the `CHATGPT_SLACK_USER_ID` repository variable) and a clear failure when the webhook secret is missing (exit 1 instead of a false green).

## Why this design

- Hub is a **public** repository, where standard GitHub-hosted Actions runners are free according to GitHub's billing docs.
- Only `issue_comment.created` on **Hub Issue #1** triggers the relay. It forwards comments from `dvp-game1` (Muse), `gemini-dvp-f3` (Jules), and Owner `dvphuong20786`; it deliberately **ignores `dvfrancois2-bot`** (ChatGPT) to avoid ChatGPT → Slack → Work → GitHub loops.
- The workflow uses the **GitHub event payload** and makes **zero GitHub REST API calls**; it does not consume GitHub API request quota.
- Slack delivery is only a **notification**. Sending a Slack message does **NOT** prove a ChatGPT Work task will start; bot/webhook-origin Slack trigger support is **unverified**. Jules is not automatically awakened by this relay unless Jules independently configures a supported trigger.
- No model API keys or paid OpenAI/Claude/Gemini API calls are needed for the relay. Work plan limits remain independent.

## Setup checklist — Owner only

1. Create/connect a Slack workspace and a test channel, e.g. `#ai-collaboration-events`.
2. Configure a **Slack Incoming Webhook** for that channel.
3. In the GitHub Hub repo, go to **Settings → Secrets and variables → Actions → New repository secret** and create `SLACK_WEBHOOK_URL` with the webhook URL. Do not paste it in an Issue, PR, file or chat. Do not commit the secret.
4. Add `@ChatGPT` to the test Slack channel and, in ChatGPT **Work**, create a **new channel message** event task (where supported in the user's plan). Authorize GitHub read/write access as needed. The Work task should ignore duplicate comment IDs and refrain from replying to its own messages. Also set the repository variable `CHATGPT_SLACK_USER_ID` (Settings → Secrets and variables → Actions → Variables) to the ChatGPT app's Slack user ID, so the relay message @mentions it; until the variable is set, messages are sent without a mention.
5. **Before merging**, run a `workflow_dispatch` manual dry-run from the feature branch if permitted. Verify Slack gets the message. Observe separately whether Work actually starts; record task run ID, event time, response time.
6. Merge this PR **only after explicit Owner approval**. The automatic `issue_comment` trigger is already live on `main` (merged as 2b1ad80); merging this PR only updates the relay behavior.
7. Post a benign **Muse/Jules/Owner** comment on Hub Issue #1. Check Actions logs, Slack message, ChatGPT Work run, and any GitHub reply. Repeat three times before claiming the workflow succeeds.

## Free quota / stop-and-resume policy

**GitHub Actions on public Hub:** Standard GitHub-hosted runner minutes are free; the monthly 2,000-minute GitHub Free allowance is for **private** repositories and does not act as a timer that must be replenished for this workflow. This pilot caps each run at **2 minutes**, has no API reads, and cannot generate paid AI API usage.

**Private video-ai Actions (future scope, not deployed by this PR):**
- In GitHub personal account **Settings → Billing & Licensing → Budgets and alerts**, enable **included usage alerts** for 90%/100%.
- Create an **Actions product-level budget with $0 additional spending** if your GitHub UI supports it, and enable **Stop usage when budget limit is reached**. Confirm GitHub shows the hard-stop, not just notification. GitHub normally blocks hosted runners at the hard budget and permits new runs once the next billing cycle begins.
- **Important:** A stopped/failed/skipped event is not automatically replayed later. Resume of quota ≠ replay of every missed comment. A durable catch-up queue or periodic reconciliation would be needed to process events that arrived during a billing block. Do not promise lossless resume.
- Budgets protect billed usage but may not include charges incurred **before the budget was created** within the current period. Other private workflows can also consume the account allowance.

**GitHub REST API (for later agent code, not used here):**
- Read `x-ratelimit-remaining`, `x-ratelimit-reset` and `Retry-After`. On `403`/ `429` rate limit, pause until reset, with backoff for secondary limits. Do not use rapid retries. In Actions, the built-in `GITHUB_TOKEN` usually has 1,000 API requests/hour/repo, whereas authenticated personal requests typically have 5,000/hour.
- Exceeding GitHub API rate limits normally results in **throttling, not per-request charges**.

**Slack:** Incoming webhooks may be rate limited (~1 message/sec/channel). This workflow retries `429` briefly using `Retry-After`, then fails clearly. It does not queue indefinitely.

## Scope boundaries

- This is a **Hub Issue #1** MVP, not yet a listener for `video-ai` Issue #14 or PR review events.
- No tokens, credentials, main edits, PR merges, or workflow activation without Owner authorization.
- The target is a new **ChatGPT Work run**, not revival of an existing ChatGPT chat session.
- Use Work event task logs to establish whether a Slack webhook message triggers Work; if not, prefer a supported GitHub PR event trigger or another connector-supported trigger.

Docs:
- https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows
- https://docs.github.com/en/actions/concepts/billing-and-usage
- https://docs.github.com/en/billing/how-tos/set-up-budgets
- https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api
