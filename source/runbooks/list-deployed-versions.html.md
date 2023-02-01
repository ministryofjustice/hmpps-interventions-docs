---
title: List deployed versions
weight: 56
last_reviewed_on: 2023-02-01
review_in: 1 year
---

# List deployed versions

### Problem

We do not have continuous deployment to production.

Therefore, we need a convenient way to tell us what changes are not yet live.

### Actions

1. Clone the [the operational scripts][ops-repo] repository.

1. Ensure you have a working [Cloud Platform setup](../get-started.html).

1. Use `./versions.sh`

   Example output: ![Example version.sh output](../images/ops-version-sample.png)


[ops-repo]: https://github.com/ministryofjustice/hmpps-interventions-ops
