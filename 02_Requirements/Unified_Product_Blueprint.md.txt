# TimeOut Unified Product Blueprint (v1)

## 1. Product Summary

TimeOut is a mobile app that helps **trusted friends trade babysitting** through a private, invitation-only circle.

It is not a babysitter marketplace. It does not connect strangers. It does not sell childcare services.

TimeOut coordinates sit requests so that a parent can quickly find a trusted sitter, and the circle can maintain reciprocity over time through a simple points system.

The core loop is:

**Invite Friends → Send Sit Request → AutoPing finds sitter → Sit happens → Points transfer → Circle grows stronger**

The long-term goal is not just to coordinate occasional childcare. The deeper goal is to help create **lasting neighborhood circles** that support families for years and possibly decades.

---

## 2. Core Principles

### Trusted friends, not strangers
TimeOut is designed for known, trusted relationships. The default mental model is “friends helping friends.”

### Invitation-only circles
A user joins a circle by being invited by an existing member or by starting a new circle. Users do not browse public groups.

### Low friction for tired parents
A parent should be able to tap the app and quickly request a sitter. The app should minimize typing, screens, and decision fatigue.

### Reciprocity without over-formality
Points help the system remain sustainable, but the emotional framing is still informal friendship and mutual support.

### Most sits are drop-off
The system assumes that most sits happen by dropping children off at the sitter’s home. Night sits at the requester’s home are rarer.

### Geography matters
TimeOut works best when members live near one another. Sits become less practical as drive times increase, especially beyond about 15 minutes.

### Meetings and social connection matter
Circles thrive when members occasionally gather, socialize, discuss norms, and recruit thoughtfully. The app can support this lightly.

---

## 3. Product Positioning

TimeOut is designed primarily for **parents of preschool and young children** who already know a handful of other parents they trust.

The main value proposition is not “cheap childcare.” It is:

- less stress
- easier sit coordination
- trusted sitters
- relief from calling around
- a reliable way to enjoy occasional TimeOut

Key emotional promise:

**Imagine date nights again. Sanity is near.**

Key functional promise:

**One tap AutoPing finds the first sitter who says yes.**

---

## 4. The Circle Model

A TimeOut circle is a private group of trusted parents who can request and provide sits for one another.

### Ideal size
- **8–12 active families** generally provides dependable sit coverage
- **14–20 members** may support long-term neighborhood continuity
- smaller circles can start with 3–5 members and grow gradually

### Growth
Circles should grow thoughtfully. As circles mature, members may want more intentional recruiting and screening.

### Geography
Tight geographic clustering helps circles work. The system may store a light location hint to assess circle spread and help identify the circle’s rough center.

### Native contacts
Members should be encouraged to save each other in their phone contacts for emergencies, directions, and off-app coordination.

---

## 5. Identity and Access

### No passwords in v1
TimeOut should not require traditional username/password registration for v1.

### Phone-number identity
The user’s phone number is the primary identity anchor.

Recommended flow:
1. user enters phone number
2. system sends a one-time verification code
3. user verifies code
4. device session is remembered

### Why phone identity is sufficient
- circles are private and invite-only
- no payments are processed in-app
- little sensitive data is stored
- phone numbers are already central to invitations and sit requests

---

## 6. Circle Formation and Invite Logic

### Invite versus Share
TimeOut must maintain a clear distinction between:

- **Invite Friends** = invite a nearby friend to join this circle
- **Share App** = send the app to a non-local friend so they can start another circle elsewhere

### Friend-origin invitations
Invites should come from the member’s own phone context so they feel personal and trusted, not like spam.

### Primary invite mechanism
The system should match invitees to circles using the **invitee phone number** selected by the inviter.

### Secondary fallback
A join code or join link may exist as a fallback, but phone-number matching is the primary mechanism.

### Invite resolution rules
When a user installs the app and verifies their phone number:

- if there are no valid invites → allow new circle creation
- if all valid invites point to one circle → auto-associate with that circle
- if multiple valid invites point to different circles → show a **Choose Your Circle** screen

### Duplicate invites to same circle
If multiple members of the same circle invite the same person, these should collapse into one circle context.

### Accidental second local circle
If a user has a valid invite to a nearby circle and tries to start a new one, the app should warn them and encourage joining the existing circle to avoid confusion.

---

## 7. Onboarding and Education

### Purpose
The first-run journey must explain the concept quickly and build confidence before the user invites friends or sends a sit request.

### Format
A short swipe-story onboarding sequence is preferred over long paragraphs.

### Core onboarding themes
- trusted friends trade babysitting
- AutoPing makes the process easy
- friends want TimeOut too
- kids benefit from playmates
- circles are private
- no spam
- free to start
- the user is signing up for TimeOut, not extra work

### Optional quick demo
A lightweight interactive demo may show the custom sit request flow and preset autofill behavior, but it should not simulate a fake AutoPing conversation in detail.

---

## 8. Primary Home Screen

### Title
**My TimeOut**

### Purpose
When a parent opens the app, they usually want to request a sitter quickly. The primary screen should therefore be a sit-request launcher, not a generic dashboard.

### Primary actions on My TimeOut
- Create Custom Sit Request
- Emergency Daycare Pickup
- Playdate
- Friday Date Night
- Saturday Date Night
- Saturday Brunch
- Sunday Project
- Invite Friends

### Notes
Official TimeOut gatherings are not launched from a preset button. They are system-generated on schedule.

---

## 9. Sit Request Model

### Core sit fields
Every sit request should support:
- date
- start time
- duration
- number of kids
- location type
- optional comments

### Time entry
The custom sit request should preserve the hand-sketched date/time entry intent:
- quick date selection
- quick start-time matrix
- quick duration matrix
- minimal scrolling

### Location types
- **Drop-off** (default)
- **My Place** (rarer, usually night/date sit)

### Comments
Comments are flexible, lightweight, and useful for context. They should not turn into a long form.

---

## 10. Presets

Presets are important for:
- reducing friction
- marketing
- teaching common use cases

Examples include:
- Friday Date Night
- Saturday Date Night
- Saturday Brunch
- Sunday Project
- Playdate
- Emergency Daycare Pickup

Presets should simply prefill the sit request form and then allow the user to review or edit before sending.

---

## 11. Emergency Daycare Pickup

### Why it matters
This preset expands relevance to working parents and communicates a strong real-world value proposition.

### Behavior
Emergency Daycare Pickup is a special sit request type with these defaults:
- urgent timing (pickup within about an hour)
- reviewed broadcast ping to the selected list
- +6 bonus incentive in addition to normal sit points

### Requester review
The requester should still review the selected list before sending. The app may default to all selected, but the requester can intentionally exclude someone.

### Daycare location
Do not require a formal address field. Use a prompted comments field such as:

**Daycare location or pickup details**  
Example: *Kindercare on Martin Way*

### Reminder
The requester should be reminded to ensure the daycare knows which sitter is authorized to pick up the child.

---

## 12. Candidate List and Ping Ethics

### Default list
The default sitter list should be:
- comprehensive
- sorted by lowest point balance / greatest debt first

This supports fairness and helps those who most need points get opportunities to earn them.

### Requester control
Safety and requester comfort come first, so the requester may exclude names intentionally.

### Product ethic
The interface should not make casual exclusion too easy, because that can weaken the fairness mechanism and reduce sit opportunities for high-debt members. This ethic belongs partly in education and agenda discussions.

---

## 13. AutoPing

### Purpose
AutoPing is the core mechanic that finds the first sitter without requiring the requester to call around.

### v1 response model
**YES or ignore**  
No response is a normal and acceptable outcome. A prominent NO is not necessary in v1.

### Normal mode
For regular sit requests:
- send pings sequentially
- wait the interval
- continue down the list
- earlier pings remain active
- first valid YES wins

### Emergency mode
For Emergency Daycare Pickup:
- send to the reviewed selected list immediately
- first valid YES wins

### First YES wins
Only one sitter may be confirmed. Later YES responses receive a polite notice that a sitter has already been found.

---

## 14. Sit Lifecycle

The core sit states are:

- draft
- ready
- ping_pending
- confirming_yes
- confirmed
- in_progress
- ending
- posted
- cancelled
- timed_out

### Cancel-and-recreate rule
In v1, do not support complex in-place edits to a confirmed sit.

If important details change, the user should:
1. cancel the sit
2. create a new sit request
3. optionally narrow the new sit request to the intended sitter

### Why
This is simpler for users and much easier to implement reliably than supporting many intermediate edit paths.

### Corrections after settlement
If the sit details differ from reality after posting, members can correct through **Transfer Points** rather than editing the sit.

---

## 15. Notifications and Messaging

### Channel priority
- if the user has the app → push notification
- if the user does not have the app → SMS

### Message-first philosophy
For fast sit coordination, messages are often more practical than expecting the user to open the app.

### Screen-first philosophy
The app screens are better for:
- composing requests
- reviewing lists
- editing settings
- viewing ledger/history
- onboarding and guidance

### Sit request messages
Sit request notices should be short and glanceable. Example:

**Sara sent you a TimeOut sit request.**  
**Friday 7:00–10:00 PM**  
**2 kids – Drop-off**  
**Reply YES if you can help.**

### Sit reminders and confirmations
Most sit confirmations and reminders should include:
- requester name
- sitter name
- sit context
- cancel option for both parties where appropriate

### Every non-app SMS
Each non-app message should include the app link so the invitee can install when ready.

---

## 16. Points Economy

### Purpose
Points help maintain reciprocity without turning the system into a formal market.

### v1 point rules
- 4 points per hour
- +4 points for 2 or more kids
- +4 points if the sitter travels to the requester’s home
- +6 points for Emergency Daycare Pickup

### Starting balance
Each member starts at **0 points**.

### Tank metaphor
The visual balance graphic may still show “half full” if the display scale is centered around zero, such as -80 to +80.

### Posted sits are immutable
Once points are posted for a sit, the sit is not edited. Corrections happen through Transfer Points.

### Balance restoration
The system should eventually support **Adjustments to Balance Sequestered Points** when members leave with positive or negative balances and take points out of circulation.

---

## 17. Gatherings and Playdates

### Official TimeOut Gatherings
These are not preset buttons.

They are system-generated according to the known rule:
- third Monday of odd months
- 8:00 PM

The system should:
1. request a host
2. request a rotating agenda person / agenda lead
3. broadcast the gathering notice to the whole circle
4. collect RSVPs

### Why gatherings matter
Meetings reinforce:
- friendship
- trust
- recruit screening
- emergency readiness
- circle culture

### Playdates
Playdates are opportunistic and weather-dependent, so they should be user-initiated and available as a preset.

A playdate request should support:
- date/time
- park/location
- optional comment
- optional co-supervisor
- broadcast RSVP mode

Multiple YES responses are expected.

---

## 18. Delegation / Spouses / Accompanying Adults

### Legacy default
The legacy co-op generally did not allow silent delegation or a spouse to substitute automatically.

### v1 product position
Do not overbuild this in v1, but support **disclosure**.

At sit confirmation, allow a lightweight disclosure such as:
- confirmed member only
- confirmed member + accompanying adult
- different attending adult disclosed below

This preserves transparency while allowing real-world flexibility. Circle norms can govern stricter expectations.

---

## 19. Circle Vital Signs

### Purpose
Circle Vital Signs is the umbrella concept for:
- monitoring
- nudges
- incentives
- startup guidance
- group health awareness

### Member Vital Signs
Useful indicators may include:
- last sit or request
- activity in the last 14 days
- point balance band
- invite activity
- likely honorary transition

### Circle Vital Signs
Useful indicators may include:
- weekly transaction rate
- active member count
- median time to ping success
- geographic spread hint
- point circulation health

### Early thresholds (provisional)
These are working assumptions and may be adjusted after real usage:
- healthy member activity: about one sit or request every 14 days
- healthy circle activity: about one transaction per week
- dependable sit coverage: 8–12 active members

### Nudges
The system may later trigger helpful nudges such as:
- invite a few nearby friends
- take a sit if you have plenty of credit
- share the app with a far-away friend
- prepare for a gathering
- recruit a replacement member

---

## 20. Governance, Agenda, and Standard Practice

### Why this matters
Many important group norms are better handled through conversation and standard practice than by hard-coding them into the app.

### Agenda themes
Useful meeting agenda themes include:
- recruiting thoughtfully
- safety and childproofing
- emergency contact readiness
- daycare pickup readiness
- fairness and sit opportunities
- honorary transitions
- replacement recruiting
- waiting list practices

### Recruit screening
As circles mature, suggested practices may include:
- sponsor introduces recruit
- discussion before invite
- optional home visit or safety check
- waiting list or deferment if the circle is full

### Waiting list
A circle may want language for interested recruits when the group is not ready to grow yet.

---

## 21. Legal, Trust, and Privacy

### Terms of Use
The app coordinates trusted-friend sits. It does not provide childcare services and does not guarantee childcare outcomes.

### Privacy
The app should collect only the minimum necessary information:
- phone number
- display name
- circle membership
- point and activity records
- light location hint if used

### Contact sharing
Users should understand that their contact information may be used for sit coordination within the circle.

### Native contact saving
The app should encourage members to save sitter contacts in their phones for emergencies and off-app coordination.

### SMS opt-out
Non-app users should have a path to stop repeated messages if they do not wish to participate.

---

## 22. Launch Strategy

### Quiet launch first
The best first launch is not broad marketing.

Instead:
- launch quietly
- let a few known circles use the app
- prove the system works
- refine based on real behavior
- market more broadly later

This is not a separate “pilot version.” It is a real v1 used with a small initial audience.

---

## 23. v1 Scope Summary

### Must-have
- phone identity
- circle creation and invites
- invite resolution by phone number
- onboarding
- My TimeOut screen
- custom sit request
- presets
- AutoPing
- sit confirmation and reminders
- end sit
- points ledger
- emergency daycare pickup

### Included in v1 if lightweight
- playdate broadcast
- official gathering host / agenda lead / RSVP flow

### Deferred or secondary
- advanced analytics dashboards
- complex multi-circle UI
- formal voting / bylaws engine
- spouse-account system
- geolocation-based sit ending
- heavy profile collection

---

## 24. Product Design Philosophy

The app should feel like:

**Tap icon → request a sitter**

not:

**Tap icon → manage a complicated system**

That means:
- speed matters
- clarity matters
- trust matters
- friction matters
- social mechanics matter

TimeOut succeeds when the software stays simple and the circle becomes stronger.
