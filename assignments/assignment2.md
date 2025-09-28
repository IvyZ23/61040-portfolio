# Assignment 1

## Problem Statement

**Trips and Vacation** - I find traveling very fun, especially with friends and family. It's a great way to unwind, try new food, explore places I've never gone before, and gain new experiences.

### Problem

**Disorganized Group Trip Planning** - Group trips and traveling with friends and family sounds fun, but planning for them is often not. Planning for a trip is a complicated task that requires many steps, from finding a hotel to planning itineraries. When traveling with family and friends, this task can become more difficult, adding on the new issue of accommodating everyone's preferences and budgets.

### Stakeholders

-   Trip Participants – friends or family who want to go on trips together but struggle with the coordination process
-   Trip Organizers – person in the group who usually takes on the responsibility of coordinating logistics and faces most of the stress
-   Travel Service Providers (airlines, hotels, activity companies) – affected when group bookings get delayed, changed, or canceled because of disorganized planning

### Evidence

1. [As Vacation Season Heats Up, Many Americans Are Feeling Stressed Over Travel Planning](https://civicscience.com/as-vacation-season-heats-up-many-americans-are-feeling-stressed-over-travel-planning): 71% of U.S. adults who make travel arrangements say the process is at least somewhat stressful.
2. [Many Tourists Dread Travel Planning, Report Says](https://www.floridatravellife.com/where-to-stay/travel-struggles-study-guidegeek/): 63% of people reported having had “bad experiences” planning a past vacation and 30% say research and preparing is “too time-consuming”.
3. [Simplifying the trip planning experience for group trip leaders](https://medium.com/design-bootcamp/simplifying-the-trip-planning-experience-for-group-trip-leaders-a-product-design-case-study-ffc3e10eb547): Through interviews, the author found that group trip planners often “labour long hours researching and organizing different tools and services while trying to coordinate with everyone”
4. [A Case-Based Approach to Understanding Vacation Planning](https://www.nrs.fs.usda.gov/pubs/jrnl/1999/nc_1999_stewart_001.pdf): Travel planning involves making many decisions under uncertainty and trying to meet multiple goals, making the process cognitively complex
5. [Friends, Fun and Finances: Experian Survey Explores How Travelers Handle Group Vacation Costs](https://www.experian.com/blogs/ask-experian/survey-financial-stress-of-traveling-with-friends/): The survey explores financial challenges faced when traveling with friends like cost division and budgeting
6. [Group Trip Planning Query Problem with Multimodal Journey](https://arxiv.org/pdf/2502.03144): The formal “Group Trip Planning Query” problem shows even computationally, selecting which points of interest and which modes of travel to take is complex for groups

### Comparables

1. [Troupe](https://app.troupe.com/): A group trip planning app where friends can suggest dates, vote on destinations, and collaborate on itineraries. Focuses mostly on destination and date voting and not handling budgets or splitting costs.
2. [Out of Office](https://www.instagram.com/): A travel discovery and recommendation app with some features for sharing trip ideas with friends. Prioritizes recommendations rather planning and scheduling.
3. [Tripit](https://www.tripit.com/web): Organizes travel confirmations (flights, hotels, rental cars) into a single itinerary that can be shared with others. Focuses on individual trip planning rather than group and doesn’t handle polls or budgetting.
4. [Roadtrippers](https://roadtrippers.com/): Helps travelers plan road trips by mapping routes and share with friends. Limited to only road trips and does not have features for budgeting and traveling with groups.
5. Google Docs/Sheets: Used for planning and tracking itineraries. Requires manual setup and is not designed for travel planning.

## Application Pitch

Group trips are fun, but planning for them is often not. Differing preferences on destination, dates, and cost splitting can become overwhelming, espcially when these pieces of information are scattered everywhere. **GroupGetaway** makes planning seamless so the only thing groups need to worry about for their trip is having a good time.

**Planning Made Simple** – GroupGetaway allows trip members to vote on dates, destinations, and activities. This ensures decisions are made fairly and efficiently. For trip participants, this means faster planning. For trip organizers, it removes the burden of trying to please everyone. Travel service providers benefit because clearer decisions lead to fewer last minute cancellations.

**Smart Budgeting and Cost Splitting** – Money is one of the biggest sources of stress in group trips. With GroupGetaway, everyone sets a personal budget and the app will keep track of shared costs. Expenses can be split evenly or adjusted based on who will pay for what. Trip participants gain transparency and stress less before and during the trip. Organizers save time.

## Concept Design

**concept** PasswordAuthentication

**purpose** limit access to known users and limit acccess each user has

**principle** after a user registers with a username and a password,
they can authenticate with that same username and password
and be treated each time as the same user

**state**

a set of Users with

-   a username String
-   a password String

**actions**

register (username: String, password: String): (user: User)

-   **requires** username does not already exist
-   **effects** creates new user

authenticate (username: String, password: String): (user: User)

-   **requires** user with username and password to exists
-   **effects** returns that user

\
\
**concept** TripPlanner [User]

**purpose** keep details about a trip all in one place

**principle** after a user creates a trip for a group,
the user can keep add and remove participants to the group,
record the location and time the trip will occur. Users can
update their budget for the trip. The creator of the trip can
finalize the trip once no more changes are needed or delete the trip
if it is no longer happening.

**state**

a set of Trips with

-   a name String
-   a finalized Flag
-   an owner User
-   a set of Pariticipants
-   a destination String
-   a dateRange DateRange

a set of Participants with

-   a user User
-   a budget Number

**action**

create(user:User, destination: String, dataRange: DataRange, name: String): Trip

-   **requires** trip under user with same destination and date range not to already exist
-   **effects** creates new trip

update(user: User, trip: Trip, destination: String, date: DateRange, name: String)

-   **requires** trip that belongs to user to exist
-   **effects** updates trip info

finalize(user: User, trip: Trip, finalize: Flag)

-   **requires** trip that belongs to user to exist
-   **effects** updates finalized flag of trip

delete(user: User, trip: Trip)

-   **requires** trip that belongs to user to exist
-   **effects** deletes trip

addParticipant(user: User, trip: Trip)

-   **requires** user to not already exist in trip
-   **effects** adds user to trip

updateParticipant(user: User, budget: Number, trip: Trip)

-   **requires** user to exist as a participant of trip
-   **effects** updates user info in trip

removeParticipant(user: User, trip: Trip)

-   **requires** user to exist as a participant of trip
-   **effects** removes user from trip

\
\
**concept** PlanItinerary [Trip]

**purpose** allow for easier itinerary crafting between multiple people

**principle** an itinerary is created for a trip. Users can add and remove events from
the intinerary. Added events await approval before being offically added. If it is not
approved, it will not be added to the itinerary.

**state**

a set of Itineraries with

-   a trip Trip
-   a set of Events
-   a finalized Flag

a set of Events with

-   a name String
-   a cost Number
-   a pending Flag
-   an approved Flag

**action**

create(trip:Trip): Itinerary

-   **requires** itinerary for trip to not already exist
-   **effects** creates new itinerary for trip

addEvent(name: String, cost: Number, itinerary: Itinerary)

-   **effects** add new pending event to the itinerary

updateEvent(event: Event, name: String, cost: Number, itinerary: Itinerary)

-   **requires** event in itinerary to exist
-   **effects** updates event

approveEvent(event: Event, approved: Flag, itinerary: Itinerary)

-   **requires** event to exist in itinerary
-   **effects** sets approval flag for itinerary and update pending to false

finalizeItinerary(intinerary: Itinerary, finalized: Flag)

-   **requires** itinerary to exist
-   **effects** sets itinerary finalized to given flag

\
\
**concept** Polling [User, Option]

**purpose** use majority vote to make a decision

**principle** a user creates a poll and adds or removed options to it.
They add and remove users to the poll. The users can vote on the options. Once the
poll is closed, the result is finalized.

**state**

a set of Polls with

-   a name String
-   a set of Users
-   a set of Options
-   a creator User
-   a set of Votes
-   a closed Flag

a set of Options with

-   an option Option

a set of Votes with

-   a user User
-   a vote Option

**actions**

create(user: User, name: String): Poll

-   **requires** a poll under the user with the same name not to already exist
-   **effects** creates new poll

addOption(poll: Poll, option: Option)

-   **requires** option to not already exist in poll
-   **effects** adds option to poll

removeOption(poll: Poll, option: Option)

-   **requires** option to exist in poll
-   **effects** removes option from poll

addUser(user)

-   **requires** user to not already be added to poll
-   **effects** adds user to poll

removeUser(user)

-   **requires** user to already be added to poll
-   **effects** removes user from poll

addVote(user: User, vote: Option, poll: Poll)

-   **requires** user and option to exist in poll and poll to not be closed
-   **effects** adds new vote to poll

updateVote(user: User, new: Option, poll: Poll)

-   **requires** user's vote to exist in poll and poll to not be closed
-   **effects** updates the user's vote with new option

close(user: User, poll: Poll)

-   **requires** poll to exist and the user to be the owner
-   **effects** closes poll

getResult(poll: Poll): Option

-   **requires** poll to exist
-   **effects** returns the highest voted option

\
\
**concept** CostSplitting [Itinerary, Item]

**purpose** allow for easier planning on how an expense would be paid for

**principle** An expense is created. Users can add themselves as
a contributor and cover a certain amount of the expense. Once the expense
has been fully covered, users can no longer contribute.

**state**

a set of Expenses with

-   an item Item
-   a cost Number
-   a set of Contributors
-   a covered Flag

a set of Contributors

-   a user User
-   a amount Number

**actions**

create(item: Item, cost: Number): Expense

-   **requires** item to not already be added as an expense
-   **effects** creates new expense

remove(expense: Expense)

-   **requires** expense to exist
-   **effects** deletes expense and contributions associated with it

addContribution(user: User, expense: Expense, amount: Number)

-   **requires** expense to exist and amount to not be more than what is needed
-   **effects** if user already exists as contributor, merge the amounts, else add user as a new contributor

updateContribution(user: User, new: Number, expense: Expense)

-   **requires** user to exist as a contributor for expense
-   **effects** updates user's contribution amount

\
\
**syncs**

**sync** createTrip

**when** Polling.close() for trip destination

Polling.close() for trip dates

Polling.getResult() for trip desintation: (destination: String)

Polling.getResult() for trip dates: (date: DateRange)

**then** TripPlanner.create(user, destination, date)

\
\
**sync** createItinerary

**when** TripPlanner.create(): Trip

**then** PlanIntinerary(trip: Trip)

\
\
**sync** eventExpense

**when** PlanItinerary.addEvent()

**then** CostSplitting.create(intinerary: Itinerary, item： Item, cost: Number)

\
\
Having user accounts and authenication ensures that users are only modifying the trips they're part of. It will also limit them to voting for polls they're added to and modifying itineraries of trips they're in. The Poll concept is used to make decisions for trips and trip itineraries through majority vote. The CostSplitting concept would allow users to divide the cost of each component of the trip amongst themselves. The Items in CostSplitting can be activities/experiences added to itineraries. The TripPlanner and PlanItinerary concepts are core to the app because they are what allow users to keep track of and customize their trip.

## UI Sketches

Home page:
![home page](/assets/assignment2_1.jpg)

Create poll page:
![home page](/assets/assignment2_2.jpg)

Poll voting page:
![home page](/assets/assignment2_3.jpg)

Trip planning page:
![home page](/assets/assignment2_4.jpg)

## User Journey

A user is excited to plan a summer trip with their friends, but quickly becomes overwhelmed when the group chat floods with conflicting messages. Everyone is suggesting destinations, proposing different weeks, and throwing out their budgets. The user feels frustrated, unsure how to keep track of all the information or how to settle on an idea.

The user signs up for our group trip planning app, GroupGetaway, and clicks the “+” button in the bottom right corner of the home page. They set up two quick polls: one for the trip’s destination, with options like Rome, Barcelona, and Paris, and another for possible date ranges in June and July. Once the poll has been created, the user shares the poll with their friends.

Within a day, the user checks the poll results in the app and sees that Rome in mid-June has emerged as the confirmed choice. Relieved, they close the poll, and GroupGetaway automatically creates a new trip planner for the group. All their friends are added as members. The friends click into the trip planner from the home page and the app prompts each to enter their personal budget.

Once budgets are visible, the group begins adding potential hotels, flights, and activities to the trip’s itinerary by clicking the plus button in the expenses section of the trip planning page. Each suggestion appears as a pending experience, with estimated costs attached. Friends vote on whether to include them, and the user watches as the itinerary takes shape: the Colosseum tour and pasta-making class are confirmed, while more expensive activities, like a hot-air balloon ride, are politely voted down. The budgeting tool tracks expenses as they’re added, showing everyone what the total trip cost will look like and allows the friends to decide how costs are to be split.

After a week of back-and-forth planning, the group has a finalized itinerary that fits everyone’s budget . The user finalizes the trip, knowing the plan can sitll be easily accessed during travel. What began as a chaotic stream of messages is now an organized, shared trip plan that everyone feels good about. Confident and stress-free, the group starts preparing for their Roman adventure, knowing they can open GroupGetaway anytime to stay on track.
