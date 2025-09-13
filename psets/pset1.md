# Problem Set 1

## Exercise 1

1. An invariant of the state is that users shouldn't be able to purchase more of an item than was requested. Another invariant is that each purchased item should be a requested item. The second invariant is more important because buying an item the owner of the registry did not ask for would defeat the purpose of the concept. An action that is affected by this is purchase. The action preserves this invariant by requiring the registry to at least request one of the item the user is trying to buy.
2. removeItem can potentially break the invariant. If a user had already purchased a requested item, then the recipient deleted the item, this would result in an unwanted purchased item. To fix this, a possible solution would be to prevent the recipient from deleting any item that has already been purchased.
3. The registry can be opened and closed repeatedly. As stated in the spec of create, the registry is closed when it is first created so we have to allow the owner of the registry to open it once they are ready. Allowing repeated openings and closings would also allow users to prepare the registry (e.g. add/update all the items first) before opening it to the purchasers.
4. It is okay not to have a delete action because the registry owner can close the registry. However, this might introduce unneccesary clutter on the registry owner's side.
5. Two common queries that mutate the state are addItem and purchase.
6. To implement the feature of masking purchases from the recipient, we can add a new property to the registry state (a flag Visible). For the create action, we would add an extra parameter to allow registry creators to set Visible.
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

-   confirm(username: String, token: String)

    -   **requires** none
    -   **effects** update user to confirmed if token matched user's confirm token

-   authenticate (username: String, password: String): (user: User)
    -   **requires** none
    -   **effects** grant access if username and password matches one user and they have been confirmed

## Exercise 3

**concept** PersonalAccessToken
**purpose** grant users access to Github from external sources
**principle** A registered user creates a token and can use it to authenticate to receive access

-   create (user: User): (token: String)

    -   **requires** user to exist
    -   **effects** creates token for user

-   authenticate (user: User): (token: String)

    -   **requires** user to exist
    -   **effects** creates token for user

**state**

-   a set of users with
    -   a username Username
    -   a password Password
    -   a token Token

Unlike PasswordAuthentication, which happens directly on GitHub’s website and provides full access to the user’s account, PersonalAccessToken is used from external sources like the command line or the GitHub API and grants more limited access (such as to repositories).

I think the Github page can be improved by being more clear on the differences between using password and a token. Most of the time, they mention them as similar entities.

## Exercise 4

### URL Shortener

**concept** URLShortener
**purpose** provide users with more readable URLs for sharing
**principle** The user provides a link they want shortened, perhaps with a suffix they would like the new shortened URL to use. If no suffix is provided, a random one is chosen. The user receives a shorter URL, which redirects to the URL they originally provided.
**state**

-   a set of URLs with
    -   an redirect link Original
    -   a shortened link Shortened
    -   a user User

**actions**

-   create (user: User, url: String, suffix: String): (shortenedURL: URL)

    -   **requires** user to exist and suffix to not already exist
    -   **effects** creates a shortened URL

-   delete (url: URL)

    -   **requires** url to exist
    -   **effects** deletes url

### Billable Hours Tracking

**concept** Timesheet
**purpose** Make sure companies are getting paid proportional to amount of time spent on project
**principle** Company create projects. Employees can create a session, which contains information on when the session started, ended, what was to be done during this time, and for which project it is for.
**state**

-   a set of Projects with

    -   a name Name
    -   a flag Complete

-   a set of Sessions with
    -   a start time Start
    -   a end time End
    -   a project Project
    -   an employee Employee
    -   a note to detail work done Note

**actions**

-   createProject (name: String): (project: Project)

    -   **requires** there doesn't already exist a project with given name
    -   **effects** creates new project with a flag of incomplete

-   completeProject (project: Project): (project: Project)

    -   **requires** project exists
    -   **effects** updates the project's flag to complete

-   createSession (employee: Employee, start: Time, project: Project, note: String): (session: Session)

    -   **requires** requires employee and project to exist
    -   **effects** creates a new session with pending end time

-   endSession(session: Session, end: Time)
    -   **requires** session to exist and end time to be later than start time
    -   **effects** updates the session to reflect end time

A way to combat someone forgetting to fill in end time is to send the employee a reminder if the end date is not filled in by the end of the work day.

### Conference Room Booking

**concept** BookRoom
**purpose** Prevent overbooking of rooms and allow users to find available rooms as quickly as possible
**principle** The company or university can imput rooms available for booking and the time slots they available. Users can reserve or cancel reservations of these rooms at available times.
**state**

-   a set of Rooms with

    -   a name/room number String

-   a set of Slots with

    -   a room Room
    -   a time Time

-   a set of Bookings with - a user User - a slot Slot
    **actions**

-   createRoom (name: String): (room: Room)

    -   **requires** room with name does not already exist
    -   **effects** creates room

-   deleteRoom(room: Room)

    -   **requires** room to exist
    -   **effects** removes room and slots and bookings associated with room

-   addSlot(room: Room, time: Time)

    -   **requires** room to exist and a time slot for that room not to exist already
    -   **effects** add new time slot for room

-   deleteSLot(slot: Slot)

    -   **requires** slot to exist
    -   **effects** deletes slot and bookings associated to it

-   book(user: User, slot: Slot): (booking: Booking)

    -   **requires** user to exist and slot to exist and has not already been booked
    -   **effects** adds new booking to set

-   unbook(booking: Booking)
    -   **requires** booking to exist
    -   **effects** remove booking from set
