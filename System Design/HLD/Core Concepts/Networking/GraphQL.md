---
tags: [system-design, hld, networking, api, graphql]
aliases: ["GraphQL API"]
---

# GraphQL: Flexible Data Fetching

GraphQL is a more recent API paradigm (open-sourced circa 2015 by Facebook) that allows clients to request exactly the data they need.

Here's the problem GraphQL solves:
1) Frequently teams and systems are organized into frontend and backend. As an example, the frontend might be a mobile app and the backend a database-based API.
2) When the frontend team wants to display a new page, they can either:
	- cobble together a bunch of different requests to backend endpoints (imagine querying 1 API for a list of users and making 10 API calls to get their details)
	- create huge aggregation APIs which are hard to maintain and slow to change
	- write brand new APIs for every new page they want to display. _None of these are particularly good solutions_ but it's easy to run into them with a standard REST API.

The problem with under-fetching is that you may need multiple requests and round trips. This adds overhead and latency to the page load.

![[Pasted image 20260418202208.png]]

**Over-fetching** is the opposite: when we pack way more than we need in an API response to guard ourselves against future use-cases that we don't have today. It means that APIs take a long time to load and return too much data.

![[Pasted image 20260418202237.png]]

And writing brand new APIs for every new page is a nightmare.

- GraphQL solves these problems by allowing the frontend team to flexibly query the backend for exactly the data they need.
- The backend can then respond with the data in the shape that the frontend needs it.
- This is a great fit for mobile apps and other use-cases where you want to reduce the amount of data transferred.

Here's an example of a GraphQL query which fetches just the data the frontend needs for a sophisticated page which shows both users with their profiles and groups they're a member of.

```
query GetUsersWithProfilesAndGroups($limit: Int = 10, $offset: Int = 0) {
  users(limit: $limit, offset: $offset) {
    id
    username
    //...
    
    profile {
      id
      fullName
      avatar
      // ...
    }
    
    groups {
      id
      name
      description
      // ...
      
      category {
        id
        name
        icon
      }
    }
    
    status {
      isActive
      lastActiveAt
    }
  }
  
  _metadata {
    totalCount
    hasNextPage
  }
}
```

The graphQL code here is basically specifying which fields and nested objects we want to fetch. The backend can interpret this query and respond with just the data the frontend needs.

In our example, instead of writing a bunch of different APIs, the frontend team can just write a single query to get the data they need and the backend can (in theory) respond with the data in the shape that the frontend needs it.

#### Where to Use It

1) GraphQL is a great fit for use-cases where the frontend team needs to iterate quickly and adjust.
2) They can flexibly query the backend for exactly the data they need.
3) On the other hand, execution of these GraphQL queries can be a source of latency and complexity for the backend — sometimes involving the same bespoke backend code that we're trying to avoid.
4) In practice, GraphQL finds its sweet spot with complex clients and when multiple teams are making wide queries to overlapping data.
5) For system design interviews specifically, the benefits of GraphQL are murky.
6) In the interview you'll have a fixed set of requirements (not the moving targets of iterating on a mobile app or web frontend where GraphQL starts to shine). Additionally, the interviewer will frequently want to see how you optimize specific query patterns and while you can talk about custom resolvers — GraphQL is frequently just in the way.
7) It is recommend bringing up GraphQL in cases where the problem is clearly focused on flexibility (e.g. the interviewer tells us we need to be able to adapt our apps quickly to changing requirements) or when the requirements in the interview are deliberately uncertain.

## Related

- [[> Networking Essentials]] — back to the networking MOC
- [[REST]] — simpler alternative for most interview scenarios
- [[gRPC]] — efficient binary alternative for internal services
- [[HTTP and HTTPS]] — GraphQL runs over HTTP
