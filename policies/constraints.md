---
title: Constraints
parent: Policy Service
permalink: /policies/constraints.html
---

# Constraints

Constraints is a mechanism in policy configuration allowing:
* to limit possible values of configuration properties
* to manage [inheritance & overriding logic](inheritance.md)

A primitive property can have a constant as its value or a constraint.
```yaml
days: 365   # constant value
```

```yaml
days:
  min: 365  # constraint instead of constant value
```

Multiple constraints could be specified for the same property:
```yaml
days:
  min: 365
  max: 700
  default: 400
```


### default

Suggests a default value, which can be overriden.
```yaml
days:
  default: 400
```
Possible value: any primitive type (numeric, string, boolean).

### min

Restricts minimum possible value.
```yaml
days:
  min: 365  # days should be set to 365 or greater
```
Possible value: any numeric.

### max

Restricts maximum possible value.
```yaml
days:
  max: 400  # days should be set to 400 or less
```
Possible value: any numeric.


### allOf

Requires to have at least the specified items.
```yaml
branches:
  allOf: ["main", "dev", "prod"]
```

Or (same with another syntax):
```yaml
branches:
  allOf:
    - main
    - dev
    - prod
```

Possible value: array of primitives (numeric, string, boolean).


## Open questions
1. Should we prefix the constraint keywords with `$` (e.g. `$min`, `$max`, `$default`)?
The problem is for properties, which have nested fields naturally, would having of `min` or `max` create a paring problem?
```yaml
numeric-property:
  min: 10
  max: 20

object-propety:
  min: 777      # here min is a property name, not a constraint
  allow: true
```

It shouldn't be a problem if parser knows if this is a promitive property or a complex object. But would it know?
