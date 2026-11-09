# GitHub → Slack → ChatGPT Work: multi-repository setup

## Scope
- Every Issue (opened, reopened, edited, new comment) and Pull Request (opened, reopened, edited, new commits/ready, reviews, new inline comments) in **each repository that installs this workflow**.
- The Hub and `video-ai` are the first two repositories. **New GitHub repositories do not automatically inherit the workflow** under a personal account; add a copy of the YAML and `SLACK_WEBHOOK_URL` as an Actions secret, or later deploy a GitHub App/organization-level listener with approved infrastructure.
- The forwarded Slack message contains only repository, event type, actor and link (no private source or comment body). Work should open GitHub using granted permissions.

## What Owner must enable
1. In Hub: the secret `SLACK_WEBHOOK_URL` already exists (verify the target Slack channel).
2. In `video-ai`: create `SLACK_WEBHOOK_URL` in repository Settings → Secrets and variables → Actions. You cannot read/copy its value back out of Hub; use the original Slack incoming webhook value.
3. In **ChatGPT Work**, create the Slack **new channel message** event trigger for the receiving Slack channel and grant access to both GitHub repos.
4. Configure Work to process dynamic GitHub event URLs, not to assume Issue #1. When posting on GitHub, put literal `[AI-WORK-REPLIED]` in the response to avoid retriggering; ignore already processed event IDs and prevent self-replies. Never automatically merge/edit main.
5. Owner review and merge both **Draft** PRs before GitHub's default-branch event hooks become active.

## Billing, behavior and caveats
- Public Hub standard GitHub-hosted Actions runners are free. Private `video-ai` uses included Action minutes; use GitHub **Billing → Budgets and alerts** with hard stop for extra charges where available.
- These YAML relays use no GitHub REST API calls and no AI API keys. Slack receiving the message is not proof the ChatGPT Work event fires. **Bot/webhook-origin Slack messages may not trigger the Work task**; verify a real Work run in the UI.
- Slack webhook secret is repository-specific. If missing, the workflow fails visibly instead of silently reporting success.
- The workflow ignores GitHub comments containing `[AI-WORK-REPLIED]` and service actors `dvfrancois2-bot`, `github-actions[bot]`, `dependabot[bot]` to avoid common loops. The receiving Work task must also deduplicate event IDs.
- GitHub Actions **does not replay missed events** when quota or an outage clears. A future backlog reconciliation mechanism would be necessary if zero-loss processing is a requirement.
- Jules is **not automatically woken** by GitHub/Slack without his own confirmed wake trigger.
- For future repositories: create a branch → copy `.github/workflows/notify-slack-collaboration.yml` → add `SLACK_WEBHOOK_URL` secret → obtain Owner PR approval → verify Work trigger.
