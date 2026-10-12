# 1. How it works, explained from scratch
### The problem.
In many companies, leave requests arrive by WhatsApp, calls and paper. Managers forget to approve, HR edits a spreadsheet by hand, nobody knows anyone’s real balance, and sometimes three people from one team end up off at the same time. I built a system that handles the whole process automatically.
### The tools.
The workflow is built in n8n, a tool where you connect “nodes” (each does one job) into a flow. An employee can reach it two ways: a Telegram bot or a web form. Google Sheets is the database, Gmail handles HR approval, and an AI model (Claude) is used only to read the chat messages.
![](https://github.com/Ebenezer080925/Hr-leave-automation-n8n/blob/main/Screenshot%202026-10-09%20151216.png)
ZOOMED IN
![](https://github.com/Ebenezer080925/Hr-leave-automation-n8n/blob/main/Screenshot%202026-10-09%20151403.png)
![](https://github.com/Ebenezer080925/Hr-leave-automation-n8n/blob/main/Screenshot%202026-10-09%20151531.png)
Sheets to get and append details
![](https://github.com/Ebenezer080925/Hr-leave-automation-n8n/blob/main/Screenshot%202026-10-12%20220447.png)
![](https://github.com/Ebenezer080925/Hr-leave-automation-n8n/blob/main/Screenshot%202026-10-12%20220505.png)

## What happens, step by step:
### -The employee asks for leave
On Telegram they just chat (“Hi, I’m EMP001, I need leave from the 14th to the 16th for a wedding”). The AI agent reads it and pulls out four things: employee ID, start date, end date, reason. If something is missing, it asks. It has a memory per chat so it can handle details arriving across several messages. On the web form, they fill the same four fields directly and the AI isn’t needed.
### -Both paths merge.
A node called Request turns either input into the same shape, so everything after it is shared. It also records which channel they used.
### -Identity check. 
The workflow looks up the employee ID in the employees sheet. If it isn’t there, the person is told “ID not found”.
### -Validity and balance.
It counts the days requested. If the end date is before the start date, or the days are more than their remaining balance, the request is refused.
### -Team clash check.
It reads all approved leave for the same team and checks each requested day. If the number of teammates already off on any day reaches the cap (currently 2), the request is refused and the clashing dates are listed.
### -Save and notify. 
If everything passes, the request is saved in leave_requests as Pending, the employee gets a “submitted, awaiting HR” message, and HR gets an email.
### -HR decision.
The email has Approve and Decline buttons. The workflow pauses until HR clicks. On Approve, the row becomes Approved and the balance is reduced. On Decline, it becomes Rejected. The employee is told either way.
### -Replies go to the right place.
Every outcome builds a subject and a text, then goes through a Channel check: Telegram users get a Telegram message, form users get an email.

![Json](https://github.com/Ebenezer080925/Hr-leave-automation-n8n/blob/main/Hr%20Leave%20Request.json)

HR Approval/ Human intervantion
![](https://github.com/Ebenezer080925/Hr-leave-automation-n8n/blob/main/Screenshot%202026-10-12%20222851.png)

Telegram and form inputs
![](https://github.com/Ebenezer080925/Hr-leave-automation-n8n/blob/main/Screenshot%202026-10-12%20223332.png)
![](https://github.com/Ebenezer080925/Hr-leave-automation-n8n/blob/main/Screenshot%202026-10-12%20220830.png)



