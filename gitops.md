---
layout: default
title: GitOps
nav_order: 2
permalink: /gitops.html
---
# GitOps

## What is GitOps?

GitOps is a means of operating a service where by the "source of
truth" is stored in a Git repository. A wonderful side effect of
this approach is that you get all of the auditing and history of
configuration changes for "free" by examining the history of the
configuration files in your Git repository. This is also known as
**_Configuration as Code_**, but GitOps takes it a step further
by continually monitoring the state of the system and the state
of the repository and keeping the two systems in sync with each other.

In the case of the Policy Service, configuration (or policies) for
your organization are stored as `.yml` files inside a special
repository named `.github` inside your organization. In the case
of your repositories, the `.yml` files are stored inside the
`.github` folder of the default branch for your repository -- typically
**main** or **master**.

The Policy Service is always running and it is integrated with GHAE
to receive webhook events for everything that happens within your
organization. When a new or changed policy is merged to the default
branch the Policy Service applies the configuration settings to
your GitHub repositories. Likewise, if someone uses the GitHub UI
or API to change a setting, the Policy Service verifies the change
is in accordance with the policy configuration and can return the
setting to the configured value. Of course if you create new
repositories in your organization the new repository will automatically
be configured according to the organization policies.

## Creating/Modifying Policies

When you want to create or make changes to policies, you simply edit
the necessary `.yml` file(s) that contain the policies. Normal access
control rules determine who is able to change these files and the
process for changing them is the same as any other code in the repository.
Typically, you would edit the file(s) and submit a Pull Request so that
the changes can be reviewed by other team members. A nice feature to
this approach is that the Policy Service integrates with the GitHub
Pull Requests and automatically runs checks on the policy files for you
The Policy Service will catch and report if your YAML file is invalid and
it will even evaluate the policies themselves. For example, suppose that
the policy at the organization level is set to require a minimum of 2
reviewers on a Pull Request. If you submit a change to the policy for
the repository to lower this to just 1 reviewer, the Policy Service
will report this problem to you on the Pull Request so that you know
that this change is not going to take effect.

## Applying Policy Changes

Policy changes are applied automatically when changes to the policy
files are pushed to the default branch regardless of whether it is
done via direct push or by merging a pull request. In addition, since
the Policy Service is always running and reconciling the current state
of the system with the policies, it is also applying policies automatically
whenever needed. For example, if the policy says there are 2 required
reviewers and someone with the right permissions modifies this setting
using the GitHub UI or API, the policy service will simply reapply the
correct settings immediately.

Likewise, if a repository policy is configured to require 1 reviewer and
someone later changes the organization policy to require 2 reviwers, the
Policy Service reconciles these policies and applies the correct value of
2 reviewers to the Repository settings. **NOTE:** the Policy Service does
NOT attempt to edit and fix the policy file in the repository that says
only 1 reviewer is required. This can admittedly be confusing to someone
that is only looking at the policy in the repository level. One way the
user could figure this out would be to submit a new Pull Request to
the repository that modifies something in the policy file. By doing this
the Policy Service checks that run on the Pull Request would run and be
able to report to the user that the repository policy is out of compliance
with the organization level policy.

## Further Reading

An Internet search for "GitOps" or "Configuration as Code" will reveal
lots of articles on the topic. We make the following distinction between
the two terms:

**Configuration as Code**: this is the more generic term and essentially
applies to any system where the rules are written as source code and then
"applied" to the system. A tool like Terraform is a great example, but there
are others like Ansible and Chef. These tools sometimes are also referred
to as GitOps but we use a narrower definition for that term.

**GitOps**: this is a subset of Configuration as Code where the source
code and the running system are constantly being reconciled and kept
in synch. Unlike Configuration as Code where changes are usually applied
as part of an automated CI/CD pipeline, with GitOps there is a service
that is running that keeps the systems in agreement at all times. Aside
from our GitHub Policy Service the best example of this tool is FluxCD
which is a GitOps tool for managing Kubernetes clusters.

[This website](https://www.gitops.tech/) uses the terms "Push-based" and
"Pull-based" to distinguish these two methods of GitOps. Using their
terminology, the GitOps Policy Service is "pull-based".
