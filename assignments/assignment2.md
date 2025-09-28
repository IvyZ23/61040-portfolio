# Assignment 1

## Problem Statement

**Trips and Vacation** - I find traveling very fun, esspcially with friends and family. It's a great way to unwind, explore places I've never gone before, and gain new experiences.

### Problem

**Disorganized Group Trip Planning** - Group trips and traveling with friends and family sounds fun, but planning for them is often not. Planning for a trip is a complicated task that requires many steps, from finding a hotel to planning itineraries. When traveling with family and friends, this task can become more difficult, adding on the new issue of accommodating everyone's preferences and budgets.

### Stakeholders

-   Trip Participants – friends or family who want to go on trips together but struggle with the coordination process
-   Trip Organizers – person in the group who usually takes on the responsibility of coordinating logistics and faces most of the stress.
-   Travel Service Providers (airlines, hotels, activity companies) – affected when group bookings get delayed, changed, or canceled because of unorganized planning

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

Planning group trips should be fun, but too often it becomes stressful with long text threads, confusion over dates, disagreements about where to go, and messy cost splitting. **GroupGetaway** makes planning seamless so the only thing groups need to worry about is having a good time.

**Polling Made Simple**– GroupGetaway lets trip members vote on dates, destinations, and activities. This ensures decisions are made fairly and efficiently. For trip participants, this means less back-and-forth and faster planning. For trip organizers, it removes the burden of trying to please everyone. Travel service providers benefit because clearer decisions lead to fewer last-minute cancellations.

**Smart Budgeting & Cost Splitting** – Money is one of the biggest sources of stress in group trips. With GroupGetaway, everyone sets a personal budget, and the app keeps track of shared costs automatically. Expenses can be split evenly or adjusted based on who will pay for what. Trip participants gain transparency, organizers save time, and fewer financial misunderstandings reduce stress during and after the trip.

**Shared Itinerary Builder** – Once dates, destinations, and budgets are set, GroupGetaway provides a shared trip timeline that includes travel details, lodging, and activities. Everyone can see what’s planned. Trip participants know exactly what’s happening, organizers stay on top of logistics, and service providers benefit from smoother, timely bookings.

With GroupGetaway, groups can stop stressing about logistics and start looking forward to their adventures.

## Concept Design

concept PasswordAuthentication
purpose limit access to known users
principle after a user registers with a username and a password,
they can authenticate with that same username and password
and be treated each time as the same user
state
a set of Users with - a username String - a password String
actions
register (username: String, password: String): (user: User) - **requires** username does not already exist - **effects** creates new user
authenticate (username: String, password: String): (user: User) - **requires** user with username and password to exists - **effects** returns that user

concept TripPlanner [User]

purpose Keep track of who is going on a trip and what activities the trip will entail

principle A user creates a trip for a group. The user and the group can add and choose activities to do for the trip.

state
a set of Trips with

-   an owner User
-   a set of Pariticipants
-   a destination String
-   a dateRange DateRange

a set of Participants with

-   a user User
-   a budget Number

action
create(user:User, destination: String, dataRange: DataRange)

addParticipant(user: User, budget: Number)

updateParticipant(user: User, budget: Number)

removeParticipant(user: User, budget: Number)

concept PlanItinerary [Trip]

purpose Keep track of and agree on activities to do on a trip

principle An itinerary is created for a trip. Users can add events to the intinerary, which needs to be approved to be officially added to the itinerary.

state
a set of Itineraries with

-   a trip Trip
-   a set of Events
-   a finalized Flag

a set of Events with

-   a name String
-   a cost Number
-   a pending Flag
-   an approved Flag

action
create(trip:Trip): Itinerary

addEvent(name: String, cost: Number)

updateEvent(event: Event, approved: Flag)

finalizeItinerary(intinerary)

--

concept Polling [User, Option]

purpose Vote to see what the majority agrees on to make a decision

principle A user creates a poll and invites other users to it. The users can vote on what destinations and times they want to go on a trip

state
a set of Polls with

-   a set of Users
-   a set of Options
-   a creator User
-   a set of Votes

a set of Options with

-   an option Option

a set of Votes with

-   a user User
-   a vote Option

actions
create(user: User): Poll

addOption(poll: Poll, option: Option)

removeOption(poll: Poll, option: Option)

addUser(user)

removeUser(user)

addVote(user, vote: Option)

updateVote(user, vote: Option)

close(poll: Poll, user: User)

calcWinner(poll: Poll): Option

concept CostSplitting [Itinerary, Item]

purpose Allow expenses to be split amongst users

principle A user creates an expense. Other users can add themselves as a contributor and cover a certain amount of the expense.

state
a set of Expenses with

-   an item Item
-   a total Number
-   a itinerary Itinerary
-   a set of Contributors
-   a covered Flag

a set of Contributors

-   a user User
-   a amount Number

actions
create(itinerary: Itinerary, item: Item, cost: Number): Expense

close(user, expense)

addContribution(user: User, expense: Expense, amount: Number)

removeContribution(user: User, expense: Expense)

-   syncs

sync createTrip
when DestinatonPolling.close()
DatePolling.close()
DestinationPolling.calcWinner(): (destination: Option)
DatePolling.calcWinner(): (date: Option)
then TripPlanner.create(user, destination, date)

sync createItinerary
when TripPlanner.create(): Trip
then PlanIntinerary(trip: Trip)

sync eventExpense
when PlanItinerary.addEvent()
then CostSplitting.create(intinerary, item, cost: Number)

## UI Sketches

## User Journey

A user is planning to have a trip with their friends. There are too many things to keep track of, including ideas of where to go and when to go. The user opens our trip planning app and clicks the plus button on the bottom right of the screen to create a new poll. They input the destinations and dates their friends were suggesting and clicks "Create". The poll is sent out to their friends. The app shares the poll with their friends. Their friends select the options they want on the polls page and click "Submit". Once the poll closes, a trip planner is created for them. On the page, the user and their friends can click "Edit" next to their profiles on the sidebar to update their budgets. They can also click "Add New" in the itinerary section to create a new poll for an activity. They input the activity's name and budget needed. The rest of the group vote whether they want it or not. Once it is decided, the user and their friends can divide the cost for the activity. Once the user thinks planning has been finalized, they can close planning for the trip. The user is satified, as they didn't need to deal with long chats and disorganized information.
