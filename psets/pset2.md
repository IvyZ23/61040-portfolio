# Problem Set 2

## Concept Questions

1. There are many contexts that need to have unique strings, but these contexts do not overlap (i.e. if a string is in one context but not the other, the other context can still use that string). In the URL shortening app, there can be a context for each URLBase. Each context would store the suffix strings that each URL base has.
2. NonceGeneration stores used strings so that it won't generate the same string for the same URL base later on. If a counter was used, the counter can represent the last used string in a set list of strings.
3. An advantage of using common dictionary words is that the URL would be easier to memorize and type. A disadvantage is that there are fewer words in a dictionary than using a randomized string. Using common words also means that people can easily guess some user's shortened url.

## Synchronization Questions

1. The first one is not registering a url for shortening, but to register a base URL that will be used later in shortened URLs. The second sync is actually registering a URL to be shortened so that it becomes a base URL + a random string.
2. It is not always used because sometimes the variable name does not convery exactly what the argument is.
3. The first two syncs happen when the user is requesting to execute an action. The third sync happebs when the action is excuted.
4. We won't need the first sync and for the second sync, the shortUrlBase will always be set to the domain.
5. **sync** ExpireURL

**when** ExpiringResource.expireRecource: (resource: Resource)

**then** URLShortening.delete(resource: String)

## Extending the Design
- auth
- user
- link meta data

- syncs -> 