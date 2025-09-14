# Problem Set 1

## Exercise 1

1. An invariant of the state is that users shouldn't be able to purchase more of an item than was requested. Another invariant is that each purchased item should be a requested item. The second invariant is more important because buying an item the owner of the registry did not ask for would defeat the purpose of the concept. An action that is affected by this is purchase. The action preserves this invariant by requiring the registry to at least request one of the item the user is trying to buy.
2. removeItem can potentially break the invariant. If a user had already purchased a requested item, then the recipient tries to delete the item, this would result in an unwanted purchased item. To fix this, a possible solution would be to prevent the recipient from deleting any item that has already been purchased.
3. The registry can be opened and closed repeatedly. The specs of open and close do not check if the registry had already been opened/closed before. As stated in the spec of create, the registry is closed when it is first created so we have to allow the owner of the registry to open it once they are ready and close it again when they're done. Allowing repeated openings and closings would allow users to prepare the registry (e.g. add/update all the items first) before opening it to the purchasers. They may also want to reopen a registry if they did not receive all the items they requested on there.
4. It is okay not to have a delete action because the registry owner can close the registry.
5. Two common queries that mutate the state are addItem and purchase.
6. To implement the feature of masking purchases from the recipient, we can add a new property to the registry state (a flag Visible). For the create action, we would add an extra parameter to allow registry creators to set Visible. If Visible is set to false, then the purchase information will be masked from them.
7. We only need to differentiate and identify items. We do not need details such as names and prices because we only need to know if an item is requested and if it has been bought. A SKU code is enough information for that.

## Exercise 2

1. **state**

-   a set of User with
    -   a username Username
    -   a password Password

2. **actions**

-   register (username: String, password: String): (user: User)

    -   **requires** username does not already exist
    -   **effects** creates new user

-   authenticate (username: String, password: String): (user: User)
    -   **requires** none
    -   **effects** grant access if username and password matches one user

3. There must not be any two users who share the same username. It is preserved during user registration. If the username already exists, they will not be allowed to registered.

4. **state**

-   a set of User with
    -   a username Username
    -   a password Password
    -   an email Email
    -   a flag Confirmed
    -   a confirmation token Token

**actions**

-   register (username: String, password: String, email: String): (user: User)

    -   **requires** username does not already exist
    -   **effects** creates new user

-   confirm(user: User, token: String)

    -   **requires** none
    -   **effects** update user to confirmed if token matched user's confirm token

-   authenticate (username: String, password: String): (user: User)
    -   **requires** none
    -   **effects** grant access if username and password matches one user and they have been confirmed

## Exercise 3

**concept** PersonalAccessToken
**purpose** grant users access to Github from external sources
**principle** A registered Github user creates a token and can use it to authenticate to receive access. The user can set how long the token lasts before it expires and what the token can be used to access (the scope). Once the time comes, the token expires and is rendered invalid.

**state**

-   a set of Tokens with
    -   a token value String
    -   a user User
    -   an expiration time Time
    -   an access scope Scope

**actions**

-   createToken (user: User, scope: Scope, expiration: Time): (token: String)

    -   **requires** none
    -   **effects** creates token for user

-   verifyToken (user: User, token: String):

    -   **requires** none
    -   **effects** checks for a Token owned by user to see if it matches

-   deleteToken(token: Token):

    -   **requires** requires token to exist
    -   **effects** deletes token from the set

Unlike PasswordAuthentication, which happens directly on GitHub’s website and provides full access to the user’s account, PersonalAccessToken is used from external sources like the command line or the GitHub API and grants more limited access (such as to specifc repositories).

The Github page referred the access tokens and passwords as very similar concepts, that they were both for accessing Github. They can make the difference clearer by stating the difference in access the two provides.

## Exercise 4

### URL Shortener

**concept** URLShortener
**purpose** provide users with more readable URLs for sharing
**principle** The user provides a link they want shortened, perhaps with a suffix they would like the new shortened URL to use. If no suffix is provided, a random one is chosen. The user receives a shorter URL, which redirects to the URL they originally provided.
**state**

-   a set of URLs with
    -   a redirect link Original
    -   a shortened link Shortened
    -   a user User

**actions**

-   create (user: User, url: String, suffix: String | None): (shortenedURL: URL)

    -   **requires** user to exist and suffix to not already exist
    -   **effects** creates a shortened URL using suffix or a random string if suffix is not provided

-   delete (url: URL)

    -   **requires** url to exist
    -   **effects** deletes url from set

### Billable Hours Tracking

**concept** Timesheet
**purpose** Make sure companies are getting paid proportional to amount of time spent on services they provide
**principle** Employees start a session when they're about to work. They input the start time and the tasks that are to be done. Once they're done with their tasks, they can end the session by inputing the end time.
**state**

-   a set of Sessions with
    -   a start time Start
    -   a end time End
    -   an employee Employee
    -   a note to detail work done Note

**actions**

-   startSession (employee: Employee, start: Time, note: String): (session: Session)

    -   **requires** requires employee to exist
    -   **effects** creates a new session with pending end time

-   endSession(session: Session, end: Time)
    -   **requires** session to exist and end time to be later than start time
    -   **effects** updates the session to reflect end time

A way to combat someone forgetting to fill in end time is to send the employee a reminder if the end date is not filled in by the end of the work day.

### Conference Room Booking

**concept** BookRoom
**purpose** Ensure users that the room will be available when they need it
**principle** The company or university input rooms available for booking and the time slots they available. Users can reserve or cancel reservations of these rooms at available times.
**state**

-   a set of Rooms with

    -   a name/room number String

-   a set of Slots with

    -   a room Room
    -   a time Time

-   a set of Bookings with
    -   a user User
    -   a slot Slot

**actions**

-   addRoom (name: String): (room: Room)

    -   **requires** room with name does not already exist
    -   **effects** adds room to set

-   deleteRoom(room: Room)

    -   **requires** room to exist
    -   **effects** removes room and slots and bookings associated with room

-   addSlot(room: Room, time: Time)

    -   **requires** room to exist and a slot at that time for that room not to exist already
    -   **effects** add new time slot for room

-   deleteSLot(slot: Slot)

    -   **requires** slot to exist
    -   **effects** deletes slot and bookings associated with it

-   bookRoom(user: User, slot: Slot): (booking: Booking)

    -   **requires** user to exist and slot to exist and has not already been booked
    -   **effects** adds new booking to set

-   unbookRoom(booking: Booking)
    -   **requires** booking to exist
    -   **effects** remove booking from set
