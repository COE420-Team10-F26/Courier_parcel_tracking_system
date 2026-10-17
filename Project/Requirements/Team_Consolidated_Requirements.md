# Courier Parcel Tracking System — Team Consolidated Requirements (Lab 3)

Team 10 — Sarhan Alzaabi (b00100669), Kevin (b00098368), Muhammad Hammad Khan (b00094579), Ahmed El Sabagh (b00095506)

This section consolidates all four members' individual contributions into one numbered team-level specification, per the Lab 3 "Individual Contribution Rule." Original per-student IDs are shown in brackets for traceability. No duplicates or conflicts were found between members' requirements since each area is distinct in scope.

---

## Consolidated Scenarios

| ID | Title | Actor | Description | Contributor |
| --- | --- | --- | --- | --- |
| S-01 | Customer registers and books a parcel | Customer | Abdulla registers with his name, email and phone number, logs in, and books a parcel to Omar Khalid in Abu Dhabi. The system confirms the booking and gives him tracking number CPT-000123, with the expected delivery window. (Note: delivery fees and payment are out of scope, so the system goes straight to confirmation.) | Sarhan |
| S-02 | Customer tracks a booked parcel | Customer | Abdulla mistypes his tracking number, gets "not found," corrects it, and sees the current status "Picked Up" with the expected delivery window, then opens the tracking history to see earlier statuses. Later he sees "Out for Delivery." | Sarhan |
| S-03 | Admin assigns parcel to field staff | Admin | Amina logs in, reviews unassigned "Pending Pickup" parcels and field staff workload, and assigns a parcel to Ahmed based on his delivery zone. Zones are set by an admin, not allocated automatically. The system updates the status and notifies him. | Kevin |
| S-04 | A field staff member's delivery round | Field Staff | A field staff member picks up parcels, confirms pickup (status + timestamp recorded), delivers them, captures proof of delivery (name, signature, Emirates ID), and marks one delivery as "Attempted" with a mandatory note when no one answers; it will be retried (up to 3 attempts). | Hammad |
| S-05 | Delayed parcel triggers alerts and history log | Customer, Field Staff, Admin | A parcel's status changes are logged and trigger customer notifications at each step; when the parcel goes 48 hours without a status update, its status changes to "Delayed" and a delay alert appears on the admin dashboard. Ahmed later reviews the full timestamped history. | Ahmed |

---

## Consolidated Functional Requirements

| ID | Requirement | Source | Contributor |
| --- | --- | --- | --- |
| FR-01 | Register as a customer with name, email, phone number, password (min. 8 characters); reject duplicate email or empty fields. | S-01 | Sarhan |
| FR-02 | All users (customer, field staff, admin) log in with email/password and are routed to their role's dashboard; generic error on failure. | S-01, S-02 | Sarhan |
| FR-03 | Logged-in customer books a parcel with recipient details (full name, address, phone) and parcel details (weight > 0 and ≤ 70 kg; length, width and height each > 0 and ≤ 150 cm; description). Rejects the booking if a required field is empty, the phone is not a valid UAE number (+971 followed by 9 digits), or weight/dimensions are outside these limits. On confirmation, shows the expected delivery window: 24 hours from pickup if the recipient is in the same emirate as the sender, 48 hours if it is in a different emirate. | S-01 | Sarhan |
| FR-04 | Auto-generate a unique tracking number (format CPT-XXXXXX) on booking confirmation. | S-01 | Sarhan |
| FR-05 | Customer enters a tracking number to see the parcel's current status and expected delivery window, or "not found." The full status history is a separate function (FR-19). | S-02 | Sarhan |
| FR-06 | Admin views all registered users (name, email, role, status), searchable/filterable. | Admin | Kevin |
| FR-07 | Admin deactivates/reactivates customer or field staff accounts; deactivated users can't log in, records retained. | Admin | Kevin |
| FR-08 | Admin dashboard lists all parcels with their current status, destination zone and assigned field staff member, and shows each staff member's admin-assigned zone and current workload to support manual assignment. | S-03 | Kevin |
| FR-09 | Admin bulk-assigns multiple "Pending Pickup" parcels to one field staff member in one action; statuses change to "Assigned"; one summary notification is sent to the field staff member and an "Assigned" notification to each customer. | S-03 | Kevin |
| FR-10 | Admin creates a field staff/admin account (name, email, role, and zone for field staff); system generates a temporary password requiring change at first login. | Admin | Kevin |
| FR-11 | Field staff views parcels assigned to them for pickup, with sender address and parcel details. | S-04 | Hammad |
| FR-12 | Field staff views parcels assigned to them for delivery, with recipient address and parcel details. | S-04 | Hammad |
| FR-13 | Field staff updates parcel status (Picked Up, In Transit, Out for Delivery, Attempted, Delivered) from their task list. "Attempted" requires a note; after the 3rd failed attempt the status becomes "Returned to Sender" and the customer is notified. | S-04 | Hammad |
| FR-14 | Field staff confirms pickup from sender; system records pickup time. | S-04 | Hammad |
| FR-15 | Field staff captures proof of delivery (recipient's full name, digital signature, Emirates ID) before the system marks delivery complete. | S-04 | Hammad |
| FR-16 | System notifies the customer whenever their parcel's status changes. | S-05 | Ahmed |
| FR-17 | System notifies the assigned field staff member when new pickup/delivery tasks are assigned; a bulk assignment sends one summary notification (see FR-09). | S-05 | Ahmed |
| FR-18 | System records a timestamped status-history entry (status, timestamp, actor/location) on every status change. | S-05 | Ahmed |
| FR-19 | Customer (by tracking number) and admin (by parcel ID) view the complete chronological, timestamped status history of a parcel. | S-05 | Ahmed |
| FR-20 | Admin dashboard shows a real-time alert when an undelivered parcel has had no status change for 48 hours; the parcel's status becomes "Delayed". | S-05 | Ahmed |


### List of Status Types

These are the only parcel statuses used across the scenarios, requirements and use cases.

| Status | Meaning |
| --- | --- |
| Booked | Customer completes a booking and a tracking number (CPT-XXXXXX) is generated. |
| Pending Pickup | Confirmed parcel waiting for an admin to assign it to field staff. |
| Assigned | Admin assigns the parcel to a field staff member. |
| Picked Up | Field staff collects the parcel from the sender and confirms it in the app (pickup time recorded). |
| In Transit | Parcel is moving through sorting hubs toward the destination city. |
| Out for Delivery | Field staff starts the delivery round with the parcel. |
| Attempted | A delivery attempt failed (e.g. no one answered); a note is mandatory and the parcel is retried (maximum 3 attempts). |
| Delivered | Parcel handed over and valid proof of delivery captured (name, signature, Emirates ID). |
| Delayed | Undelivered parcel with no status change for 48 hours; also raises an alert on the admin dashboard. |
| Returned to Sender | Final state after the 3rd failed delivery attempt; the customer is notified. |

---

## Consolidated Non-Functional Requirements

| ID | Category | Requirement | Contributor |
| --- | --- | --- | --- |
| NFR-01 | Performance | Under normal 4G/broadband conditions (at least 10 Mbps), the tracking page shows status within 2s for 95% of requests, up to 50 concurrent users. | Sarhan |
| NFR-02 | Performance | Under normal 4G/broadband conditions (at least 10 Mbps), login completes within 3s for 95% of attempts, up to 50 concurrent users. | Sarhan |
| NFR-03 | Usability | First-time customer registers + books in under 5 minutes with no instructions (tested with 5 new users). | Sarhan |
| NFR-04 | Usability | Form errors shown inline in plain language; other entered data preserved. | Sarhan |
| NFR-05 | Reliability | Lost connection mid-submission never saves a partial/duplicate record. | Sarhan |
| NFR-06 | Security | All passwords stored with salted adaptive hashing (bcrypt/Argon2); never logged or transmitted in plaintext. | Kevin |
| NFR-07 | Security | Session terminates after 30 min inactivity; all traffic over HTTPS. Protected actions (such as booking or updating status) use the existing authenticated session and do not ask for a new login; once the session expires, the user must log in again. | Kevin |
| NFR-08 | Maintainability | Notification channels (email/SMS) and the delay threshold (48 hours) are stored in configuration, changeable without code changes or redeployment. | Kevin |
| NFR-09 | Security | Account locks 15 min after 4 failed logins; generic error message shown either way. | Kevin |
| NFR-10 | Maintainability | Every unhandled error logged with timestamp, function, and message. | Kevin |
| NFR-11 | Reliability | Lost connection after a status update queues it locally and retries within 30s of reconnect, no data loss. | Hammad |
| NFR-12 | Reliability | Proof-of-delivery data (name, signature, Emirates ID) stored locally on the device; retries upload up to 3 times before prompting manual sync. | Hammad |
| NFR-13 | Reliability | Status update reflects in customer/admin views within 5s under normal 4G/broadband conditions (at least 10 Mbps). | Hammad |
| NFR-14 | Portability | Field staff interface works fully on Android 10+ and iOS 15+ with no functionality loss. | Hammad |
| NFR-15 | Portability | Field staff interface renders correctly on mobile, tablet, and desktop (screen widths 360px to 1920px) without usability loss. | Hammad |
| NFR-16 | Scalability | Notification service handles up to 10,000 status-change notifications/hour, ≤2 min queuing delay. | Ahmed |
| NFR-17 | Scalability | Tracking-history store handles ≥1,000,000 records without tracking page load exceeding 2s. | Ahmed |
| NFR-18 | Robustness | If a notification channel is down, queue and retry for up to 24h before marking failed. | Ahmed |
| NFR-19 | Robustness | Notification failures never block status-history logging; the two are independent. | Ahmed |
| NFR-20 | Scalability | Admin dashboard supports ≥50 concurrent admins viewing delay alerts, ≤3s added refresh latency. | Ahmed |

**Note:** Performance NFRs state the network conditions they are measured under, and platform NFRs state the supported OS versions and screen sizes, so every NFR can be tested.

---

## Consolidated Use Cases

| ID | Name | Actor | Description | Contributor |
| --- | --- | --- | --- | --- |
| UC-01 | Register Account | Customer | Create an account with name, email, phone, password. | Sarhan |
| UC-02 | Log In | Customer, Field Staff, Admin | Any user (customer, field staff, admin) enters credentials and lands on their role's dashboard. | Sarhan |
| UC-03 | Book Parcel | Customer | Enter recipient + parcel details to book delivery. | Sarhan |
| UC-04 | Generate Tracking Number | Customer | System issues a tracking number on booking confirmation. | Sarhan |
| UC-05 | Track Parcel | Customer | Look up current status + expected delivery window by tracking number. | Sarhan |
| UC-06 | View and Search User Accounts | Admin | View/search/filter all registered users. | Kevin |
| UC-07 | Deactivate/Reactivate User Account | Admin | Toggle a user's account status; records retained. | Kevin |
| UC-08 | View All Parcels on Dashboard | Admin | View all parcels with status and assigned staff. | Kevin |
| UC-09 | Assign Parcels to Field Staff | Admin | Bulk-assign "Pending Pickup" parcels to a staff member. | Kevin |
| UC-10 | Create Staff/Admin Account | Admin | Create a new staff/admin account with a temp password. | Kevin |
| UC-11 | Change Temporary Password | Field Staff / Admin | Set a permanent password at first login. | Kevin |
| UC-12 | View Assigned Pickups | Field Staff | View parcels assigned for pickup. | Hammad |
| UC-13 | View Assigned Deliveries | Field Staff | View parcels assigned for delivery. | Hammad |
| UC-14 | Update Parcel Status | Field Staff | Change a parcel's status as it moves through delivery, logging a timestamped history entry for each change. | Hammad |
| UC-15 | Confirm Pickup | Field Staff | Confirm a parcel has been collected. | Hammad |
| UC-16 | Record Proof of Delivery | Field Staff | Capture recipient confirmation on delivery. | Hammad |
| UC-17 | Receive Status Notification | Customer | Get notified whenever parcel status changes. | Ahmed |
| UC-18 | Receive Assignment Notification | Field Staff | Get notified when assigned a new task. | Ahmed |
| UC-19 | View Parcel Tracking History | Customer | View full timestamped status history. | Ahmed |
| UC-20 | View Dashboard Delay Alerts | Admin | See real-time alerts for overdue parcels. | Ahmed |
| UC-21 | Search Tracking History by Parcel ID | Admin | Look up any parcel's full history for monitoring and audit (complaints are not submitted through the system). | Ahmed |

**Note:** Logging a history entry is part of UC-14 Update Parcel Status, so there is no separate "Record Status History" use case (it was removed from the diagram).

---

## Consolidated Use Case Relationships

| ID | Base Use Case | Related Use Case | Relationship | Justification | Source |
| --- | --- | --- | --- | --- | --- |
| R-01 | UC-03 Book Parcel | UC-04 Generate Tracking Number | `<<include>>` | Every booking always gets a tracking number. | Sarhan |
| R-02 | UC-07 Deactivate/Reactivate User Account | UC-06 View and Search User Accounts | `<<include>>` | Admin must locate the user before changing their status. | Kevin |
| R-03 | UC-09 Assign Parcels to Field Staff | UC-08 View All Parcels on Dashboard | `<<include>>` | Assignment always starts from the parcel list. | Kevin |
| R-04 | UC-02 Log In | UC-11 Change Temporary Password | `<<extend>>` | Only triggered when logging in with a temporary password. | Kevin |
| R-05 | UC-09 Assign Parcels to Field Staff | UC-18 Receive Assignment Notification | `<<include>>` | Every assignment always sends one notification to the field staff member. | Kevin |
| R-06 | UC-15 Confirm Pickup | UC-14 Update Parcel Status | `<<include>>` | Confirming pickup always sets status to Picked Up. | Hammad |
| R-07 | UC-16 Record Proof of Delivery | UC-14 Update Parcel Status | `<<include>>` | Recording proof of delivery always sets status to Delivered. | Hammad |
| R-08 | UC-14 Update Parcel Status | UC-17 Receive Status Notification | `<<include>>` | A status change always triggers a customer notification. | Ahmed |
| R-09 | UC-20 View Dashboard Delay Alerts | UC-08 View All Parcels on Dashboard | `<<extend>>` | Delay alert is conditional, not part of every dashboard view. | Ahmed |

**Note:** The `<<include>>` relationships from Book Parcel and Track Parcel to Log In were removed, since logging in is a precondition of those use cases rather than part of their behaviour. Log In is associated with all three actors in the diagram.

---

## AI Use Declaration

Tools used: Gemini (and/or ChatGPT/Claude)

How we used them:

* Rubric Alignment: We prompted the AI to review our initial drafts of the Non-Functional Requirements (NFRs) to help ensure they were phrased in a measurable and verifiable way, as requested by the lab instructions.
* Logic Verification: We used AI as a sounding board to double-check the logic of our Use Case Relationships, specifically verifying whether certain interactions (like tracking a parcel or changing a temporary password) technically qualified as `<<include>>` or `<<extend>>`.
* Formatting and Proofreading: After all four team members drafted their 5/5/5 assignments independently, we used AI to help check for grammar, standardize the tone, and format the markdown tables for this consolidated document.

Originality Statement: All system features, scenarios, actors, requirements (FRs and NFRs), and use cases are the original intellectual property of Team 10, derived directly from our Lab 2 project scope. No requirements or scenarios were auto-generated from scratch; AI was used strictly for review, refinement, and formatting of our own original work.
