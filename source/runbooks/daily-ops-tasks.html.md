---
title: Daily ops tasks
weight: 50.1
last_reviewed_on: 2023-01-12
review_in: 6 months
---

# Daily ops tasks

This page explains how a day on the ops rotation works. In this role, our responsibility is to:

| Responsibility | Why |
| --- | --- |
| Keep the service's integrity | Data cannot be corrupted. ("Service" is the entire "Refer and monitor an intervention", not just the API.) |
| Secure the service | Security issues must be fixed immediately. |
| Keep the service performant | Tolerances (CPU/memory/database storage/query performance) must be monitored. It cannot run out of pod limits, storage space or network bandwith. |
| Reduce alert noise | Only actionable, real problems should create alerts. Everything else should be a dashboard. |


### Checklist

1. Check [Grafana][grafana] for any irregularities.

1. Check [#interventions-alerts] for any AlertManager (Kubernetes) errors.

1. Check [#interventions-alerts] for any [exceptions](../monitoring.html#exceptions).

1. Check `kubectl get pods --namespace=hmpps-interventions-prod` for any errors, backoffs, or excessive restarts.

1. Check the [logs](../monitoring.html#logs) for any errors or problems.

1. 🙋 Extend this documentation with more specific tips.


### When we find anomalies

1. Look into why they happened. Use [five whys][five-whys].

1. Communicate all along the way in [#interventions-dev]: when you start looking, what you are looking into.

1. Resolve the issue with one of the below outcomes. Please update the thread with your conclusion so future people
   can find it.

    | Decision | Action |
    | -- | -- |
    | Not enough information | Consider what extra information we need to collect.<br>Write new tickets if needed. Prioritise them if urgent. |
    | Intermittent issue | Nothing to worry about right now. Escalate if occurs more times. |
    | Serious issue | Establish how users are impacted and how many of them.<br>If this is an outage, announce an incident (see below). |
    | Noise | Not an issue, noise. Eliminate noise by improving the code and integrations. Ensure the problem is present on a dashboard. Write tickets if necessary. |
    | Undetermined | Collaborate with others to determine an outcome. Extend this guide if necessary. |


### When we have a user-impacting serious incident

1. Announce that we are looking into it in [#interventions] with this template:

    > **📟 Degraded service**
    >
    > **From**: {the time it started}
    >
    > **User impact**: {explain the user impact in terms of what they cannot do and how many of them}
    >
    > **What**: {explain what is happening}
    >
    > Investigation in progress.

1. Raise an incident on the [status page](https://status-refer-monitor-intervention.apps.live.cloud-platform.service.justice.gov.uk/status/refer-and-monitor)
   by "Edit status page" and "Create incident".

1. Proceed to solve it and please keep the thread and incident updated with the progress.

    When finished, please add the following lines to the original message:

    > ✅ **Resolved**
    >
    > **Until**: {the time it resolved}


[#interventions-alerts]: https://mojdt.slack.com/archives/C022FKANS87
[#interventions-dev]: https://mojdt.slack.com/archives/C01DYKJUKDX
[#interventions]: https://mojdt.slack.com/archives/CLA97UR7D
[grafana]: https://grafana.live.cloud-platform.service.justice.gov.uk/d/PyQ91ARnk/hmpps-refer-and-monitor-an-intervention?orgId=1&from=now-3h&to=now
[kibana-pod-logs]: https://kibana.cloud-platform.service.justice.gov.uk/_plugin/kibana/app/discover#/view/70ddf910-97df-11ed-ae16-ef32c250fb53?_g=(filters%3A!()%2CrefreshInterval%3A(pause%3A!t%2Cvalue%3A0)%2Ctime%3A(from%3Anow-0d%2Fd%2Cto%3Anow))
[kibana-ingress-logs]: https://kibana.cloud-platform.service.justice.gov.uk/_plugin/kibana/app/discover#/view/dc3efc90-97df-11ed-81cd-e3a96a1418c1?_g=(filters%3A!()%2CrefreshInterval%3A(pause%3A!t%2Cvalue%3A0)%2Ctime%3A(from%3Anow-0d%2Fd%2Cto%3Anow))
[five-whys]: https://en.wikipedia.org/wiki/Five_whys
