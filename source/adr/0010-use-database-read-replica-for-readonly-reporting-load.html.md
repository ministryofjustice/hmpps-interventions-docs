---
title: 10. Validating user-given custody data with prison data
weight: 80
last_reviewed_on: 2025-03-05
review_in: 1 year
---

# 10. Using database read replica for readonly and reporting load

Date: 2023-11-20


### Status

Accepted


### Context

As the service has grown over the years and number of contracts increased so are the referrals.

The performance report is ran by the service providers on daily basis to pull yesterdays referrals and referrals from the inception of the service till date. 
This puts lot of load on the database and have noticed the database utilisation hitting 100%. This impacts availability of the service.




### Decision

To overcome this, we have increased the database instance size and at the same time we started to set up read-replica and potential to move the readonly / reporting load to read-replica.

Our reports use Spring Batch framework to pull the data. This posed additional challenge as Spring Batch has its own metadata tables which need to be moved to H2 in-memory database as read-replica does not allow any writes.

This solution was tested and have seen the load on primary DB gone down. 

### Consequences

We delivered the read-replica solution which will be used :

1. To run all the reporting load
   - Performance Report
   - NDMIS Report
   - Conclude Referrals
   - Transfer Women's Referrals
2. The next step would be to move the dashboard queries (PP and SP) to read-replica


We will review the DB utilisation and if the utlisation is consistently between 30% and 40%, we would explore the option to go back to lower DB instance class.

#### References

- This change went live on [30 Jan 2024] for performance report and then on [2 March 2024] for all other reports as part of spring boot upgrade .
