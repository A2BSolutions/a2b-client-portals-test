# A2B Client Portals - TEST ONLY

This repository hosts **TEST previews** of A2B client dashboards. Nothing in
it is production. Nothing in it ever will be.

## Hard rules

- **Never attach a custom domain to this repo's Vercel project.** It serves
  its own `*.vercel.app` address only. `clients.a2b-solutions.online` is
  attached to the production project (`a2b-client-portals`) and nowhere else.
- Every dashboard folder is named `test-<slug>` - the guard workflow fails
  any PR that puts content anywhere else.
- Every HTML page must carry `<meta name="a2b-test-build" content="true">`
  and a visible TEST / SIMULATED / NOT RATIFIED marker - the guard workflow
  enforces this too.
- Content arrives ONLY via `.github/scripts/preview-test-dashboard.sh` in
  `a2b-client-system`, which refuses packs that are not TEST-marked.
  Production packs go through `promote-dashboard.sh` into
  `a2b-client-portals` - that validator rejects TEST content, so the two
  populations can never mix.
- No real client data, no secrets, no internal files. The preview script
  scans for all three before anything is pushed.

## Lifecycle

- A TEST build lands as a PR from a `test-preview/<slug>-<commit>` branch.
- The Vercel bot comments the Preview URL on the PR - that URL is the
  deliverable. Merging to main is optional (it only updates this project's
  own vercel.app address) and is never required for a preview.
- When a test is finished: close the PR and delete the branch.
- The weekly cleanup workflow closes `test-preview/*` PRs untouched for 14
  days and deletes their branches.

## Relationship to production

| | TEST (this repo) | Production (`a2b-client-portals`) |
|---|---|---|
| Content | TEST-marked packs only | Approved client dashboards only |
| Gate | markers REQUIRED | markers REJECTED |
| Folders | `test-<slug>/` | `<slug>/` |
| Domain | none, ever | `clients.a2b-solutions.online` |
| Merge means | nothing real | production publish |
