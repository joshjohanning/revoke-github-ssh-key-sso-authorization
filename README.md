# revoke-github-ssh-key-sso-authorization

Revokes the SSO authorization for SSH keys against a GitHub organization. This can help enforce that the HTTPS git protocol be used instead of SSH.

> [!IMPORTANT]
> This action will only work in organizations that have configured SAML SSO.

## Sample Usage

```yml
name: ⛔️ Revoke SSH key authorization policy
on:
  schedule:
    - cron: 0,5 * * * * # runs every 5 mins
  workflow_dispatch:

jobs:
  revoke-ssh-keys:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/create-github-app-token@v1
      id: app-token
      with:
        app-id: ${{ vars.APP_ID }} # use an app with `organization: admin write` permissions
        private-key: ${{ secrets.PRIVATE_KEY }}
        owner: ${{ github.repository_owner }}
    - name: Check and Revoke SSH keys
      uses: joshjohanning/revoke-github-ssh-key-sso-authorization@v1
      with:
        organization: ${{ github.repository_owner }}
        github-token: ${{ steps.app-token.outputs.token }} # use a PAT with `admin:org` permissions or a GitHub app token with org admin write permissions
        dry-run: false # change to true to not revoke any SSH keys
        create-issue: false # change to true to create an issue and assign to the user whose SSH key is being revoked (can't be used with dry-run)
        issue-repo: ${{ github.repository }} # if create-issue is true, repo to create the issue in
```

## Sample Screenshots

Here's an example of what the SSH key looks like before and after it has been revoked.

### SSH key authorized to organization

![SSH key with SSO authorization to org](https://github.com/joshjohanning-org/revoke-github-ssh-key-sample/assets/19912012/893ec157-3792-47f6-9987-7bf11bc01dbb)

### SSH key with authorization revoked to organization

![SSH key with SSO authorization to an org removed](https://github.com/joshjohanning-org/revoke-github-ssh-key-sample/assets/19912012/36c585cd-2250-498b-8bac-552947e88acf)
