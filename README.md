# enforce-github-pat-expiration

Revokes the SSO authorization for classic Personal Access Tokens (PATs) that don't meet the maximum expiration policy defined in the action (i.e.: those over 90 days or those with no expiration).

> **Note**
> This action will only work in organizations that have configured SAML SSO.

> **Note**
> This action only works with classic PATs.

## Sample Usage

```yml
name: 🔎 Enforce PAT expiration policy
on:
  schedule:
    - cron: 0,30 * * * * # runs every 30 mins
  workflow_dispatch:

jobs:
  check-pats:
    runs-on: ubuntu-latest
    steps:
    - name: Check and Revoke PATs
      uses: joshjohanning/enforce-github-pat-expiration@v1
      with:
        organization: ${{ github.repository_owner }}
        policy-in-days: 90
        github-token: ${{ secrets.ORG_ADMIN_TOKEN }}
        dry-run: false # change to true to not revoke any PATs
        create-issue: false # change to true to create an issue and assign to the user whose PAT is being revoked (can't be used with dry-run)
        issue-repo: ${{ github.repository }} # if create-issue is true, repo to create the issue in
```
