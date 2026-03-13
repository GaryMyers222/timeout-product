\# Developer Implementation Blueprint



This document translates the TimeOut product design into a buildable technical system.



It is intended to help developers implement the app without having to infer the behavior from scattered notes.



The system is organized around a small set of engines and shared data structures.



\---



\## 1. System Overview



TimeOut can be implemented as six core engines:



1\. Identity and Circle Engine

2\. Sit Request Engine

3\. AutoPing and Broadcast Engine

4\. Notification Engine

5\. Points and Ledger Engine

6\. Circle Vital Signs Engine



These engines operate on a shared set of entities.



The core product loop is:



Invite Friends → Create Sit Request → AutoPing finds sitter → Sit happens → Points transfer → Circle grows stronger



\---



\## 2. Core Entities



\### users



Represents a real person identified primarily by phone number.



Suggested fields:



\- user\_id

\- phone\_number

\- display\_name

\- email\_optional

\- app\_installed\_boolean

\- push\_enabled\_boolean

\- location\_hint\_optional

\- status\_active\_boolean

\- created\_at

\- updated\_at



Notes:



\- phone\_number should be unique

\- no password field is required for v1

\- location\_hint should be light-touch, not a precise address



\---



\### circles



Represents a private TimeOut circle.



Suggested fields:



\- circle\_id

\- circle\_name

\- circle\_name\_auto\_generated

\- created\_by\_user\_id

\- location\_center\_hint\_optional

\- is\_active

\- created\_at

\- updated\_at



Notes:



\- circle\_name may be user-defined or system-suggested

\- location center may later be computed from member location hints



\---



\### circle\_memberships



Represents a user’s membership inside a specific circle.



Suggested fields:



\- membership\_id

\- circle\_id

\- user\_id

\- points\_balance

\- member\_role

\- membership\_status

\- joined\_at

\- last\_request\_at\_optional

\- last\_sit\_at\_optional

\- is\_honorary\_boolean



Suggested enums:



\- member\_role: member, host\_optional, agenda\_lead\_optional

\- membership\_status: invited, active, inactive, honorary, removed



Notes:



\- balance must be circle-specific

\- users may belong to more than one circle, though this is rare



\---



\### invites



Represents an invitation record.



Suggested fields:



\- invite\_id

\- invite\_type

\- circle\_id\_optional

\- circle\_name\_snapshot\_optional

\- inviter\_user\_id

\- invitee\_phone

\- state

\- sent\_at

\- installed\_at\_optional

\- resolved\_at\_optional

\- obsolete\_reason\_optional



Suggested enums:



\- invite\_type: join, share

\- state: draft, sent, waiting, installed\_unresolved, joined, shared\_new\_circle, conflict, obsolete



Notes:



\- invites are primarily resolved by phone number

\- join codes may exist as a fallback only



\---



\### sit\_requests



Represents a childcare request.



Suggested fields:



\- sit\_request\_id

\- circle\_id

\- requester\_user\_id

\- request\_type

\- preset\_name\_optional

\- scheduled\_date

\- start\_time

\- duration\_minutes

\- estimated\_end\_time

\- num\_children

\- location\_type

\- comment\_text\_optional

\- status

\- confirmed\_sitter\_user\_id\_optional

\- created\_at

\- updated\_at

\- cancelled\_by\_user\_id\_optional

\- cancelled\_at\_optional



Suggested enums:



\- request\_type: custom, preset, emergency\_daycare

\- location\_type: drop\_off, my\_place

\- status: draft, ready, ping\_pending, confirming\_yes, confirmed, cancelled, timed\_out, in\_progress, ending, posted



Notes:



\- most sits are drop\_off

\- app should not depend on publishing requester home address



\---



\### sit\_candidate\_selections



Represents the default and edited sitter list for a sit request.



Suggested fields:



\- selection\_id

\- sit\_request\_id

\- candidate\_user\_id

\- default\_order\_rank

\- is\_selected

\- is\_excluded\_by\_requester

\- selection\_source



Suggested enums:



\- selection\_source: default\_circle\_list, manual\_add, emergency\_default\_all



Notes:



\- the default list should be comprehensive

\- ranking should reflect lowest points / greatest debt first

\- exclusions should be intentional



\---



\### ping\_attempts



Represents each ping notification sent during AutoPing or a broadcast.



Suggested fields:



\- ping\_attempt\_id

\- sit\_request\_id

\- target\_user\_id\_optional

\- target\_phone

\- mode

\- sequence\_order\_optional

\- sent\_via

\- sent\_at

\- state

\- response\_received\_at\_optional

\- response\_value\_optional



Suggested enums:



\- mode: sequential, broadcast

\- sent\_via: sms, push

\- state: sent, responded\_yes, no\_response, superseded, expired

\- response\_value\_optional: yes



Notes:



\- v1 uses YES-or-ignore

\- no\_response is a system interpretation, not a user action



\---



\### ledger\_transactions



Represents all point movements.



Suggested fields:



\- transaction\_id

\- circle\_id

\- transaction\_type

\- short\_description

\- requester\_user\_id\_optional

\- sitter\_user\_id\_optional

\- related\_sit\_request\_id\_optional

\- points\_delta\_requester\_optional

\- points\_delta\_sitter\_optional

\- points\_amount

\- created\_at

\- created\_by\_system\_boolean

\- comment\_optional



Suggested enums:



\- transaction\_type: sit, manual\_transfer, meeting\_host, playdate\_host\_optional, emergency\_bonus, adjustment\_sequestered\_points



Notes:



\- circle\_id should always be present

\- short\_description is important for readable history



\---



\### gatherings



Represents official TimeOut gatherings.



Suggested fields:



\- gathering\_id

\- circle\_id

\- scheduled\_datetime

\- host\_user\_id\_optional

\- agenda\_lead\_user\_id\_optional

\- status

\- created\_at

\- updated\_at



Suggested enums:



\- status: scheduled, host\_pending, agenda\_pending, open\_rsvp, completed, cancelled



Notes:



\- gatherings are system-generated, not presets

\- official schedule: third Monday of odd months at 8:00 PM



\---



\### gathering\_rsvps



Represents RSVP responses for gatherings.



Suggested fields:



\- rsvp\_id

\- gathering\_id

\- user\_id

\- response\_value

\- responded\_at



Suggested enums:



\- response\_value: yes



Notes:



\- for v1, silence means not attending



\---



\### playdates



Represents a user-initiated playdate event.



Suggested fields:



\- playdate\_id

\- circle\_id

\- initiator\_user\_id

\- co\_supervisor\_user\_id\_optional

\- scheduled\_datetime

\- location\_text

\- comment\_optional

\- status

\- created\_at



Suggested enums:



\- status: draft, open\_rsvp, completed, cancelled



\---



\### playdate\_rsvps



Represents RSVPs to playdates.



Suggested fields:



\- playdate\_rsvp\_id

\- playdate\_id

\- user\_id

\- response\_value

\- responded\_at



Suggested enums:



\- response\_value: yes



\---



\### vital\_sign\_snapshots



Represents computed circle-level health data.



Suggested fields:



\- snapshot\_id

\- circle\_id

\- snapshot\_type

\- active\_member\_count

\- weekly\_transaction\_rate

\- median\_time\_to\_ping\_success\_optional

\- geographic\_spread\_hint\_optional

\- point\_circulation\_flag\_optional

\- created\_at



Suggested enums:



\- snapshot\_type: circle, startup



\---



\### member\_vital\_signs



Represents computed member-level activity signals.



Suggested fields:



\- member\_vital\_sign\_id

\- circle\_id

\- user\_id

\- activity\_last\_14d

\- activity\_last\_60d

\- balance\_band

\- likely\_honorary\_flag

\- invite\_activity\_count

\- computed\_at



\---



\## 3. Authentication and Identity Flow



\### v1 authentication model



Use phone-number verification with one-time SMS code.



Recommended flow:



1\. user enters phone number

2\. system sends verification code

3\. user enters code

4\. session is established on device



No password is required.



\### Why this is sufficient



\- circles are private and invitation-only

\- no payment transactions are handled in-app

\- very little sensitive data is stored

\- phone number is already the natural identity anchor



\---



\## 4. Invite Resolution Flow



After phone verification, the system checks invites by phone number.



Resolution rules:



\- if there are no invites, allow circle creation

\- if all valid invites point to one circle, auto-associate with that circle

\- if multiple valid invites point to different circles, show Choose Your Circle



Additional rule:



If a user with a valid local invite attempts to create a new local circle, show a protective warning before allowing it.



Join codes or links may exist only as a fallback.



\---



\## 5. Sit Request Engine Logic



The primary entry screen is \*\*My TimeOut\*\*.



Main actions:



\- Create Custom Sit Request

\- Emergency Daycare Pickup

\- Playdate

\- Friday Date Night

\- Saturday Date Night

\- Saturday Brunch

\- Sunday Project

\- Invite Friends



\### custom sit request



Fields:



\- date

\- start time

\- duration

\- number of kids

\- location type

\- comments



\### preset behavior



Presets simply prefill the sit request form, then allow review/edit before send.



\### emergency daycare behavior



Defaults:



\- near-immediate timing

\- reviewed broadcast to selected list

\- prompted daycare location in comments

\- +6 bonus points



\---



\## 6. Candidate List Logic



When a sit request is created, the system builds a default sitter list.



Rules:



\- list should be comprehensive

\- order by lowest points / greatest debt first

\- requester may intentionally exclude names

\- fairness-first remains the default ethic



UI should not make casual exclusion too easy.



\---



\## 7. AutoPing Algorithm



\### Normal sequential mode



For regular sit requests:



1\. build default sitter list

2\. send first ping

3\. wait ping interval

4\. if no valid YES, send next ping

5\. keep earlier pings active

6\. first valid YES wins



\### YES-or-ignore model



Users may:



\- reply YES

\- ignore the request



NO is not required in v1.



Silence becomes no\_response in the system.



\### first YES wins



When a YES arrives:



1\. verify sit request is still open

2\. verify no sitter already confirmed

3\. lock the confirmed sitter

4\. move sit request to confirmed state



If a later YES arrives, send a polite message that a sitter has already been found.



\### emergency daycare broadcast



Emergency pickup uses broadcast delivery but still follows first-YES-wins logic.



\---



\## 8. Broadcast RSVP Engine



A separate but related engine handles events with multiple expected YES responses.



\### gathering host request



System asks:

\- who will host the next gathering?

\- who will lead the agenda?



First YES fills each role.



\### gathering RSVP



Once host and agenda lead are set, system broadcasts the gathering announcement and collects YES responses.



\### playdate RSVP



Playdates are user-initiated and use broadcast RSVP mode.



Multiple YES responses are expected.



\### emergency distinction



Emergency daycare is broadcast delivery, but not RSVP logic. It still uses first-YES-wins.



\---



\## 9. Sit Lifecycle Logic



Core sit states:



\- draft

\- ready

\- ping\_pending

\- confirming\_yes

\- confirmed

\- in\_progress

\- ending

\- posted

\- cancelled

\- timed\_out



\### cancel-and-recreate rule



v1 does not support complex editing of confirmed sits.



If core details change:



1\. cancel sit

2\. create new sit request

3\. optionally narrow list to intended sitter



\### why



This reduces system complexity, avoids notification confusion, and keeps the workflow understandable for users.



\### correction after posting



After points are posted, do not edit the sit. Use manual transfer points for adjustments.



\---



\## 10. Notification Workflows



\### channel priority



\- app user → push notification

\- non-app user → SMS



All non-app SMS messages should include app link.



\### message-first coordination



Use fast, glanceable messages for:

\- invites

\- sit requests

\- confirmations

\- reminders

\- emergency requests



\### screen-first coordination



Use app screens for:

\- composing requests

\- reviewing candidate list

\- point history

\- settings

\- onboarding

\- agenda and guidance content



\### reminder schedule



After confirmation, send reminders at:

\- 24 hours before

\- 1 hour before

\- sit start time



\### naming rule



Sit confirmations and reminders should include both:

\- requester name

\- sitter name



\### cancel option



Most sit-related confirmations and reminders should support Cancel Sit for both parties.



\---



\## 11. Points Engine Rules



\### base formula



\- 4 points per hour

\- +4 if 2 or more children

\- +4 if sitter travels to requester home

\- +6 emergency daycare bonus



\### starting balance



All members begin at 0 points.



\### display



The tank-style balance display may still appear half-full if centered around zero on a negative-to-positive scale.



\### posted sits are immutable



Once points are posted:

\- do not edit sit

\- use manual transfer for corrections



\### sequestered points



The ledger should support a transaction type for:



\*\*Adjustment to Balance Sequestered Points\*\*



This is needed when members leave the circle with non-zero balances.



\---



\## 12. Gatherings and Playdates



\### official gatherings



System-generated on known rule:

\- third Monday of odd months

\- 8:00 PM



System flow:

1\. request host

2\. request agenda lead

3\. all-ping gathering notice

4\. collect RSVPs



\### playdates



User-initiated broadcast events.



Suggested fields:

\- date/time

\- park/location

\- optional comment

\- optional co-supervisor



\---



\## 13. Delegation / Disclosure



Do not build a full spouse-account or delegation model in v1.



Instead, support lightweight disclosure at confirmation:



\*\*Who will attend the sit?\*\*

\- confirmed member only

\- confirmed member plus accompanying adult

\- different attending adult disclosed below



This supports transparency without overcomplicating the system.



\---



\## 14. Circle Vital Signs Architecture



Circle Vital Signs is the umbrella concept for monitoring and nudges.



\### circle-level signals



Examples:

\- weekly transaction rate

\- active member count

\- median ping success time

\- geographic spread hint

\- point circulation health



\### member-level signals



Examples:

\- last sit or request

\- activity within last 14 days

\- high positive balance

\- likely honorary status

\- invite activity



\### provisional thresholds



Early working assumptions:

\- healthy member activity ≈ one sit or request every 14 days

\- healthy circle activity ≈ one transaction per week

\- dependable coverage ≈ 8–12 active members



\### outputs



The system may later trigger:

\- invite more nearby friends

\- take a sit if you have high credit

\- share app to a distant friend

\- prepare for gathering

\- recruit replacement member



\---



\## 15. Legal / Trust / Privacy Workstream



This is a separate but required workstream.



Needed documents and rules:



\- Terms of Use

\- Privacy Policy

\- SMS consent language

\- contact-sharing consent

\- disclaimer that TimeOut coordinates trusted-friend sits but does not provide childcare services

\- opt-out for non-app SMS recipients



\### data minimization



Store only what is necessary:



\- phone number

\- display name

\- circle membership

\- point and activity records

\- light location hint if used



Avoid requiring precise addresses for normal operation.



\---



\## 16. Recommended Implementation Order



\### phase 1



\- phone verification

\- circle creation

\- invite flow

\- invite resolution by phone number

\- My TimeOut screen



\### phase 2



\- sit draft creation

\- candidate list ordering

\- sequential AutoPing

\- YES handling

\- confirmation flow



\### phase 3



\- reminders

\- end sit

\- points posting

\- ledger



\### phase 4



\- emergency daycare

\- gatherings

\- playdates

\- basic Circle Vital Signs



\---



\## 17. Engineering Guardrails



1\. Do not overbuild account security in v1; phone auth is enough.

2\. Do not support complex sit edits; use cancel-and-recreate.

3\. Do not require NO responses; use YES-or-ignore.

4\. Do not expose a full address directory by default.

5\. Do not let requester casually defeat fairness; keep comprehensive default list.

6\. Do not silently merge different circles; keep circle boundaries clear.

7\. Do not let share invites behave like join invites.



\---



\## 18. Development Outcome



If implemented correctly, the system should support:



\- private circle formation

\- fast sitter coordination

\- clear sit lifecycle

\- balanced reciprocity

\- lightweight group governance

\- long-term neighborhood continuity



The app should feel simple for the parent, even though the underlying system is carefully structured.

