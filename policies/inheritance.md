---
title: Inheritance and Overriding
parent: Policy Service
permalink: /policies/inheritance.html
---

# Configuration Inheritance and Overriding

Different policy configuration can be applied to the same resource if there are multiple policy YAML files in the same repo or on different hierarchy levels (e.g. organization and repo level). [Constraints](constraints.md) is the mechanism to solve potential conficts in the configuration.

This section describes the config resolution logic.

### Same hierarchy levels
E.g. several YAML files in the same repo for the same policy type.

| Config 1             | Config 2             | Result   | Comments |
|----------------------|----------------------|----------|----------|
| min: 42              |                      | 42       | min range value |
| max: 50              |                      | 50       | max range value (since min is not specified) |
| min: 42 <br> max: 50 |                      | 42       | min range value |
|                      |                      |          | |
| min: 42              | min: 45              | 45       | min value of the range intersection |
| max: 50              | max: 60              | 50       | max value of the range intersection (since min is not specified) |
| min: 42 <br> max: 50 | min: 45 <br> max: 60 | 45       | min value of the range intersection |
|                      |                      |          | |
| default: 42          |                      | 42       | |
| default: 42          | default: 43          | 42 or 43 | undetermined, depends on what rule will be applied first |
|                      |                      |          | |
| 42                   | 43                   | 42 or 43 | undetermined, depends on what rule will be applied first |


### Different hierarchy levels
E.g. organization-level (higher level) policy and repo-level (lower level) policy.

| Higher level         | Lower level          | Result   | Comments | 
|----------------------|----------------------|----------|----------|
| min: 42              |                      | 42       | |
| min: 42              | 43                   | 43       | 43 is in the allowed range |
| min: 42              | 40                   | 42       | min value in the allowed range |
| min: 42 <br> max: 50 | 43                   | 43       | 43 is in the allowed range |
| min: 42 <br> max: 50 | min: 45              | 45       | min in the allowed intersection |
| min: 42 <br> max: 50 | min: 60 <br> max: 70 | 42       | ignoring constraints from the lower level |
|                      |                      |          | |
| default: 42          |                      | 42       | |
| default: 42          | 43                   | 43       | default allows overriding |
| default: 42          | default: 43          | 43       | default allows overriding |


## Open Questions
1. Anything better than "undetermined" above?
