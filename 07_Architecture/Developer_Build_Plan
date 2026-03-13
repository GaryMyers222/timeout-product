# Developer Build Plan

This document translates the TimeOut product architecture into a practical development sequence.

The goal is to build the smallest complete system that allows a real circle to:

Invite Friends → Send Sit Request → AutoPing finds sitter → Sit happens → Points transfer

The build plan is organized in phases so the app can become usable early and expand without rework.

---

## Phase 0 — Setup and Project Foundations

### Objectives

- create a clean codebase
- configure version control
- choose the technical stack
- establish environments for local development and deployment

### Tasks

1. Create the app code repository (`timeout-app`) if it does not already exist.
2. Choose the app framework.
   - Recommended: React Native with Expo, or another mobile-first stack the team can move quickly in.
3. Choose the backend.
   - Firebase is a strong fit for v1 because it supports:
     - phone authentication
     - real-time database / Firestore
     - cloud functions
     - push notifications
4. Set up environments:
   - local development
   - staging / test
   - production later
5. Add a `.gitignore`
6. Add basic project README
7. Create app folder structure
8. Confirm coding conventions

### Deliverables

- working code repository
- app launches locally
- backend project created
- basic deployment path known

---

## Phase 1 — Phone Authentication and User Identity

### Objectives

- identify users by phone number
- avoid passwords
- establish persistent device sessions

### Tasks

1. Implement phone-number sign-in.
2. Send one-time verification code.
3. Verify code and create user record.
4. Store:
   - phone number
   - display name
   - app installed flag
5. Preserve login session on device.
6. Add minimal sign-in UI.

### Rules

- no password flow in v1
- phone number is the primary identity anchor

### Deliverables

- user can install app
- user can verify phone number
- user record is created in backend

---

## Phase 2 — Circle Creation and Invite Resolution

### Objectives

- let the first user create a circle
- let invited users join the correct circle
- prevent accidental circle fragmentation

### Tasks

1. Create circle record when appropriate.
2. Build invite service:
   - invite_type = join
   - invite_type = share
3. Store invite records by invitee phone number.
4. Build invite resolution flow on install:
   - no invites → user may create circle
   - invites to one circle → auto-associate
   - invites to multiple circles → choose circle
5. Build Choose Your Circle screen.
6. Add protective warning if user with valid invite tries to create a different local circle.
7. Support duplicate invites to the same circle as one circle context.

### Deliverables

- first user can create circle
- inviter can send friend-origin invites
- invitee joins the correct circle after phone verification

---

## Phase 3 — My TimeOut Screen and Sit Request UI

### Objectives

- make the app immediately useful
- preserve the fast request flow
- implement the hand-sketched sit request intent

### Tasks

1. Build **My TimeOut** home screen.
2. Add main actions:
   - Create Custom Sit Request
   - Emergency Daycare Pickup
   - Playdate
   - Friday Date Night
   - Saturday Date Night
   - Saturday Brunch
   - Sunday Project
   - Invite Friends
3. Build custom sit request form with:
   - date
   - start time
   - duration
   - number of kids
   - location type
   - comments
4. Build preset autofill behavior.
5. Build Emergency Daycare Pickup special form behavior.
6. Build playdate form.
7. Save sit request drafts.

### Deliverables

- user can open app and create a sit request draft
- presets reduce typing
- emergency and playdate flows exist

---

## Phase 4 — Circle Membership and Candidate List Logic

### Objectives

- show circle members
- generate the default sitter list correctly
- preserve fairness

### Tasks

1. Build circle membership records.
2. Build candidate selection list for sit requests.
3. Sort candidates by:
   - lowest points balance first
4. Allow intentional exclusions.
5. Keep default list comprehensive.
6. Add Invite Friends access from relevant screens.
7. Support light location hint storage for future Circle Vital Signs.

### Deliverables

- system can build a sitter candidate list
- requester can review list before sending

---

## Phase 5 — AutoPing Engine

### Objectives

- make the circle operational
- find the first sitter quickly
- handle YES responses correctly

### Tasks

1. Implement sit request states:
   - draft
   - ready
   - ping_pending
   - confirming_yes
   - confirmed
   - cancelled
   - timed_out
   - in_progress
   - ending
   - posted
2. Implement sequential AutoPing:
   - send first ping
   - wait interval
   - send next ping
3. Keep earlier pings active.
4. Implement YES-or-ignore model.
5. Lock the first valid YES.
6. Handle race conditions.
7. Stop sending new pings once confirmed.
8. Mark no-response attempts internally.
9. Build emergency broadcast mode:
   - send to selected list
   - first YES still wins

### Deliverables

- real sit requests can find a sitter
- first valid YES wins reliably
- emergency requests broadcast correctly

---

## Phase 6 — Notification System

### Objectives

- make the system usable from the phone
- support app and non-app users
- preserve the message-first workflow

### Tasks

1. Implement channel logic:
   - app user → push
   - non-app user → SMS
2. Build sit request notifications.
3. Build sit confirmation notifications.
4. Build reminder notifications:
   - 24 hours before
   - 1 hour before
   - sit start
5. Ensure confirmations/reminders include:
   - requester name
   - sitter name
6. Add Cancel Sit action where appropriate.
7. Include app link in all non-app SMS.
8. Implement emergency pickup message.
9. Implement late responder “sitter already found” message.

### Deliverables

- users receive sit requests correctly
- sit lifecycle is understandable through messages
- non-app users can still participate

---

## Phase 7 — Sit Completion and Ledger

### Objectives

- settle sits cleanly
- post points
- keep history understandable

### Tasks

1. Implement End Sit action for both parties.
2. Compute actual or rounded duration.
3. Apply point rules:
   - 4 points per hour
   - +4 for 2+ children
   - +4 for sitter travel
   - +6 emergency bonus
4. Post ledger transaction.
5. Build points history screen.
6. Add short descriptions to transactions.
7. Implement manual Transfer Points.
8. Add transaction type for:
   - Adjustment to Balance Sequestered Points

### Deliverables

- completed sits post points correctly
- history is readable
- manual adjustments possible

---

## Phase 8 — Gatherings and Playdates

### Objectives

- support circle culture in v1
- add lightweight RSVP coordination

### Tasks

1. Implement official gathering schedule logic:
   - third Monday of odd months
   - 8:00 PM
2. Build host request flow.
3. Build agenda lead request flow.
4. Build gathering RSVP broadcast.
5. Build playdate broadcast RSVP.
6. Allow multiple YES responses for broadcast RSVP events.
7. Build co-supervisor field for playdates.

### Deliverables

- system-generated gatherings work
- playdate broadcasts work
- meetings and playdates support culture and marketing value

---

## Phase 9 — Circle Vital Signs (Basic)

### Objectives

- observe whether circles are functioning
- support future nudges
- avoid overbuilding analytics too early

### Tasks

1. Compute basic member activity:
   - sit/request in last 14 days
2. Compute basic circle activity:
   - transactions per week
   - active member count
3. Compute geographic spread hint if location hints exist.
4. Store Circle Vital Signs snapshots.
5. Build simple internal reporting or admin visibility.

### Deliverables

- system can detect low activity
- system can later support nudges intelligently

---

## Phase 10 — v1 QA and Quiet Launch

### Objectives

- prove the system with real circles
- refine without broad marketing
- identify friction before growth

### Tasks

1. Test onboarding and invite resolution.
2. Test sit request flow.
3. Test AutoPing race conditions.
4. Test emergency pickup.
5. Test reminders and cancellations.
6. Test point posting and manual transfer.
7. Test gathering and playdate RSVP.
8. Observe real user behavior in a few known circles.
9. Fix friction and release improvements before broad marketing.

### Deliverables

- stable v1 used by real circles
- post-launch learning captured
- ready for wider release later

---

## Recommended Screen Build Order

1. Phone verification
2. Choose Your Circle / circle creation
3. My TimeOut
4. Custom Sit Request
5. Candidate List / Ping Order
6. Ping Status
7. Sit Confirmation
8. End Sit
9. Points Ledger
10. Playdate
11. Gathering host / RSVP

---

## Recommended Backend Build Order

1. users
2. circles
3. circle_memberships
4. invites
5. sit_requests
6. sit_candidate_selections
7. ping_attempts
8. ledger_transactions
9. gatherings
10. playdates
11. vital_sign_snapshots
12. member_vital_signs

---

## Engineering Priorities

### Highest priority

- invite flow
- sit request flow
- AutoPing
- messaging
- point posting

### Important but secondary

- playdates
- gatherings
- Circle Vital Signs

### Deferred

- advanced dashboards
- multiple-circle UX polish
- formal governance tools
- spouse account system
- geolocation sit ending

---

## Core Guardrails

1. Do not add password complexity in v1.
2. Do not support complex edit-in-place sit changes.
3. Do not require NO responses.
4. Do not expose private address directories.
5. Do not weaken the fairness-first default ping list.
6. Do not collapse different circles together accidentally.
7. Do not turn the app into a marketplace.

---

## Definition of v1 Success

Version 1 is successful when a real circle can:

- invite friends
- send a sit request
- get a sitter through AutoPing
- complete the sit
- post the points
- repeat the cycle with confidence

The system does not need to solve every edge case at launch. It needs to make the core circle loop work reliably.