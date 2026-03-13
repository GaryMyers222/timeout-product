\# TimeOut Conflict \& Decision Register



This document records key product decisions made during design so they remain clear during development.



\---



\## Identity and Access



Decision:  

TimeOut will use \*\*phone-number verification\*\* rather than usernames and passwords.



Reason:  

Circles are invitation-only and involve trusted friends. Phone verification provides sufficient identity confirmation while keeping onboarding friction low.



\---



\## Invitation Model



Decision:  

Primary invite matching is done through \*\*phone-number matching\*\*, not join codes.



Reason:  

Users invite friends from their phone contacts, so the system already knows who is being invited.



Join codes may exist as a \*\*fallback\*\*, not the primary mechanism.



\---



\## Invite Conflict Resolution



Decision:  

If a phone number receives invitations from different circles, the user will be shown a \*\*Choose Your Circle\*\* screen.



Reason:  

This prevents accidental circle fragmentation while allowing legitimate multiple-circle invitations.



\---



\## Sit Editing



Decision:  

v1 does not support complex editing of confirmed sits.



Instead:  

Users should \*\*cancel the sit and create a new one\*\*.



Reason:  

This simplifies the system and avoids confusing notification chains.



Corrections after settlement can be handled using \*\*Transfer Points\*\*.



\---



\## Response Model



Decision:  

Sit requests support \*\*YES or ignore\*\*.



A formal NO response is not required.



Reason:  

Parents are busy and most sit requests should be digestible at a glance.



Silence is treated as "no response".



\---



\## AutoPing Ordering



Decision:  

The default sitter list is \*\*comprehensive and ordered by lowest points first\*\*.



Users may exclude individuals intentionally but the system should encourage fairness.



Reason:  

Members with higher point debt should have opportunities to take sits and restore balance.



\---



\## Emergency Daycare Pickup



Decision:  

Emergency pickup requests use \*\*broadcast ping\*\* to the selected list rather than sequential AutoPing.



Additional rule:  

Emergency pickup carries a \*\*+6 point incentive\*\*.



Reason:  

The request is urgent and requires rapid responses.



\---



\## Points Adjustments



Decision:  

The system supports \*\*Adjustments to Balance Sequestered Points\*\* when members leave the circle with point balances that remove points from circulation.



Reason:  

Maintains long-term ledger balance.



\---



\## Meeting Scheduling



Decision:  

Official TimeOut gatherings occur on a known schedule and are \*\*system-generated events\*\*, not presets.



The system will:

1\. request a host  

2\. request an agenda lead  

3\. broadcast RSVP  



Reason:  

Meetings are culturally important but should not require manual scheduling each time.



\---



\## Delegation / Spouse Participation



Decision:  

The app will not enforce strict delegation rules in v1 but will allow \*\*sitter disclosure\*\* at confirmation.



Example options:

\- confirmed member only

\- confirmed member plus accompanying adult

\- different attending adult disclosed



Reason:  

Circles may have different norms and emergencies may require flexibility.



\---



\## Geographic Scope



Decision:  

The system will encourage recruiting friends within approximately \*\*15 minutes travel time\*\*, but will not enforce strict geographic limits.



Reason:  

Tight geography improves sit practicality while still allowing flexibility for real relationships.

