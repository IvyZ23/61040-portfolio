# Problem Set 2

## Concept Questions

1. Contexts in the URL shortening app are the different URL bases. The same nonce can be used for multiple URL bases, so the purpose of keeping track of contexts is to make sure we know which nonce has been used for which base.
2. NonceGeneration stores used strings so that it won't generate the same string for the same URL base later on. We can also use a counter, where each number maps to some unique nonce. As the counter goes up, we are guaranteed to receive nonces we haven't used yet.
3. An advantage of using common dictionary words is that the URL would be easier to memorize and type. A disadvantage is that there are fewer words in a dictionary than using a randomized string, leading to fewer possible shortned URLs. We can modify NonceGeneration to pull words from a dictionary of some sort when generating words to achieve this.

## Synchronization Questions

1. The generate sync is for generating a nonce for a given base URL, so the targetUrl argument is not needed. On the other hand, the register sync is when the targetUrl is linked with the shortened URL, so it is necessary to include it as an argument there.
2. It is not always used because sometimes the variable name does not convey exactly what the argument is. Not using the shorthand can sometimes improve readability.
3. The first two syncs happen when the user is requesting the action to shorten a URL. The third sync happens when the registration action had already been completed. The **then** section of the third sync also needed the output of UrlShortening.register.
4. We will no longer need the shortUrlBase argument when shortening a URL or generating a nounce. It would default to the fixed domain instead.
5.

**sync** ExpireURL

**when** ExpiringResource.expireResource(): (resource: Resource)

**then** URLShortening.delete(resource: Resource)

## Extending the Design

1.

**concept** UrlCreator

**purpose** allow only the creator to access the metadata of the shortened URLs they created

**principle** after registering for a short URL, associate the URL to the user that created it

**state**

-   a set of Ownership with
    -   a shortened URL shortUrl
    -   the creator of the URL User

**actions**

add (user: User, shortUrl: String)

-   **effect** associates the shortened URL with the user

delete (user: User, shortUrl: String)

-   **requires** short URL to be associated with the user
-   **effect** deletes association between user and shortened URL

authorize (user: User, shortUrl: String): (ok: Boolean)

-   **effect** authorize if shortened URL is associated with user

\
\
**concept** UrlMetadata

**purpose** record and view usage metadata for shortened URLs

**principle** when a shortened URL is looked up, increment count. Only the creator of the shortened URL can view its metadata

**state**

-   a set of Metadata with:
    -   a shortened URL shortUrl
    -   a count Count

**actions**

update (shortUrl: String)

-   **effect** increments count Count for shortened URL

delete (shortUrl: String)

-   **requires** shortened URL with metadata to exist in set
-   **effect** removes metadata for shortened URL from the list

lookup (user: User, shortUrl: String): (count: Number)

-   **requires** shortened URL with metadata to exist in set
-   **effect** returns metadata for the URL

2.

**sync** registerCreator

**when**
Request.shortenUrl (user)\
UrlShortening.register(): (shortUrl)

**then** UrlCreator.add(user, shortUrl)

\
**sync** recordLookup

**when** UrlShortening.lookup(shortUrl: String)

**then** UrlMetadata.update(shortUrl: String)

\
**sync** getMetadata

**when** Request.lookupMetadata(shortUrl: String, user: User)\
UrlCreator.authorize(user: User, shortUrl: String): (ok: Boolean)

**then** UrlMetadata.lookup(user: User, shortUrl: String)

3.

-   To allow users to create their own short URLs, during the registering sync, we check to see if they provide a string. If they do not, then we generate a nonce for them, else we use the string given if it is not used for the same base URL already.
-   To use a word as a nonce, we can add a new concept that has access to a dictionary of words. The concept will have a state to keep track of which words it has already used, similar to the nonce generation concept we have, but instead of randomly generating a string, it pulls from that dictionary.
-   In UrlMetadata, also record the targetUrl in the state. When attempting to look up metadata on this targetUrl, sum up the count of all shortened URL metadata associated with the user that has that targetUrl.
-   Generating short URLs that are not easily guessed can be done though the nonce generation. It can generate a random, crytographically secure string that is hard to guess.
-   To allow unregisterd users to view analytics on their URLs, we can ask them to provide some sort of identification, such as email or phone number. When they attempt to access their analytics, we can send them a verification code to the contact information. This in a way can work as an alternative to registering. We would need a new concept to keep track of these users, saving in the state the set of such users and their contact info. It would have actions to add, remove, and verify users.
