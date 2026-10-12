# 1. How it works, explained from scratch
### The problem.
In many companies, leave requests arrive by WhatsApp, calls and paper. Managers forget to approve, HR edits a spreadsheet by hand, nobody knows anyone’s real balance, and sometimes three people from one team end up off at the same time. I built a system that handles the whole process automatically.
### The tools.
The workflow is built in n8n, a tool where you connect “nodes” (each does one job) into a flow. An employee can reach it two ways: a Telegram bot or a web form. Google Sheets is the database, Gmail handles HR approval, and an AI model (Claude) is used only to read the chat messages.
![]()
![]()
![]()
