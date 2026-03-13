\# AutoPing State Machine



The AutoPing system coordinates sit requests within a TimeOut circle and finds the first sitter who accepts.



The system is designed to:



\- minimize effort for the requester

\- give members fair opportunities to earn points

\- prevent notification overload

\- handle race conditions safely



\---



\## Core Modes



\### Sequential AutoPing



Used for most sit requests.



Process:



1\. Build the default sitter list ordered by \*\*lowest points balance first\*\*.

2\. Send the sit request to the first sitter.

3\. Wait the ping interval.

4\. Send the request to the next sitter.

5\. Continue until the first \*\*YES\*\* is received.



Earlier pings remain active.



The first valid \*\*YES\*\* wins.



\---



\### Broadcast Mode



Used for specific request types:



\- Emergency Daycare Pickup

\- Playdate broadcasts

\- TimeOut gathering RSVP



Broadcast sends the request to all selected members at once.



Emergency pickup still accepts only the \*\*first YES\*\*.



Playdates and gatherings accept \*\*multiple YES responses\*\*.



\---



\## Sit Request States



The sit lifecycle moves through the following states.



\### draft



The requester is entering sit details.



\---



\### ready



Sit details are complete and the sitter list has been reviewed.



\---



\### ping\_pending



AutoPing has started and notifications are being sent.



\---



\### confirming\_yes



The system has received a YES and is verifying it is the first valid response.



This state prevents race conditions.



\---



\### confirmed



A sitter has been confirmed.



The AutoPing sequence stops sending new pings.



Other responders receive a notice that a sitter has already been found.



\---



\### in\_progress



The sit has begun.



\---



\### ending



One of the parties tapped \*\*End Sit\*\* and the system is calculating final duration.



\---



\### posted



Points have been transferred and the sit is complete.



\---



\### cancelled



The sit request was cancelled by either party.



\---



\### timed\_out



AutoPing reached the end of the sitter list without finding a sitter.



\---



\## Response Model



Sit requests use a \*\*YES-or-ignore\*\* response model.



Members may:



\- reply \*\*YES\*\*

\- ignore the request



A NO response is not required.



Silence is treated as \*\*no response\*\*.



\---



\## Ping Attempt Records



Each AutoPing attempt is stored as a child record of the sit request.



Suggested fields:



\- ping\_attempt\_id

\- sit\_request\_id

\- target\_user\_id

\- target\_phone

\- sequence\_order

\- sent\_at

\- state

\- response\_received\_at



\---



\## First YES Rule



The system must enforce a strict \*\*first YES wins\*\* rule.



When a YES arrives:



1\. Verify the sit request is still open.

2\. Verify no sitter has already been confirmed.

3\. Lock the sitter as the confirmed sitter.

4\. Transition to \*\*confirmed\*\* state.



Later YES responses receive a polite message indicating the sit has already been covered.



\---



\## Cancel-and-Recreate Rule



In v1, confirmed sit requests are not edited.



If a change is required:



1\. Cancel the sit.

2\. Create a new sit request.

3\. Optionally narrow the sitter list.



This avoids complex edit logic and reduces notification confusion.



\---



\## Emergency Daycare Pickup



Emergency pickup uses broadcast mode but still follows the \*\*first YES wins\*\* rule.



Additional rules:



\- request should occur within approximately one hour

\- requester must review the sitter list before sending

\- emergency requests include a +6 point incentive



\---



\## Reminder Schedule



Confirmed sits trigger reminders at:



\- 24 hours before

\- 1 hour before

\- sit start time



Reminders include both \*\*requester and sitter names\*\*.



\---



\## End Sit Behavior



Either party may tap \*\*End Sit\*\*.



Duration is calculated and rounded to the nearest 15 minutes.



Points are transferred and recorded in the ledger.



\---



\## Correction After Posting



If the sit duration or circumstances differ from expectations:



Members should use \*\*Transfer Points\*\* rather than editing the sit.



This keeps the sit history immutable and simpler to implement.

