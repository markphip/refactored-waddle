---
layout: default
title: Home
nav_order: 1
permalink: /index.html
{% if site.url contains "ghe.com" %}
published: false{% endif }
---

# Policy Service Documentation

The GitHub Policy Service is created and maintained by the
[GitHub Inside Microsoft](https://aka.ms/gim) team at Microsoft 1ES.
This service enables the management of compliance and governance
policies on an Enterprise GitHub instance. In a nutshell, the Policy
Service allows you to manage your organizations and repositories
using a technique known as **_GitOps_**. See [What is GitOps?](policies/gitops.md)
for more information.

## Policy Configuration

The Policy Service is continually evolving to provide an ever
increasing set of policies that it can apply to your organization
and repositories. The list of available policies you can configure
is available here:

* [Supported Policies](policies/README.md)

