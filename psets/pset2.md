# Problem Set 2

## Concept Questions

1. There are many contexts that need to have unique strings, but these contexts do not overlap (i.e. if a string is in one context but not the other, the other context can still use that string). In the URL shortening app, there can be a context for each URLBase. Each context would store the suffix strings that each URL base has.
2. NonceGeneration stores used strings so that it won't generate the same string for the same URL base later on. If a counter was used, the counter can represent the last used string in a set list of strings.
3. An advantage of using common dictionary words is that the URL would be easier to memorize and type. A disadvantage is that there are fewer words in a dictionary than using a randomized string. Using common words also means that people can easily guess some user's shortened url.

## Synchronization Questions

1. The first one is not registering a url for shortening, but to register a base URL that will be used later in shortened URLs. The second sync is actually registering a URL to be shortened so that it becomes a base URL + a random string.
2. It is not always used because sometimes the variable name does not convery exactly what the argument is.
3. The first two syncs happen when the user is requesting to execute an action. The third sync happens when the action is excuted.
4. We won't need the first sync and for the second sync, the shortUrlBase will always be set to the domain.
5. **sync** ExpireURL

**when** ExpiringResource.expireRecource: (resource: Resource)

**then** URLShortening.delete(resource: String)

## Extending the Design

1.

**concept** UserUrls

**purpose** record keeping for urls a user makes

**principle** after registering for a short url, save the url to the user. deleting the url will remove it from the user

**state**

-   a set of Users with
    -   a set of shortUrls

**actions**
add (user: User, shortUrl: String)

-   **requires** shortening to not already exist in user's set
-   **effect** adds shortened url to user set

delete (user: User, shortUrl: String)

-   **requires** shortening to exist in user's set
-   **effect** deletes shortened url to user set

authorize (user: User, shortUrl: String)

-   **effect** authorize if shortUrl is associated with user

**concept** UrlMetaData

**purpose** track number of times a shortened url was accessed as meta data for a user's analysis

**principle** upon creation, keep track of meta data for that url. When the shortened url is deleted, the meta data is deleted as well.

**state**

-   a set of sortUrls with
    -   a count timesAccessed

**actions**
create (shortUrl: String)

-   **requires** shortened url to not already be in the set
-   **effect** adds shortened url to list

update (shortUrl: String)

-   **requires** shortened url to exist in set
-   **effect** increments count timesAccessed for shortened URL

delete (shortUrl: String)

-   **requires** shortened url to exist in set
-   **effect** removes shortened url from the list

lookup (user: User, shortUrl: String)

-   **requires** shortened url to exist in set
-   **effect** returns the number of times the url was accessed

2.

-   create shortening -> create new analytic for it, add to created by

**sync** createMetaData

**when** UrlShortening.register(): (shortUrl)

**then** UserUrls.addShortening()
UrlMetaData.create()

-   shortening translated to target -> increment

**sync** incrementVisitCount

**when** UrlShortening.lookup(shortUrl: String): (targetUrl: String)

**then** UrlMetaData.update(shortUrl)

-   user examines analytics -> return only their url analytics

**sync** getMetaData

**when** Request.lookupMetaData(shortUrl: String)

**then** UserUrls.authorize(user: User, shortUrl: String)

3.

-   To allow users to create their own short URLs, during the registering sync, we check to see if they provide a string. If they do not, then we generate a nonce for them, else we use the string given.
    possible, change sync for registering (check if string provided, generate if not)
-   To use a word as a nonce, we can add a new concept that saves or has access to a list of words. The concept will have a state to keep track of which words it has already used, similar to the nonce generation concept we have.
-   Adding the target URl to the analytic sis not needed because multiple users might have made shortened URLs for the same target URL and they do not need to see the combined analytic of how many visits/clicks they got all together for that target URL. Furthermore, analytics for a URL should only be viewable to the user who created it, so grouping together all the shorten URLs with the same target URL would not be a good idea.
-   Generating short URLs that are not easily guessed can be done though the nonce generation. It can generate a random string, which would make it harder to guess.
-   To allow unregisterd users to view analytics on their URLs, we can ask them to provide some sort of identification, such as email or phone number. When they attempt to access their analytics, we cna send them a verification code/email to the contact information. This in a way can work as an alternative to registering. We would need a new concept to keep track of these users, saving in the state the set of such users and their contact info. It would have actions to add, remove, and vertify people.
