---
title: Branch Protection
parent: Policy Service
permalink: /policies/branch-protection.html
---
# Branch Protection Policy

The branch policy allows branch protection rules to be configured at the repository,
organization, or company level. Inherited rules or rules at the same level with the
same `branchNamePattern` will be merged together into the same rule. The GitHub settings
managed by these policies are [documented here](https://docs.github.com/en/github-ae@latest/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/about-protected-branches).

The policy configuration is stored at `.github/policies/branch-protection.yml`

## Accepted properties

| Key | Name in GH | Value | Notes |
|-----|------------|-------|-------|
| `branchNamePattern` | Branch name pattern | `string` (regex) | |
| `requiredApprovingReviewsCount` | Require pull request reviews before merging | `int` (1-6) | |
| `dismissStaleReviews` | Dismiss stale pull request approvals when new commits are pushed | `bool` | Requires `requiredApprovingReviewsCount` |
| `requireCodeOwnersReview` | Require review from Code Owners | `bool` | Requires `requiredApprovingReviewsCount` |
| `restrictsReviewDismissals` | Restrict who can dismiss pull request reviews | `bool` | Requires `requiredApprovingReviewsCount` |
| `whoCanDismissReviews` | People and teams that can dismiss reviews | `string[]` (username or team slug) | Requires `requiredApprovingReviewsCount` and `restrictsReviewDismissals`. The user / team must have at least write (push) permissions otherwise it will be omitted in the applied rule |
| `requiresStrictStatusChecks` | Require status checks to pass before merging | `bool` | Requires `requiredStatusChecks` |
| `requiredStatusChecks` | | `string[]` | Requires `requiresStrictStatusChecks`. Values can be any string, but if the value does not correspond to any existing status check, the status check will be stuck on pending for status since nothing exists to push an actual status |
| `requiresCommitSignatures` | Require signed commits | `bool` | |
| `requiresLinearHistory` | Require linear history | `bool` | |
| `isAdminEnforced` | Include administrators | `bool` | |
| `restrictsPushes` | Restrict who can push to matching branches | `bool` | Requires `whoCanPush` |
| `whoCanPush` | People, teams or apps with push access | `string[]` (username or team slug) | Requires `restrictsPushes`. The user / team must have at least write (push) permissions otherwise it will be omitted in the applied rule |
| `allowsForcePushes` | Allow force pushes | `bool` | |
| `allowsDeletions` | Allow deletions | `bool` | |

## Example

```yaml
id: id
name: branch_protection
description: Default branch protection policy
resource: repository
where: 
configuration:
- branchProtectionRules:
  - branchNamePattern: "main"
    requiredApprovingReviewsCount: 1
    dismissStaleReviews: true
    restrictsReviewDismissals: true
    whoCanDismissReviews:
      # A username
      - petyan
      # A team name
      - startcleandevs
    
    requiresStrictStatusChecks: true
    requiredStatusChecks:
      - 1ES/pull-request-quantifier
    restrictsPushes: true
    whoCanPush:
      # A username
      - petyan
      # A team name
      - startcleandevs
```
## Organization Level Policy Example

Policies can be set at the organization and/or repository level. By setting policies
at the organization level you can set the default policies you want in place for all
repositories but you can also enforce specific policies.

In the following example we are setting policies based on the name of the repository.
In this example, the required reviewers is set to 2 for the main and release
branches and we are also requiring that specific compliance checks have run. For
the release branches we are also restricting who can push to those branches.

We are also requiring signed commits and disabling forced pushes and branch deletion.

```yaml
id: id
name: branch_protection
description: Organization Branch protection policy
resource: repository
where: 
  - repository.name.startsWith("production-")
configuration:
  branchProtectionRules:
  - branchNamePattern: main
    requiredApprovingReviewsCount:
      min: 2
    requireCodeOwnersReview: true
    requiresStrictStatusChecks: true
    requiredStatusChecks:
      - 1ES/compliance-checks
    requiresCommitSignatures: true
    isAdminEnforced: true
    allowsForcePushes: false
    allowsDeletions: false
  - branchNamePattern: [Rr]elease/*
    requiredApprovingReviewsCount:
      min: 2
    requireCodeOwnersReview: false
    requiresStrictStatusChecks: true
    requiredStatusChecks:
      - 1ES/compliance-checks
    requiresCommitSignatures: true
    isAdminEnforced: true
    allowsForcePushes: false
    allowsDeletions: false
    restrictsPushes: true
    whoCanPush:
      - releaseteam
```
