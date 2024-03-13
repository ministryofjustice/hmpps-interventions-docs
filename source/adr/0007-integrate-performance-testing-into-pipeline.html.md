---
title: 7. Integrate performance testing into the continuous integration pipeline
weight: 77
last_reviewed_on: 2025-03-05
review_in: 1 year
---

# 7. Integrate performance testing into the continuous integration pipeline

Date: 2022-08-31


### Status

Proposed


### Context

We released memory-intensive code to production on July 2022, which caused a long outage.

In the early days, before June 2021, the service discarded performance testing based on the anticipated
number of referrals per person was not expected to be above a couple of dozen referrals.

Later, performance problems arose from unbounded pagination. In June 2022, we rolled out pagination to mitigate
and introduced monitoring for slow responses.

The above led to a gap: we could detect slowing down over time, but **we cannot detect slowing down caused by
changes to the service**.


### Decision

1. Introduce read-only<sup>6</sup> performance testing into the continuous integration pipeline:

   - Automatically run before **all** production deployments.
   - **Block** production deployments if performance tests fail.
   - The team agrees to **focus on maintaining** the performance tests so they do not fail outside valid reasons.
      - Flaky tests, environmental issues, data drift, etc. will be prioritised immediately.
      (Otherwise, the test suite becomes a burden, not help.)
   - To let critical fixes reach production, we will also provide an **emergency bypass** to allow production
   deployments to proceed even if the performance test fails.
      - Can only be used for user-impacting critical fixes. In all other cases, the flaky tests must be fixed first.

2. Extend checklists:

   - Add a refinement checklist about "does this need a performance test".
   - Add an incident checklist about "would a performance test help detect this problem".


#### When will we know this succeeded?

- When we revert the fix to the performance problems and see consistent failures on the performance tests.
- When we re-apply the fix, we see passing tests.

#### What does this not solve?

Will not guard against:

- Running out of capacity on concurrent requests.
- Deadlocks and query locking performance issues, caused by migrations or otherwise.
- Effects of anomalous peaks of usage.
- Dependency failures, timeouts, server errors.


### Consequences

#### 1. Methodology

As of today, we get about around 23-25 requests per second, assets and operational endpoints excluded.

The (Gatling) tooling can define that we want N number of journeys to complete per second.

We need to define our success in aggregate. For example, "98% of dashboard calls to this user are under 2 seconds".

This is a predictable, synthetic load, which aligns with our current goal: realistic load testing or
duplicating traffic from production is not in scope.

|ℹ️ Consider synthetic load tests with aggregate success conditions.
|-

#### 2. Tool

Pre-production network access will be limited. Any testing tool we use needs to be able to be deployed on the
target environment.

|ℹ️ Consider using a tool that can be configured with code and deployed with a Docker image.
|-

[Gatling](https://gatling.io/) looks to be a likely candidate. It is already used in HM Prison and Probation Service
for _nDelius_ performance testing.

#### 3. Monitoring

In case of genuine performance problems, we anticipate infrastructure and code alerts firing. They currently go to
the [`#interventions-dev-notifications`](https://mojdt.slack.com/archives/C01F047EYA2) channel.

|ℹ️ Consider how we can identify alerts caused by performance tests and how to identify real alerts.
|-

#### 4. Dependencies and data

We do not aim to performance test external dependencies.
The external data we use in these tests is also a dependency.

Currently, it's prohibitively expensive to set up a completely isolated environment.

Going around this problem is either with mocking:

- Mocking to avoid real dependencies is
   - hard to do with realistic data;
   - likely has a high maintenance burden as it reflects work done by other teams.
- Mocking `hmpps-auth` on an environment with live data creates a security backdoor.

|ℹ️ Consider using real dependencies for now. Re-evaluate if they become a problem.
|-

Or data generation:

- As of today, generating data on demand for testing is **not** built into any of the services.
- We cannot generate data for our dependencies: risk data, CRNs, etc.
- Building a data generation toolset is going to be immensely useful.
  External interfaces (internal API, events) would allow other services to request test data.

We need short-term wins. Since we have all referrals copied over to pre-prod, we can pick the user that has
the largest number of referrals and test their happy path still works.

|ℹ️ Consider testing as a user with a huge referral pool. Develop data generation as a next step.
|-

#### 5. Local development

Allow developing and trying the test suite without going needing a commit on mainline and full deployment.

|ℹ️ Consider one-line tooling to start the test suite against a target environment with local test code.
|-

#### 6. Concurrency and resetting the environment state

Our continuous integration tool (CircleCI) allows multiple concurrent jobs to run.
Our dependencies can be busy with their tests.

Tests should be as isolated as possible so that other tests do not change behaviour depending on
the sequence or concurrency of each other.

|ℹ️ Consider writing only reader tests for now, to defer decision on how to deal with concurrency.
|-

#### 7. Team time

Maintaining such tests will take up team time.

|ℹ️ Consider documenting how and why we are doing this so the whole team is aware.
|-
