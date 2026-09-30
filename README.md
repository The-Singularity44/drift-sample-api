# drift-sample-api

A small sample API that shows [DRIFT](https://github.com/Yugvyas10/DRIFT1.2version), an API contract gate, blocking a breaking change in a pull request.

- `openapi.yaml`: the contract of a small Orders API.
- `traffic.jsonl`: a few recorded requests (made-up data), used by DRIFT as evidence.
- `.github/workflows/drift.yml`: runs the DRIFT GitHub Action on every pull request.

## What DRIFT does here

On every pull request, DRIFT compares `openapi.yaml` with the version on the base branch and labels each change:

- **BREAKING** only with failing evidence: a recorded or generated request that the old contract accepts and the new one rejects;
- **RISKY** when a change is dangerous but not proven;
- **SAFE** otherwise.

It writes the report to the job summary, posts one comment on the pull request (updated on every push), and fails the check when there is a BREAKING change.

## Try it

1. Open a pull request that removes `overnight` from `ShippingMethod` in `openapi.yaml`. Two recorded requests in `traffic.jsonl` use it, so DRIFT proves the change BREAKING and fails the check. Its comment shows the rule, the rationale and the failing request.
2. Open a pull request that only adds things, such as an optional field or a new endpoint. DRIFT labels the changes SAFE and the check passes.

To make a failing check block the merge, the default branch needs a branch protection rule with **Require status checks to pass before merging**, and the check **DRIFT contract gate** selected.
