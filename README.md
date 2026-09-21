# Task Board

A JIRA-style task tracker built natively on Salesforce, created as a hands-on project to learn core Salesforce Admin skills — data modeling, security, and automation — by building something real rather than working through isolated tutorials.

## Features

- **Kanban board** — tasks organized by status (To Do, In Progress, In Review, Done)
- **Role-based access** — a dedicated Team Member profile with scoped object permissions and tab visibility, separate from full Admin access, validated by testing as an impersonated non-admin user
- **Validation rules** — enforces business logic like requiring an assignee before a task can be marked Done, and blocking past due dates on new tasks (while still allowing edits to already-overdue tasks)
- **Automated notifications** — a Record-Triggered Flow sends a custom notification and email the moment a task is assigned, so nobody has to manually check the board
- **Related data model** — Projects, Task Items, and Comments linked through Lookup and Master-Detail relationships, with cascading deletes and auto-numbered records (e.g. `TASK-0001`)

## Data Model

| Object | Relationship | Purpose |
|---|---|---|
| `Project__c` | Parent | Top-level container (e.g. "Website Redesign") |
| `Task_Item__c` | Lookup to Project | Individual tasks/tickets, with Status, Priority, Type, Assigned To, Due Date |
| `Comment__c` | Master-Detail to Task Item | Discussion thread on a task, cascade-deletes with its parent |

## Tech Used

- Custom Objects, Fields & Relationships
- Validation Rules
- Record-Triggered Flow (with Custom Notifications and Email)
- Profiles & Object/Tab-level Permissions
- Lightning App Builder (Kanban list view)

## What I Learned

This project was built end-to-end as a solo learning exercise, including debugging a real Record-Triggered Flow failure (`Invalid parameter value for: customNotifTypeId`) down to its root cause using Flow Debug logs and direct SOQL queries via Developer Console — rather than guessing at fixes.

## Known Limitations

- The Kanban board's display mode (Table vs. Kanban) doesn't reliably persist per user in Salesforce's native List View component — a known platform constraint. A custom Lightning Web Component board is a planned future improvement.
- Sharing is currently org-wide (Public Read/Write); project-based visibility scoping (via a junction object + sharing rules) is a planned next step.

## Setup

This project was built and is version-controlled using the Salesforce CLI. To retrieve this metadata into your own org:

\`\`\`bash
sf project retrieve start --metadata CustomObject:Project__c CustomObject:Task_Item__c CustomObject:Comment__c Flow:Notify_User_on_Task_Assignment
\`\`\`
