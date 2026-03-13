\# Notification Matrix



The Notification Matrix defines how TimeOut communicates sit requests, confirmations, reminders, and broadcast events to circle members.



TimeOut uses two communication channels.



\## Channel Rules



Primary rule:



\- Push notifications are used when the recipient has the TimeOut app installed.

\- SMS messages are used when the recipient does not have the app installed.



All SMS messages should include the app download link.



Example footer for SMS messages:



Download TimeOut: \[app link]



\---



\## Message-First Philosophy



TimeOut assumes that users often respond directly from their phones without opening the app.



Therefore:



\- sit requests must be readable at a glance

\- responses must require minimal effort

\- notifications should avoid unnecessary complexity



\---



\## Sit Request Notification



Trigger:  

A requester sends a sit request and AutoPing begins.



Channel:  

Push if the user has the app installed.  

SMS if the user does not have the app.



Example message:



Sara sent you a TimeOut sit request.



Friday 7:00–10:00 PM  

2 kids – Drop-off



Reply YES if you can help.



Download TimeOut: \[app link]



Important rule:



Sit requests use a YES-or-ignore response model.  

A NO response is not required.



\---



\## Sit Confirmed



Trigger:  

The first valid YES response is received.



Recipients:



\- requester

\- confirmed sitter



Example message:



TimeOut sit confirmed.



Requester: Sara  

Sitter: Kelly  

Friday 7:00–10:00 PM



Other responders receive:



Thanks! A sitter has already been confirmed.



\---



\## Reminder Notifications



Reminders are sent only after a sitter has been confirmed.



Schedule:



\- 24 hours before the sit

\- 1 hour before the sit

\- at the scheduled start time



Example reminder:



Reminder: Sara and Kelly have a TimeOut sit tomorrow at 7:00 PM.



Reminders should include both requester and sitter names.



\---



\## Sit Start



Trigger:  

Scheduled start time reached.



Example message:



Sara and Kelly’s TimeOut sit starts now.



\---



\## Cancel Sit



Most sit-related notifications should allow Cancel Sit.



Either party may cancel the sit before completion.



Design rule:



Instead of editing sit details, the system uses a cancel-and-recreate approach.



If details change:



1\. Cancel the sit  

2\. Create a new sit request  



This keeps notifications clear and simplifies system logic.



\---



\## End Sit



Trigger:  

Either party taps End Sit.



Example message:



Sara and Kelly’s TimeOut sit has ended.



\---



\## Points Transfer



Trigger:  

Sit settlement completed.



Example message:



Sit complete.



Kelly earned 12 TimeOut points from Sara.



\---



\## Emergency Daycare Pickup



Trigger:  

Emergency Daycare Pickup preset is used.



Channel:  

Broadcast to selected sitter list.



Example message:



Sara needs emergency daycare pickup.



Location: Kindercare on Martin Way  

Pickup needed within about one hour.



Reply YES if you can help.



Download TimeOut: \[app link]



Emergency requests include a +6 point incentive.



\---



\## TimeOut Gathering Host Request



Trigger:  

System scheduled gathering cycle.



Example message:



Who will host the next TimeOut gathering?



Host earns 1 point from each member.  

Reply YES to host.



The first YES becomes host.



\---



\## TimeOut Gathering RSVP



After the host is confirmed, the system broadcasts the gathering notice.



Example:



TimeOut gathering Monday at 8:00 PM.



Sara is hosting.



Reply YES if you are coming.



Multiple YES responses are allowed.



\---



\## Playdate Broadcast



Playdates are user-initiated broadcast events.



Example message:



Playdate at Evergreen Park  

Saturday 10:00 AM



Sara and Kelly supervising.



Reply YES if your kids are coming.



Multiple YES responses are allowed.



\---



\## Notification Design Principles



All messages should follow these rules:



\- short

\- glanceable

\- friend-origin tone

\- one clear action



A user should understand the message in 2–3 seconds.

