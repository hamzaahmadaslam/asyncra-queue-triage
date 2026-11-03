# Status

Last updated 2026-11-02.

## The plugin

**Asyncra Queue Triage for Action Scheduler**, slug `asyncra-queue-triage`, version 1.0.0.
GitHub: https://github.com/hamzaahmadaslam/asyncra-queue-triage

A read-only screen under Tools > Queue Triage, two Site Health tests, a Site Health Info section and
`wp queue-triage report`. It groups the newest failed Action Scheduler actions by hook and normalised
error and labels each group "Fix first", "Retry candidate, check first" or "Review"; shows past-due
actions by hook, the runner, stuck actions and stale claims, table sizes, retention in effect and the
bundled copies; and writes a support report without arguments, ids, emails, IPs, queries or paths. It
writes nothing, stores nothing and makes no external requests.

### Why this name

Guideline 17 does not allow a slug that starts with another project's name, and Action Scheduler is an
Automattic project, so `action-scheduler-health` would be refused. "X for Action Scheduler" is the
pattern the guideline gives. "Queue Triage" was free on wordpress.org and matches the owner's
`action-scheduler-triage` command-line tool. See [research.md](research.md) for the competitors and the
rules that were checked.

Renamed 2026-10-31. The wordpress.org review of 2026-10-24 (submitted as "Queue Triage for Action Scheduler",
slug `queue-triage-for-action-scheduler`) asked for a distinctive identifier at the start of the name. The
plugin is now "Asyncra Queue Triage for Action Scheduler", slug `asyncra-queue-triage`; "Asyncra" had no
matches on wordpress.org and no WordPress or job-queue product on the web (only an unrelated Rust crate).
The menu label stays "Queue Triage". The renamed zip was uploaded 2026-11-01 with a note asking for the slug
`asyncra-queue-triage`; wordpress.org filed the note in the review ticket and the review now waits on the plugins
team. The assigned slug was still `queue-triage-for-action-scheduler` at that point.

Checked 2026-11-02 on the Add page: the assigned slug is now `asyncra-queue-triage`, and the review status is
"Being Reviewed: waiting on action from our plugins team" (326 plugins in the first-review queue). Nothing is
waiting on the owner. On the renamed tree (fefe4b5 code): `scripts/check.sh` passes (PHPCS clean, 81 unit tests),
the integration test passes 47 of 47 on WordPress 7.1.3 + WooCommerce and on WordPress 6.5.5 + Action Scheduler
3.9.2, and Plugin Check reports no errors or warnings. The old GitHub URL redirects to the new one.

2026-11-03, 9:34 PM PKT: the reviewer confirmed the slug change and opened an ownership check (review ID
`OWN asyncra-queue-triage/hamzaahmadaslam/24Oct26/T2 3Nov26`). A gmail.com account email is not accepted as
proof that the account owns the Author URI domain. Chosen route: a TXT record at the root of
hamzaahmadaslam.com (DNS on Cloudflare) with the value `wordpressorg-hamzaahmadaslam-verification`, then a
short reply in the same email thread once the record resolves publicly. **Waiting on the owner** to add the
record (the assistant's Cloudflare DNS change was blocked by its permission settings) and to approve the reply.
The 90-day clock for this request runs from 2026-11-03. Plugin files were rechecked the same day: name, slug
and text domain all match `asyncra-queue-triage`, no old slug left.

Separate thread, 2026-11-03: the team said to submit the CookieConsentHQ plugin only after this one is approved
and uploaded to SVN.

## Tests, 2026-10-20

| Check | Where | Result |
| --- | --- | --- |
| `php -l` on every PHP file | PHP 8.5 (WSL), CI on 7.4, 8.1, 8.4 | pass |
| PHPCS, WordPress Coding Standards 3 + PHPCompatibilityWP (7.4+) | WSL and CI | 0 errors, 0 warnings |
| PHPUnit (parsing, cleaning, templates, labels, checks, report) | WSL and CI on 7.4, 8.1, 8.4 | 81 tests, 211 assertions, pass |
| Integration (`tests/integration.sh`, 47 checks) | WordPress 7.1.3 + WooCommerce 11.2.1 (Action Scheduler 4.1.0), PHP 8.5, MariaDB 11.8 | 47 pass |
| Integration | same site plus the Action Scheduler 4.2.0 plugin | 47 pass |
| Integration | WordPress 6.5.5 + Action Scheduler 3.9.2 plugin, PHP 8.5 | 47 pass |
| Integration | CI: PHP 7.4 + WordPress 6.5.5 + Action Scheduler 3.9.2, and PHP 8.4 + WordPress 7.1.3 + WooCommerce | 47 pass on both, run 39346350519; the first PHP 7.4 run found a real bug (PHP 7 words a TypeError differently, so it was labelled invalid data), fixed in the next commit |
| Plugin Check 2.1.0 (`wp plugin check`, new-plugin mode, experimental and low-severity included) | WSL, built zip contents | no errors, no warnings |
| Plugin Check action | CI, built plugin | pass |
| PHP notices with WP_DEBUG | both local sites | none from the plugin |
| Browser | Chromium (Playwright) on the local site | screen renders, Copy report works, "View these actions" opens Scheduled Actions filtered to that hook, no console errors |

The integration test builds a backlog with Action Scheduler's own runner: 40 timeouts, 12 rate limits, 8
SMTP authentication failures, 5 payment timeouts, 20 TypeErrors, 9 invalid rows, 4 missing orders, 6
actions with no callback, a recurring action with an unknown error, 60 completed actions, 150 past-due
actions up to three days late, one action stuck in progress and one stale claim. It checks every group's
count, cause and label, that emails, URL queries, server paths and group names never reach the report,
that a subscriber is turned away, the Site Health results, that `CHECKSUM TABLE` on the four tables is
the same before and after reading, and the report with Action Scheduler deactivated.

The CI box (10.10.10.1) could not run the gate: it has no PHP (`php: command not found`, log
`/home/ci/ci/logs/queue-triage-for-action-scheduler/20261020-050736-bd7eef5.log`), and the `ci` user
cannot install packages. The same `scripts/check.sh` ran in WSL on the committed tree and passed before
each push, and GitHub Actions runs the full matrix.

## wordpress.org submission

Submitted 2026-10-20 from the owner's wordpress.org account (hamzaahmadaslam), with the zip from commit
a29bd36 (40,731 bytes, sha256 15b2b391605c12bec8005103b5bf50648efff2e27a7d0bdeca643f6e9df7342f).

- Assigned slug: `queue-triage-for-action-scheduler`.
- Automated plugin scan on upload: Pass.
- Review status: Awaiting Review. 460 plugins were waiting for a first review; the page said submissions
  received before 2026-10-15 had had their first reply.
- A short note for the reviewer went with it: read-only, bounded prepared SELECT queries on Action
  Scheduler's tables, no writes, no external requests, link to this repository.

## Pending after submission

- Review by the Plugins Team. Their email, subject "[WordPress Plugin Directory] Review in Progress: Queue
  Triage for Action Scheduler", goes to the address on the owner's wordpress.org account; add
  plugins@wordpress.org to the allowed senders. Reply to that email with any fixes, and upload fixed zips
  from the Add page; never submit again. Only one plugin may be under review at a time.
- If the reviewer finds the name too generic (the form warns that generic names are often refused), the
  slug can be changed once before review from the Add page, and the text domain must change with it.
- After approval, the SVN commit: copy the build into `trunk/` and `tags/1.0.0/`, and the files in
  `.wordpress-org/` (banner, icon, four screenshots) into `assets/`. Then the plugin page goes live.
- Set the GitHub repository's website to the wordpress.org page once it exists. The portfolio site's link
  to the plugin is owned by the portfolio controller session.
- Optional: install PHP on the CI box (`php-cli php-xml php-mbstring`, needs sudo) so `ci verify` can gate
  this repository.
