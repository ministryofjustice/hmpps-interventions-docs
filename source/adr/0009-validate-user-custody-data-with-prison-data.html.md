---
title: 9. Validating user-given custody data with prison data
weight: 79
last_reviewed_on: 2025-03-05
review_in: 1 year
---

# 9. Validating user-given custody data with prison data

Date: 2023-02-01


### Status

Accepted


### Context

Some of our referrals are for people currently in custody.

Our service providers must know where these users are and their expected release date.

User research found that the prison information using their typical sources (nDelius) is unreliable.
(Slides 70, 71, 72 of [research findings round 20](https://docs.google.com/presentation/d/12f6xI8ctv2ByjC9WC73ZXbGIPpfMPS8muHv_z3ddebE/edit?usp=sharing).)

The team designed a new input field in the referral process for storing prison locations as part of the referral.

This solution is an **anti-pattern** (duplicating information, re-keying, domain violation),
but pressures in and outside the team forced us to proceed.


### Decision

We agreed to run the input as an experiment for a limited time.

We will implement real-time data comparison with prison services to measure the following:

- Rate of agreement between the prison data and referral form input
- Rate of probation-to-prison lookup problems to gauge if we need to ask for prison-specific user identifiers
- Rate of unexpected errors

Once we find a significant (95%+?) portion of inputs match prison sources, we will remove the question from the referral form.


### Consequences

We will deliver the prison data collection as part of the first iteration. In this, during referral send, we will:

1. Lookup prison user identifiers from probation identifiers
2. Extract location and release date data from prison systems
3. Store and compare these results

We will review the collected data after 1000 referrals with custody information or three months after deployment,
whichever comes sooner.

#### References

- The input field went live on [22 Feb 2023 at 15:26](https://mojdt.slack.com/archives/CLA97UR7D/p1677079616289429).
- The data collection pull request is https://github.com/ministryofjustice/hmpps-interventions-service/pull/1468.
