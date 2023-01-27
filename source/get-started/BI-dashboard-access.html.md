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

1. Ensure you have a [GitHub account](https://github.com/join?plan=free).
1. [Add your **digital.justice.gov.uk** email address to your account](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/adding-an-email-address-to-your-github-account).
1. [Configure two-factor authentication](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication).
1. Join the [**ministryofjustice** organisation](https://github.com/orgs/ministryofjustice/sso).
1. Request to join the [hmpps-interventions-dashboard-access team](https://github.com/orgs/ministryofjustice/teams/hmpps-interventions-dashboard-access/members)
   with the "Request to join" button in the top right corner.
1. Ask the team in the [#interventions-dev] Slack channel to approve the team join request.

After joining the GitHub team, it may take up to **six hours** for access to propagate to the Modernisation Platform.

If you need immediate access, please [#ask-operations-engineering] to manually run the SSO Sync job, referring to this guide
and [this definition](https://github.com/ministryofjustice/modernisation-platform/blob/4b7becb4a7162fd59039b9e1c1d65b9d2d1e79e8/environments/refer-monitor.json#L12).


### Data access

1. Visit [https://moj.awsapps.com/start](https://moj.awsapps.com/start)
1. Log in with GitHub.
1. Open the "Management console" button on the `refer-monitor-development` namespace with `modernisation-platform-sandbox` credentials:
   ![Management console button](../images/modplatform-console.png)
1. Check you see the "AWS console" main page:
   ![AWS console main page](../images/modplatform-console-main-page.png)
1. Add yourself to the _refer-monitor-team_ group [here](https://quicksight.aws.amazon.com/sn/console/groups):
   ![Add yourself to the group](../images/modplatform-quicksight-group-add.png)
1. Verify access by opening [the shared folders](https://quicksight.aws.amazon.com/sn/folders/public). You must see at least one:
   ![Shared folders](../images/modplatform-quicksight-shared-folders.png)


### Testing access

1. After adding yourself, go to [QuickSight analyses](https://quicksight.aws.amazon.com/sn/start/analyses). You should see a page like this:
   ![QuickSight start page](../images/modplatform-quicksight-landing-page.png)
1. Open one of the "Analyses", and you should see something similar:
   ![Sample analysis](../images/modplatform-quicksight-sample-analysis.png)

If you have seen all of the above, you have access!


### Changing analyses and dashboards

Most things are auto-saved in QuickSight.

**❗️ Do not change shared analyses and dashboards for experiments.**

If you want to experiment with something, first **make a copy** with the Save icon:
![Copy an analysis](../images/modplatform-quicksight-analysis-saveas.png)


[#interventions-dev]: https://mojdt.slack.com/archives/C01DYKJUKDX
[#ask-operations-engineering]: https://mojdt.slack.com/archives/C01BUKJSZD4
