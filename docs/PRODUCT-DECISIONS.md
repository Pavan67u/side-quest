# Document 6 — `PRODUCT-DECISIONS.md`

This document is the **decision log** for SideQuest. Its purpose is to prevent product drift while we move from idea → UX → architecture → development.

````markdown
# SideQuest — Product Decisions

> Status: Active
> Product codename: SideQuest
> Public brand name: TBD
> Primary launch audience: College students + young professionals
> Minimum age: 16+
> Last updated: 2026-09-18

---

# 1. Purpose

This document records important product decisions made for SideQuest.

It exists to prevent:

- Feature creep
- Product drift
- Repeated debates
- Contradictory requirements
- Accidental transformation into a generic social network

When a new feature or requirement is proposed, it should be evaluated against these decisions.

---

# 2. Core Product Principle

## Decision

SideQuest is built around:

> **"I want to do this. Who else wants to join?"**

## Consequence

The activity is the center of the product.

The product should not primarily ask:

> "Who do you want to meet?"

It should ask:

> "What do you want to do?"

---

# 3. Primary Product Object

## Decision

The primary object is:

> **SideQuest / Activity**

## Secondary Objects

- User
- Activity Participant
- Message
- Notification
- Report
- Block

## Consequence

Product architecture should revolve around activities.

The user is not the primary discovery object.

---

# 4. Activity-First Product

## Decision

SideQuest is activity-first rather than people-first.

## Example

Correct:

```text
5v5 Football
7/10 joined
Need 3
Tonight · 8:30 PM
````

Incorrect direction:

```text
People near you

Arjun
Rahul
Pavan
...
```

## Reason

The reason people connect is the activity.

---

# 5. Core Product Loop

## Decision

The permanent product loop is:

```text
Want something
      ↓
Create / Discover
      ↓
Join
      ↓
Coordinate
      ↓
Do
      ↓
Complete
      ↓
Expire
```

Every major feature should strengthen this loop.

---

# 6. Anyone Can Create

## Decision

Any authenticated eligible user can create a SideQuest.

A user is not permanently classified as:

* Host
* Participant

A user can perform both roles.

## Example

```text
Monday
→ Join Football

Friday
→ Host Trek

Sunday
→ Join Watch Party
```

---

# 7. Minimum Age

## Decision

SideQuest is:

> **16+**

Users below 16 cannot create accounts or participate.

---

# 8. Host-Defined Age Eligibility

## Decision

Hosts can define an age requirement for their SideQuest.

Examples:

```text
16+
```

```text
18+
```

```text
18–25
```

```text
21–30
```

```text
Custom
```

## Rule

The host can make the activity more restrictive than the platform minimum.

A user outside the configured range cannot join/request to join.

Platform safety/legal rules may impose additional restrictions.

---

# 9. Join Modes

## Decision

Hosts choose how users join.

Two modes:

### Instant Join

Users join immediately when eligible and capacity exists.

### Request to Join

Users submit a request and the host approves or rejects it.

---

# 10. Why Host-Controlled Join Mode

Different activities have different trust requirements.

Example:

```text
Football
→ Instant Join
```

versus:

```text
Private House Gathering
→ Request to Join
```

The host should have control rather than forcing one model on every activity.

---

# 11. Capacity

## Decision

Capacity is a first-class product concept.

Example:

```text
7/10 joined
Need 3
```

When full:

```text
10/10 joined
Full
```

## Backend Requirement

The backend must enforce capacity.

The system must never produce:

```text
11/10
```

---

# 12. Simultaneous Joining

## Decision

Capacity must remain correct even when multiple users attempt to join simultaneously.

Example:

```text
4/5
```

Two users join simultaneously.

Expected:

```text
5/5
```

Never:

```text
6/5
```

## Implementation Implication

The eventual database/backend implementation must use an atomic transaction or equivalent concurrency-safe mechanism.

---

# 13. Increasing Capacity

## Decision

Hosts can increase capacity after publishing.

Example:

```text
10/10
```

becomes:

```text
10/12
```

The host should not be able to reduce capacity below the number of already-approved participants without an appropriate resolution flow.

---

# 14. Cost

## Decision

SideQuest can display an optional activity cost.

Example:

```text
₹300/person
```

## MVP Rule

SideQuest does not process payments.

Payment happens outside the platform.

## Reason

Payment infrastructure is not necessary to validate the core participant-discovery problem.

---

# 15. Visibility

## Decision

SideQuest supports three visibility modes.

### Public

Appears in discovery.

### Unlisted

Does not appear in normal discovery.

Accessible through a shared link.

### Invite-Only

Restricted to explicitly invited users.

---

# 16. Private Activities

## Decision

Private SideQuests are supported.

They can use either:

* Unlisted
* Invite-only

Private activities may be useful for:

* House gatherings
* Small groups
* Personal plans
* Private outings
* Invite-based activities

---

# 17. Location

## Decision

Location is fundamental to SideQuest.

Users should be able to:

* Discover nearby activities.
* Search another city/location.
* View approximate location before appropriate access.
* Receive exact location when authorized.

---

# 18. Location Privacy

## Decision

Exact sensitive/private locations should not be publicly exposed.

Example:

Before approval:

```text
Jubilee Hills
Hyderabad
```

After approval:

```text
Exact meeting location
```

The exact exposure rules will be finalized during the safety and UX stages.

---

# 19. Nearby Discovery

## Decision

Nearby activities are the default discovery experience.

The user should not need to manually search for their current city every time.

---

# 20. Other-City Discovery

## Decision

Users can search for activities outside their current location.

Example:

```text
Current:
Hyderabad

Search:
Bengaluru
```

This is useful for:

* Upcoming travel
* Moving to a new city
* Planning ahead
* Visiting another location

---

# 21. List-First Discovery

## Decision

The primary discovery interface should be list/card based.

Maps are secondary.

## Reason

The important information is:

* What
* When
* Where
* Who
* How many slots remain
* Join availability

A map is not required to understand those things.

---

# 22. Map

## Decision

A map is not a core MVP discovery interface.

It may be introduced later if user research demonstrates meaningful value.

---

# 23. Time-Based Discovery

## Decision

Discovery should support time-oriented views such as:

* Happening Now
* Tonight
* Tomorrow
* This Weekend
* Upcoming

## Reason

SideQuest includes spontaneous activities as well as planned activities.

---

# 24. Activity Categories

## Decision

Initial categories include:

* Sports
* Trekking
* Outdoor
* Travel
* Watch Party
* Gaming
* Movies
* Food
* Concerts
* Other

The category system must remain extensible.

---

# 25. Activity-Specific Fields

## Decision

Different activity types may have different fields.

Examples:

### Sports

* Sport
* Number of players
* Skill level
* Venue
* Cost

### Trekking

* Destination
* Difficulty
* Start point
* Transport
* Duration
* Cost

### Travel

* Destination
* Departure
* Return
* Transport
* Budget
* Seats

### Watch Party

* Event/race/movie
* Venue
* Date/time
* Capacity
* Cost

### Gaming

* Game
* Platform
* Players
* Skill level
* Location

## Status

The exact fields are not yet frozen.

They will be validated during UX/product design.

---

# 26. Activity Chat

## Decision

Activity-specific chat is part of the MVP.

## Reason

Participants need a way to coordinate.

Examples:

> "Which entrance?"

> "Where should we meet?"

> "I'll be 10 minutes late."

---

# 27. General Messaging

## Decision

General-purpose messaging is not part of the MVP.

There will not be a generic:

```text
User A → User B
```

messaging network.

Chat exists because an activity requires coordination.

---

# 28. Chat Lifecycle

## Decision

Chat belongs to the SideQuest.

When the activity is completed/cancelled, the chat should no longer function as an active coordination channel.

It can later become:

* Read-only
* Archived

The final retention behavior will be defined before production launch.

---

# 29. User Profiles

## Decision

Profiles should be lightweight and trust-oriented.

Potential information:

* Name
* Profile photo
* Basic bio
* Verification
* Activities hosted
* Activities completed/joined

---

# 30. Social Profiles

## Decision

SideQuest will not initially build profiles around:

* Followers
* Following
* Likes
* Popularity
* Social posts

---

# 31. Permanent Communities

## Decision

Permanent communities are not part of the MVP.

## Reason

Communities would shift the product toward:

```text
Community
    ↓
Members
    ↓
Events
```

instead of:

```text
Activity
    ↓
Participants
```

---

# 32. Dating

## Decision

Dating functionality is not part of SideQuest.

There will be no:

* Swiping
* Romantic matching
* Match scores
* Dating profiles

People connect through shared activities.

---

# 33. Event Ticketing

## Decision

Event ticketing is not part of the MVP.

SideQuest can represent activities that cost money, but it does not need to become a ticket marketplace.

---

# 34. In-App Payments

## Decision

Not part of MVP.

Costs can be displayed.

Payments happen externally.

---

# 35. Social Feed

## Decision

No generic social feed in MVP.

There should not be an Instagram-style content system.

---

# 36. Followers / Following

## Decision

Not part of MVP.

Users do not need to build a follower graph to use SideQuest.

---

# 37. Gamification

## Decision

Gamification is not part of MVP.

Avoid:

* Points
* Levels
* Badges for engagement
* Leaderboards
* Streaks

unless later research establishes a strong product need.

---

# 38. AI

## Decision

AI is not part of the initial product definition.

Potential future AI features may include:

* Recommendations
* Activity suggestions
* Matching
* Personalization

But these should only be considered after validating the core product.

---

# 39. Recommendation System

## Decision

Do not build complex recommendations initially.

Start with straightforward discovery based on:

* Location
* Time
* Category
* Availability
* Activity status

---

# 40. Trust

## Decision

Trust is a core product requirement.

Initial trust mechanisms include:

* Phone verification
* Basic profiles
* Host control
* Age eligibility
* Participant visibility
* Report
* Block
* Location privacy

---

# 41. Safety

## Decision

Safety must be implemented before beta.

This is especially important because SideQuest facilitates offline interactions between people who may not know each other.

---

# 42. Reports

## Decision

Users can report:

* Users
* SideQuests
* Relevant content

Reports should enter the moderation system.

---

# 43. Blocking

## Decision

Users can block other users.

The final blocking behavior across discovery, activities, participants and chat will be specified during the safety/API design stage.

---

# 44. Activity Cancellation

## Decision

Hosts can cancel their SideQuest.

When cancelled:

* New joins stop.
* Pending requests become invalid.
* Participants are notified.
* Activity leaves active discovery.
* Activity status becomes `CANCELLED`.

---

# 45. Participant Leaving

## Decision

Participants can leave an activity before the relevant deadline/lifecycle point.

The participant slot becomes available again according to the activity rules.

---

# 46. Activity Expiry

## Decision

SideQuests are temporary.

They should automatically leave active discovery after their relevant time has passed.

Possible terminal states:

* Completed
* Expired
* Cancelled

---

# 47. User Roles

## Decision

There is no permanent host/participant account type.

A user can:

* Host one SideQuest
* Join another
* Host another later

Roles are contextual to an activity.

---

# 48. Launch Audience

## Decision

The primary launch audience is:

> **College students + young professionals**

The broader product can eventually serve a wider adult audience.

---

# 49. Launch Age

## Decision

Minimum age:

> **16+**

The initial target audience therefore includes some users aged 16–17.

Safety requirements must account for this.

---

# 50. Initial Launch Location

## Status

Not permanently locked.

The development plan recommends launching in one city first.

Potential example:

```text
Hyderabad
```

The final city should be informed by validation/research.

---

# 51. Initial Activity Categories

## Status

Candidate categories:

* Football
* Trekking
* Watch parties
* Gaming
* Food
* Badminton
* Movies
* Outdoor activities

The final launch category set should come from user research.

---

# 52. Brand Name

## Decision

Keep:

> **SideQuest**

as the current working/internal name.

## Status

Public brand name remains TBD.

Before public launch, investigate:

* Trademark availability
* App-store conflicts
* Domain availability
* Social handles
* Search/SEO conflicts
* Existing products

No public brand commitment should be made until this is checked.

---

# 53. Technical Complexity

## Decision

Do not introduce infrastructure complexity before it is needed.

The initial architecture should favor:

> **Simple modular architecture**

rather than:

* Microservices
* Kubernetes
* Elasticsearch
* GraphQL
* Complex event-driven infrastructure

---

# 54. Performance

## Decision

Performance is a product requirement, not a post-launch optimization.

The application should be designed for:

* Low-end Android
* Mid-range Android
* Recent Android
* Older iPhones
* Recent iPhones
* Slow 4G
* Unstable networks

---

# 55. Visual Design

## Decision

Premium visual quality should come primarily from:

* Typography
* Spacing
* Layout
* Hierarchy
* Icons
* Color
* Interaction design

Avoid using heavy rendering effects to create visual quality.

---

# 56. Empty Marketplace

## Decision

Marketplace liquidity is a critical product problem.

SideQuest has:

```text
Hosts
  ↓
Supply
```

and:

```text
Joiners
  ↓
Demand
```

Both sides must exist.

An empty activity feed is a critical failure mode.

---

# 57. Product Success

The primary early product outcome should be successful real-world activity formation.

Important metric:

> **Completed SideQuests with at least one successful participant join.**

Downloads alone are not sufficient evidence of product success.

---

# 58. Feature Evaluation Rule

Before adding a feature, ask:

1. Does it strengthen the core loop?
2. Does it make activity discovery easier?
3. Does it make joining easier?
4. Does it improve coordination?
5. Does it improve trust/safety?
6. Does it work on low-end devices?
7. Does it work on slow networks?
8. Does it introduce unnecessary complexity?
9. Does it move SideQuest toward a generic social network?

If the feature does not provide a strong reason to exist, it should not be added to the MVP.

---

# 59. Explicitly Rejected Product Direction

SideQuest should not gradually become:

```text
Meetup
    +
Tinder
    +
Instagram
    +
WhatsApp
```

The product should remain:

```text
Activity
    ↓
People
    ↓
Coordination
    ↓
Real-world experience
```

---

# 60. Decision Status System

Future decisions should use one of these statuses:

### LOCKED

Decision has been finalized.

### OPEN

Decision has not yet been finalized.

### HYPOTHESIS

Working assumption requiring validation.

### DEFERRED

Intentionally postponed until a later product stage.

---

# 61. Current Locked Decisions

```text
Product:
Activity-first

Core principle:
"I want to do this. Who else wants to join?"

Primary object:
SideQuest / Activity

Launch audience:
College students + young professionals

Minimum age:
16+

Age eligibility:
Host-defined

Creation:
Any eligible authenticated user

Join modes:
Instant Join + Request to Join

Capacity:
Required

Capacity increase:
Host can increase

Cost:
Optional display, no in-app payment

Visibility:
Public + Unlisted + Invite-only

Location:
Nearby by default + other-city search

Discovery:
List-first

Chat:
Activity-specific and MVP

Profiles:
Lightweight + trust-oriented

Communities:
Not MVP

Dating:
Not MVP

Followers:
Not MVP

Social feed:
Not MVP

Ticketing:
Not MVP

AI:
Not MVP

Public brand:
TBD

Working name:
SideQuest
```

---

# 62. Current Open Decisions

The following decisions still require product/UX validation:

* Exact activity-specific fields
* Exact age selector UI
* Exact notification rules
* Exact chat retention policy
* Exact block behavior
* Exact report categories
* Exact private-location behavior
* Exact invite mechanism
* Exact discovery ranking
* Exact launch city
* Exact initial category set
* Exact onboarding fields
* Final navigation structure
* Final visual design
* Public brand name

---

# 63. Product Drift Test

Whenever the product starts moving toward:

```text
People
Followers
Feeds
Groups
Likes
Messages
```

return to:

```text
What do you want to do?
        ↓
Who else wants to join?
```

That is the product's anchor.

---

# 64. Final Product Decision

SideQuest exists to solve one specific problem:

> **People want to do activities but sometimes don't have enough people to go with.**

The product should make that problem easier to solve.

Everything else is secondary.

```