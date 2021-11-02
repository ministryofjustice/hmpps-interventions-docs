# 6. Treat Delius NSIs as collectors, not owners of interventions

Date: 2021-05-27

Written: 2021-08-04

## Status

Approved

## Context

To facilitate

- [breach proceedings][breach] when someone misses an intervention appointment,
- [RAR (rehabilitation activity requirement)][rar] day calculations when someone attends an intervention appointment,
- booking intervention appointments,
- and notifying probation officers about what happened,

we currently use a data structure called NSI (non-statutory intervention, but the name is diluted now) in Delius,
the monolithic probation system.

In essence, NSIs are "probation activities": they have a start, an end, an outcome.

<p align="center">፨</p>

Delius owns the person-on-probation appointments; but they are made _in context of_ an intervention.

Currently, this modelled through linking appointments and notifications to NSIs:

![Delius relationships](./0006-delius-nsi-dependencies.svg)

❗️ **Problem**: if we were to store the link between interventions and Delius NSIs outside Delius,
it would look as if Delius _created_ both appointments and referrals. We want Delius to
- be _aware_ of referrals
- be _notified_ of updates in context of referrals
- and _own_ appointments in context of referrals

<p align="center">፨</p>

**To make ownership of referrals explicit**, we have to decide which semantic is more applicable:

1. we _request_ an activity from Delius we could use as a vessel for intervention referrals
    - this makes Delius an **owner**: new updates are in context of the NSI
    - this is applicable when Delius owns all business rules around who and when can create activities
2. we _tell_ Delius that we have created a new external activity (e.g. intervention referral)
   and ask it to be aware of it
    - this makes Delius a **collector**: new updates are in context of the external activity
    - this is applicable when the external service owns all business rules around who and when can create
      those activitites

## Decision

We choose option 2:

As we can only validate business rules around intervention referrals in the interventions service,
we treat Delius NSIs as the **collector** of intervention activities, not the owner.

👉 Interventions does **not** need to store `nsi_id` to work with Delius. Further communications can look up
the relevant NSI based on the referral's UUID.

👉 We will store the UUID of our intervention referral directly on the NSI
(currently, only possible in the notes field). (_Edit 2 Nov 2021_: This field is now called `external_reference` in Delius)

👉 We will use [URNs][urn] to do so, in the form of `urn:hmpps:interventions-referral:{intervention-uuid}`,
for example `urn:hmpps:interventions-referral:3355304d-10d5-4e0e-9212-323fb2de2d5d`.

## Consequences

❗️ **Consistency problem**: This decision semantically ties the NSI to be a consequence of the referral.
Unfortunately, the NSI cannot be made read-only. If the NSI gets closed before the intervention referral is "done",
further updates get rejected. (There are business processes where people manually close NSIs,
for example, when someone's sentence gets terminated.)

When someone creates an activity (NSI), the NSI is responsible for storing

- the context: UUID or ID of the external activity
- the what: the type of the external activity (in our case, intervention)

Both are possible in a single field via [URNs][urn].

👉 NSI activities that contain such context shift semantically from "source of truth" to "container";
their _data fields_ have no guarantees to be correct/maintained. This is intended. Up-to-date data must be acquired
from the service which created the activity.

👉 Reporting must join intervention referral data on NSIs using this new URN.

🙋 Encoding origin IDs as URNs could allow us to drop specific NSI types. For example,
`urn:hmpps:interventions-referral:3355304d-10d5-4e0e-9212-323fb2de2d5d` encodes the origin too,
which then can defer the type to the origin service

[breach]: https://www.gov.uk/guide-to-probation/if-you-break-the-rules-of-your-probation
[rar]: https://www.gov.uk/government/publications/the-rehabilitation-activity-requirement-in-probation/rar-guidance
[urn]: https://en.wikipedia.org/wiki/Uniform_Resource_Name#Namespaces
