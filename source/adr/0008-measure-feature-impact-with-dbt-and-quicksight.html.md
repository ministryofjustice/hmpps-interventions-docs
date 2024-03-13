---
title: 8. Measure feature impact with dbt and QuickSight
weight: 78
last_reviewed_on: 2025-03-05
review_in: 1 year
---

# 8. Measure feature impact with dbt and QuickSight

Date: 2022-08-15


### Status

Accepted


### Context

As a team, we found that measuring the impact of features was either not done or repetitive and tedious for developers.
The collected data required manual visualisation in Excel. We did not have baseline values if we collected metrics
programmatically for AppInsights.

We wanted a solution that:

- Could be extended with new metrics within the service team.
- Could be visualised in graphs and charts.
- Could automatically refresh with data up to last midnight.
- Could be used by future data analysts and modellers.

We engaged with a consultancy in June 2022 to explore our options in the Ministry of Justice.
We aimed to reuse as much as possible from existing platforms.

We also held workshops to identify how to change our planning process to define hypotheses of impact for features,
which we can measure.


### Decision

As a result of the consultancy's work and our workshops, we will:

- Discuss and define the expected impact on measurements as part of feature kick-offs.
- Use the Analytical Platform's [create-a-derived-table] tool for data modelling and creating data **marts**,
  representing domain metrics that are important to us.
- Use the Modernisation Platform to visualise the data marts in QuickSight.


### Consequences

We can create and track metrics based on our database with a short lead time.

We can track measurements before we change service behaviour impacting those measurements.

The trade-off is that we must get familiar with [dbt], data modelling, and maintain the [production data to QuickSight pipeline][bi-pipeline].


#### Work done so far

- [Documentation on accessing the Analytical Platform](../get-started/analytical-platform-access.html)
- [Documentation on accessing the Modernisation Platform](../get-started/BI-dashboard-access.html)
- [Technical documentation of the pipeline][bi-pipeline]
- Data mart: [sent referrals by contract type and intervention title](https://github.com/moj-analytical-services/create-a-derived-table/pull/127)
- Data mart: [time to first supplier assessment from referral sent](https://github.com/moj-analytical-services/create-a-derived-table/pull/132)


[create-a-derived-table]: https://github.com/moj-analytical-services/create-a-derived-table
[dbt]: https://docs.getdbt.com/guides/best-practices
[bi-pipeline]: ../integrations.html#data-transformation-pipeline
