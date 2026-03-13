\# AutoPing State Machine



States:



draft → ready → ping\_pending → confirming\_yes → confirmed → in\_progress → ending → posted



Rules:



\- Sequential pings

\- First YES wins

\- Earlier pings remain active

\- Silence counts as no response

\- Cancel and recreate instead of editing sit

