---
title: BI dashboard access
weight: 22
last_reviewed_on: 2023-01-17
review_in: 6 months
---

# Business intelligence dashboard access

Our data is extracted daily into the Analytical Platform, transformed there and exposed to the Modernisation Platform.

The Modernisation Platform exposes these data points with [QuickSight](https://aws.amazon.com/quicksight/).

(Please refer to [the Integrations page](../integrations.html) for _how_ this process works.)

### Access checklist

1. Ensure you have a GitHub account [with **digital.justice.gov.uk** email and multi-factor authentication](https://user-guidance.services.alpha.mojanalytics.xyz/get-started.html#1-github-account).
1. Join the [**ministryofjustice** organisation](https://github.com/orgs/ministryofjustice/sso).
1. Request to join the [hmpps-interventions-dashboard-access team](https://github.com/orgs/ministryofjustice/teams/hmpps-interventions-dashboard-access/members)
   with the "Request to join" button in the top right corner.


### Data access

1. Visit [https://moj.awsapps.com/start](https://moj.awsapps.com/start)
1. Log in with GitHub.
1. Open the "Management console" button on the `refer-monitor-development` namespace with `modernisation-platform-sandbox` credentials:
   ![Management console button](images/modplatform-console.png)
1. Once in, search for "quicksight":
   ![Search for QuickSight](images/modplatform-search-quicksight.png)
1. After opening it, you should see a page with "Analyses" selected that might look like this:
   ![QuickSight start page](images/modplatform-quicksight-landing-page.png)
1. Open one of the "Analyses", and you should see something similar:
   ![Sample analysis](modplatform-quicksight-sample-analysis.png)

If you have seen all of the above, you have access!


### Changing analyses and dashboards

Most things are auto-saved in QuickSight.

**❗️ Do not change shared analyses and dashboards for experiments.**

If you want to experiment with something, first **make a copy** with the Save icon:
![Copy an analysis](modplatform-quicksight-analysis-saveas.png)
