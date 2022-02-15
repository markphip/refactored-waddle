---
title: Supported Policies
nav_order: 3
has_children: true
has_toc: false
permalink: /policies/index.html{% if site.url contains "ghe.com" %}
published: false{% endif }
---

# Supported Policies

The list of supported policies is constantly evolving so check back here for
more information. The supported policies are presented here in two ways.
Click on file names for more details about each configuration file.

## Policy File Structure

├ 📁 .github
├── 📁 policies  
└──── 🗎 [branch-protection.yml](branch-protection.md) (Branch Policies)  


### List of Supported Policies

| Policy Name | Description | Possible scenarios| Policy level |
|:------------|:------------|:------------------|:-------------|
| [Branch Protection](branch-protection.md) | Control required number of code reviewers, protect pushing to release branches etc. | Enforces good deployment/coding practices and ensures compliance checks are run on all code before it is merged | Organization, Repository |