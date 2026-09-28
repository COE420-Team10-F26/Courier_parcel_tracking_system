# Courier Parcel Tracking System — Team Consolidated Requirements (Lab 3)

Team 10 — Sarhan Alzaabi (b00100669), Kevin (b00098368), Muhammad Hammad Khan (b00094579), Ahmed El Sabagh (b00095506)

This section consolidates all four members' individual contributions into one numbered team-level specification, per the Lab 3 "Individual Contribution Rule." Original per-student IDs are shown in brackets for traceability. No duplicates or conflicts were found between members' requirements since each area is distinct in scope.

---

## Consolidated Scenarios

| ID | Title | Actor | Description | Contributor |
| --- | --- | --- | --- | --- |
| S-01 | Customer registers and books a parcel | Customer | Abdulla registers with his name, email and phone number, logs in, and books a parcel to Omar Khalid in Abu Dhabi. The system confirms the booking and gives him tracking number CPT-000123. | Sarhan |
| S-02 | Customer tracks a booked parcel | Customer | Abdulla mistypes his tracking number, gets "not found," corrects it, and sees the status update from "Booked" to "Picked Up" to "Out for Delivery." | Sarhan |
| S-03 | Admin assigns parcel to field staff | Admin | Amina logs in, reviews unassigned "Pending Pickup" parcels and field staff workload, and assigns a parcel to Ahmed based on his delivery zone. The system updates the status and notifies him. | Kevin |
| S-04 | A field staff member's delivery round | Field Staff | A field staff member picks up parcels, confirms pickup (status + timestamp recorded), delivers them, captures proof of delivery, and marks one delivery as "attempted" when no one answers. | Hammad |
| S-05 | Delayed parcel triggers alerts and history log | Customer, Field Staff, Admin | A parcel's status changes are logged and trigger customer notifications at each step; when delivery runs 6 hours late, a delay alert appears on the admin dashboard. Ahmed later reviews the full timestamped history. | Ahmed |

---

## Consolidated Functional Requirements

| ID | Requirement | Source | Contributor |
| --- | --- | --- | --- |
| FR-01 | Register as a customer with name, email, phone number, password (min. 8 characters); reject duplicate email or empty fields. | S-01 | Sarhan |
| FR-02 | Log in with email/password, route to role-based dashboard, show generic error on failure. | S-01, S-02 | Sarhan |
| FR-03 | Logged-in customer books a parcel with recipient + parcel details; reject incomplete/invalid submissions. | S-01 | Sarhan |
| FR-04 | Auto-generate a unique tracking number (format CPT-XXXXXX) on booking confirmation. | S-01 | Sarhan |
| FR-05 | Customer looks up a tracking number to see current status and full status history, or "not found." | S-02 | Sarhan |
| FR-06 | Admin views all registered users (name, email, role, status), searchable/filterable. | Admin | Kevin |
| FR-07 | Admin deactivates/reactivates customer or field staff accounts; deactivated users can't log in, records retained. | Admin | Kevin |
| FR-08 | Admin dashboard lists all parcels with current status and assigned field staff member. | S-03 | Kevin |
| FR-09 | Admin bulk-assigns multiple "Pending Pickup" parcels to one field staff member in one action; statuses change to "Assigned," one notification sent. | S-03 | Kevin |
| FR-10 | Admin creates a field staff/admin account (name, email, role); system generates a temporary password requiring change at first login. | Admin | Kevin |
| FR-11 | Field staff views parcels assigned to them for pickup, with sender address and parcel details. | S-04 | Hammad |
| FR-12 | Field staff views parcels assigned to them for delivery, with recipient address and parcel details. | S-04 | Hammad |
| FR-13 | Field staff updates parcel delivery status (dispatched, in shipment, etc.) from their task list. | S-04 | Hammad |
| FR-14 | Field staff confirms pickup from sender; system records pickup time. | S-04 | Hammad |
| FR-15 | Field staff captures proof of delivery (name, signature, ID) before the system marks delivery complete. | S-04 | Hammad |
| FR-16 | System notifies the customer whenever their parcel's status changes. | S-05 | Ahmed |
| FR-17 | System notifies the assigned field staff member when a new pickup/delivery task is assigned. | S-05 | Ahmed |
| FR-18 | System records a timestamped status-history entry (status, timestamp, actor/location) on every status change. | S-05 | Ahmed |
| FR-19 | Customer views the complete chronological status history of a parcel by tracking number. | S-05 | Ahmed |
| FR-20 | Admin dashboard shows a real-time alert when a parcel remains undelivered beyond its expected window. | S-05 | Ahmed |

---

## Consolidated Non-Functional Requirements

| ID | Category | Requirement | Contributor |
| --- | --- | --- | --- |
| NFR-01 | Performance | Tracking page shows status within 2s for 95% of requests, up to 50 concurrent users. | Sarhan |
| NFR-02 | Performance | Login completes within 3s for 95% of attempts, up to 50 concurrent users. | Sarhan |
| NFR-03 | Usability | First-time customer registers + books in under 5 minutes with no instructions (tested with 5 new users). | Sarhan |
| NFR-04 | Usability | Form errors shown inline in plain language; other entered data preserved. | Sarhan |
| NFR-05 | Reliability | Lost connection mid-submission never saves a partial/duplicate record. | Sarhan |
| NFR-06 | Security | All passwords stored with salted adaptive hashing (bcrypt/Argon2); never logged or transmitted in plaintext. | Kevin |
| NFR-07 | Security | Session terminates after 30 min inactivity; all traffic over HTTPS. | Kevin |
| NFR-08 | Maintainability | All changes go through a feature branch + review before merging to main; meaningful commit messages. | Kevin |
| NFR-09 | Security | Account locks 15 min after 4 failed logins; generic error message shown either way. | Kevin |
| NFR-10 | Maintainability | Every unhandled error logged with timestamp, function, and message. | Kevin |
| NFR-11 | Reliability | Lost connection after a status update queues it locally and retries within 30s of reconnect, no data loss. | Hammad |
| NFR-12 | Reliability | Proof-of-delivery photo stored locally; retries upload up to 3 times on failure. | Hammad |
| NFR-13 | Reliability | Status update reflects in customer/admin views within 5s under normal network conditions. | Hammad |
| NFR-14 | Portability | Field staff interface works fully on Android and iOS with no functionality loss. | Hammad |
| NFR-15 | Portability | Field staff interface renders correctly on mobile, tablet, and desktop without usability loss. | Hammad |
| NFR-16 | Scalability | Notification service handles up to 10,000 status-change notifications/hour, ≤2 min queuing delay. | Ahmed |
| NFR-17 | Scalability | Tracking-history store handles ≥1,000,000 records without tracking page load exceeding 2s. | Ahmed |
| NFR-18 | Robustness | If a notification channel is down, queue and retry for up to 24h before marking failed. | Ahmed |
| NFR-19 | Robustness | Notification failures never block status-history logging; the two are independent. | Ahmed |
| NFR-20 | Scalability | Admin dashboard supports ≥50 concurrent admins viewing delay alerts, ≤3s added refresh latency. | Ahmed |

**Note:** NFR-08 (Kevin) describes a Git branching/review workflow, not a property of the system itself. Worth flagging with the instructor since it isn't really a non-functional requirement in the standard sense — the team may want to swap it for an actual system NFR (e.g. an additional Maintainability requirement about code/module structure) before final submission.

---

## Consolidated Use Cases

| ID | Name | Actor | Description | Contributor |
| --- | --- | --- | --- | --- |
| UC-01 | Register Account | Customer | Create an account with name, email, phone, password. | Sarhan |
| UC-02 | Log In | Customer | Enter credentials, land on role-based dashboard. | Sarhan |
| UC-03 | Book Parcel | Customer | Enter recipient + parcel details to book delivery. | Sarhan |
| UC-04 | Generate Tracking Number | Customer | System issues a tracking number on booking confirmation. | Sarhan |
| UC-05 | Track Parcel | Customer | Look up status + status history by tracking number. | Sarhan |
| UC-06 | View and Search User Accounts | Admin | View/search/filter all registered users. | Kevin |
| UC-07 | Deactivate/Reactivate User Account | Admin | Toggle a user's account status; records retained. | Kevin |
| UC-08 | View All Parcels on Dashboard | Admin | View all parcels with status and assigned staff. | Kevin |
| UC-09 | Assign Parcels to Field Staff | Admin | Bulk-assign "Pending Pickup" parcels to a staff member. | Kevin |
| UC-10 | Create Staff/Admin Account | Admin | Create a new staff/admin account with a temp password. | Kevin |
| UC-11 | Change Temporary Password | Field Staff / Admin | Set a permanent password at first login. | Kevin |
| UC-12 | View Assigned Pickups | Field Staff | View parcels assigned for pickup. | Hammad |
| UC-13 | View Assigned Deliveries | Field Staff | View parcels assigned for delivery. | Hammad |
| UC-14 | Update Parcel Status | Field Staff | Change a parcel's status as it moves through delivery. | Hammad |
| UC-15 | Confirm Pickup | Field Staff | Confirm a parcel has been collected. | Hammad |
| UC-16 | Record Proof of Delivery | Field Staff | Capture recipient confirmation on delivery. | Hammad |
| UC-17 | Receive Status Notification | Customer | Get notified whenever parcel status changes. | Ahmed |
| UC-18 | Receive Assignment Notification | Field Staff | Get notified when assigned a new task. | Ahmed |
| UC-19 | View Parcel Tracking History | Customer | View full timestamped status history. | Ahmed |
| UC-20 | View Dashboard Delay Alerts | Admin | See real-time alerts for overdue parcels. | Ahmed |
| UC-21 | Search Tracking History by Parcel ID | Admin | Look up any parcel's full history (e.g. for a complaint). | Ahmed |

**Note:** Kevin's diagram also shows a "Record Status History" use case (system logs a history entry on every status update, per FR-18) that isn't formally listed as anyone's individual UC — it's implied by FR-18 but has no owner or UC entry yet. The team should either add it as a UC (probably under Hammad's or Ahmed's area) or fold it into UC-14's description before final submission.

---

## Consolidated Use Case Relationships

| ID | Base Use Case | Related Use Case | Relationship | Justification | Source |
| --- | --- | --- | --- | --- | --- |
| R-01 | UC-03 Book Parcel | UC-02 Log In | `<<include>>` | Can't book without logging in. | Sarhan |
| R-02 | UC-03 Book Parcel | UC-04 Generate Tracking Number | `<<include>>` | Every booking always gets a tracking number. | Sarhan |
| R-03 | UC-05 Track Parcel | UC-02 Log In | `<<include>>` | Can't track without logging in. | Sarhan |
| R-04 | UC-07 Deactivate/Reactivate User Account | UC-06 View and Search User Accounts | `<<include>>` | Admin must locate the user before changing their status. | Kevin |
| R-05 | UC-09 Assign Parcels to Field Staff | UC-08 View All Parcels on Dashboard | `<<include>>` | Assignment always starts from the parcel list. | Kevin |
| R-06 | UC-02 Log In | UC-11 Change Temporary Password | `<<extend>>` | Only triggered when logging in with a temporary password (corrected from Kevin's table, which referenced UC-05 by mistake — the use case named "Change Temporary Password" is UC-11, not UC-05). | Kevin |
| R-07 | UC-09 Assign Parcels to Field Staff | UC-18 Receive Assignment Notification | `<<include>>` | Every assignment always sends one notification to the field staff member. | Kevin |
| R-08 | UC-15 Confirm Pickup | UC-14 Update Parcel Status | `<<include>>` | Confirming pickup always sets status to Picked Up. | Hammad |
| R-09 | UC-16 Record Proof of Delivery | UC-14 Update Parcel Status | `<<include>>` | Recording proof of delivery always sets status to Delivered. | Hammad |
| R-10 | UC-14 Update Parcel Status | UC-17 Receive Status Notification | `<<include>>` | A status change always triggers a customer notification. | Ahmed (proposed) |
| R-11 | UC-20 View Dashboard Delay Alerts | UC-08 View All Parcels on Dashboard | `<<extend>>` | Delay alert is conditional, not part of every dashboard view. | Ahmed (proposed, mapped to Kevin's actual UC-08) |
| R-12 | UC-10 Create Staff/Admin Account | UC-09 Assign Parcels to Field Staff | `<<extend>>` | Visible in Kevin's diagram — admin can optionally create a staff account mid-assignment if the needed person doesn't exist yet. **Not yet in any written table — needs Kevin's confirmation.** | Diagram (unconfirmed) |

R-10 also implies a history-logging relationship (Update Parcel Status → the unowned "Record Status History" use case noted above) — leave that row out of the table until the use case itself is formally assigned an ID.

---

## AI Use Declaration

Claude (Anthropic) was used to help draft, format, and consolidate this requirements specification — merging individual FR/NFR/UC/scenario contributions into one numbered team document, checking for duplicates/conflicts, and reconciling the use case relationship table against the team's UML diagram. All underlying requirements, scenarios, and design decisions originated from the team members themselves; AI assistance was limited to organization, consistency-checking, and wording. Per the instructor's guidance, no AI tool is listed as a contributor or co-author in the GitHub repository.
