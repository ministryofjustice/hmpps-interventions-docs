# 6. Process service provider reports asynchronously

Date: 2021-07-07

## Status

Proposed

## Context

Service providers need downloadable reports in CSV (comma seperated values) format, containing key performance metrics about their referrals.

The existing proposed design to allow them to get these reports is as follows:

1. The user enters a pair of dates in the Refer and Monitor (R&M) UI and clicks a button. The dates correspond to the time period for which the report should cover.
2. An HTTP GET request is sent to the Interventions API with the dates and the user's authentication token.
3. The Interventions service determines the contracts the user can access, and uses these parameters, along with the dates to run a query which populates the report for the entire date range.
4. The results are serialized to JSON and returned over the wire to the UI.
5. The UI converts the JSON response into CSV format and tells the users browser to download the data as a file.

There are several drawbacks with this approch:

- The user experience: the user has to wait an unknown amount of time for the request to complete. This duration is dependent on the date range, the number of contracts the user has access to, as well and the current load on the service (and probably a bunch of other factors too). Fundamentally, we have no idea how long it will take. To address this, the propsed UI solution sets a request timeout of 2 minutes for the request to complete. That's potentially 2 minutes of the user staring at a browser loading a page not knowing what's going on. If the request _does_ timeout after 2 minutes, that's 2 minutes wasted. The fact we set this value to 2 minutes in the first place means we really don't know how long it's going to take at all. See the code snippet below:

```
      // this will likely be a very large request, so we're overriding the timeout to play it safe
      timeout: {
        deadline: 120000,
        response: 120000,
      },
```

- The load on the database: the required query is large and very complex. We don't have a good handle on how it will perform. Whilst the query is read-only, many large queries at once could put strain on the database server. The main issue is we just don't know.

- The complexity of the query: the required query is complicated, and error prone. We are computing values and deriving states all in raw SQL. It's hard to read the code and understand what it's doing; that makes it hard to fix if there are bugs, and hard to extend.

- The fragility of the query: since the query is written in raw SQL, model changes or migrations can break it without warning. Integration tests can help us with this, but they don't cover every edge case. Furthermore, we have to manually deal with type casts from the query results, since the ORM (Hibernate) is not involved in mapping the query to the models. 

- The inability to unit test the report generation: since the whole report is generated from a single SQL query, we cannot test logic contained within it in isolation. As mentioned previously, integration tests provide a level of test coverage, but even then, due to the complexity of the query, decyphering what's wrong with it if the tests fail may not be easy, especially for those who are unfamiliar with the code.