# Selected Workflows

Lab 7 models the same four use cases selected in Lab 6 (see `Selected_Use_Cases.md`) as UML activity diagrams, one per member. Each activity diagram gives a workflow view of the same behavior shown in the matching Lab 6 sequence diagram.

Shared conventions used in all four diagrams:
- Swimlanes are the actor, System, and Notification (the StatusNotification / AssignmentNotification classes). UC-19 has only Customer and System.
- A status change is drawn as: set status -> record status history entry. In UC-09 and UC-16 this is followed by a decision `[active delay alert]` / `[no active delay alert]` that rejoins at a merge. A Delayed parcel still counts as the status it had before the delay (Lab 3 status table), so it can still be assigned or delivered and its alert is resolved at that update. UC-03 has no delay decision, because a newly created parcel cannot have a delay alert.
- Notification actions are named "Send ... notification" and carry the note: "send() hands the notification to the notification queue and returns at once; failures are retried for up to 24 h (NFR-18) and never block status-history logging (NFR-19) or the rest of the workflow."

Each member fills in their own section below.

## UC-03 Book Parcel (Sarhan Alzaabi, b00100669)
- **Primary actor:** Customer
- **Swimlanes:** Customer, System, Notification
- **Main flow:** The Customer enters the pickup, recipient and parcel details and submits the booking. The System validates the details (FR-03). If they are valid, the System creates the parcel with status Booked, generates its tracking number (`<<include>>` UC-04, FR-04), sets the status to Pending Pickup and records the status history entry (P2). The flow then forks: the Notification lane sends the Pending Pickup notification to the customer (FR-16) while the System calculates the expected delivery window (24 h if the recipient is in the same emirate as the sender, 48 h if not). After the join, the Customer views the confirmation with the tracking number and delivery window and the flow ends.
- **Decisions and guards:** After "Validate booking details": `[valid]` creates the parcel and `[invalid]` shows the errors next to the fields and keeps the entered data (NFR-04). After the errors are shown, the Customer decides: `[correct and resubmit]` goes back to entering the details and `[cancel]` ends the flow with nothing created. There is no `[active delay alert]` decision, because a newly created parcel cannot have a delay alert (a note on the diagram explains this).
- **Merges:** One merge, before "Enter pickup, recipient and parcel details". It joins the initial flow and the `[correct and resubmit]` path.
- **Forks and joins:** One fork after "Record status history entry" and one join before "View confirmation". Sending the Pending Pickup notification and calculating the delivery window run in parallel: send() hands the notification to the queue and returns at once (NFR-18, NFR-19), and the window does not depend on it. The history entry is recorded before the fork, so the notification always follows it (P5 rule 5).
- **Lab 6 sequence diagram:** `Sequence_Diagrams/SD_UC-03_Book_Parcel.png`
- **Activity diagram:** `Activity_Diagrams/AD_UC-03_Book_Parcel.pdf`

## UC-09 Assign Parcels to Field Staff (Kevin Alex, b00098368)
- **Primary actor:** Admin
- **Swimlanes:** Admin, System, Notification
- **Main flow:** The Admin requests the parcel list. The System shows all parcels with status, zone and staff, then the unassigned Pending Pickup parcels. The Admin selects parcels. The System shows the active staff with workload and zones. The Admin selects staff and submits. The System creates the assignment, adds the parcels and validates them. If all parcels are valid, the System confirms the assignment. For each parcel the System sets the status to Assigned and records the status history entry. If there is an active delay alert, the System resolves it. The Notification lane then sends the Assigned notification to the customer. When no parcels remain, the System gets the parcel count and the assigned staff. The Notification lane sends 1 summary notification to the staff. The System shows the confirmation to the Admin and the flow ends.
- **Decisions and guards:** `all parcels valid` has two branches, `[valid]` and `[invalid]`. The `[invalid]` branch shows "invalid parcels, assignment not confirmed" and goes to the `Try again` decision, with the guards `[try again]` and `[cancel]`. `Active delay alert` has the guards `[active delay alert]` (resolve the alert) and `[no active delay alert]`. `More parcels` has the guards `[more parcels]` (next parcel) and `[all parcels done]` (staff summary).
- **Merges:** There are three merges. The first rejoins the initial flow and `[try again]` before "Request parcel list". The second rejoins the `[valid]` path and the `[more parcels]` loop before "Set next parcel to Assigned". The third rejoins the alert-resolved and no-alert paths before the customer notification.
- **Forks and joins:** None. The staff summary notification is sent before the confirmation is shown to the Admin, as in the Lab 6 sequence diagram, so the two steps are sequential.
- **Lab 6 sequence diagram:** `Sequence_Diagrams/SD_UC-09_Assign_Parcels_to_Field_Staff.png`
- **Activity diagram:** `Activity_Diagrams/AD_UC-09_Assign_Parcels_to_Field_Staff.pdf`

## UC-16 Record Proof of Delivery (Muhammad Hammad Khan, b00094579)
- **Primary actor:** Field Staff
- **Swimlanes:** Field Staff, System, Notification
- **Main flow:** The Field Staff member requests their assigned parcels, the System shows them, and the staff member selects one (FR-12, P4). The staff member enters the recipient's full name, signature and Emirates ID and submits them, and the System checks the parcel's status. If the parcel is Out for Delivery, or Delayed while Out for Delivery, the System creates the proof of delivery and checks that it is complete. If it is, the System sets the status to Delivered through updateDeliveryStatus (`<<include>>` UC-14, R-07), records the status history entry (FR-18) and resolves any active delay alert (Delayed row of the status table). The flow then forks: the Notification lane sends the Delivered notification to the customer (FR-16) while the System shows the confirmation and proof. After the join the flow ends.
- **Decisions and guards:** After "Check parcel status": `[Out for Delivery, or Delayed while Out for Delivery]` continues, and `[any other status]` shows "parcel is not out for delivery" and ends, so no status step is skipped (P4). After "Check proof is complete": `[complete]` sets Delivered, and `[incomplete]` discards the proof and shows "proof incomplete" (FR-15). The staff member then chooses `[retry]`, back to entering the proof, or `[stop]`, which ends with the parcel keeping its status. After "Record status history entry": `[active delay alert]` resolves the alert and `[no active delay alert]` skips it.
- **Merges:** Two merges. One before "Enter recipient name, signature and Emirates ID" joins "Select parcel" and the `[retry]` path. One after the delay-alert decision joins "Resolve delay alert" and the `[no active delay alert]` path.
- **Forks and joins:** One fork after the delay-alert merge and one join before the final node. Sending the Delivered notification and showing the confirmation and proof run in parallel: send() hands the notification to the queue and returns at once (NFR-18, NFR-19), and both come after the history entry (P5 rule 5).
- **Lab 6 sequence diagram:** `Sequence_Diagrams/SD_UC-16_Record_Proof_of_Delivery.png`
- **Activity diagram:** `Activity_Diagrams/AD_UC-16_Record_Proof_of_Delivery.pdf`

## UC-19 View Parcel Tracking History (Ahmed El Sabagh, b00095506)
- **Primary actor:** Customer
- **Swimlanes:** Customer, System. There is no Notification lane, because this use case only reads data and never changes a status.
- **Main flow:** The Customer enters a tracking number. The System looks for it among the customer's own parcels (`viewMyParcels()`). If the parcel is found and belongs to the customer, the System gets its status history in chronological order (`getStatusHistory()`, oldest first) and then, for each entry, gets the text of that status change (`getTransition()`). When there are no more entries, the Customer views the full status history (status, date/time, and location or staff for each entry) and the flow ends.
- **Decisions and guards:** After "Find the parcel in the customer's own parcels": `[found and belongs to customer]` gets the status history and `[not found or not own parcel]` shows "parcel not found" (P5 rules 1 and 2). After the message is shown, the Customer decides: `[try again]` goes back to entering a tracking number and `[done]` ends the flow. In the loop, `[more entries]` goes back to get the next entry's transition text and `[no more entries]` goes on to show the full history.
- **Merges:** Two merges. The first is before "Enter tracking number" and joins the initial flow and the `[try again]` path. The second is the loop merge before "Get next entry's transition text" and joins the path from "Get status history (oldest first)" and the `[more entries]` path.
- **Forks and joins:** None. The workflow is a read-only lookup (P5 rule 7): it sets no status and records no history entry, so there is no status change, no delay-alert decision and no notification. Each step needs the result of the step before it (the history needs the parcel, each transition text needs its entry, and the display needs all of them), so nothing in it is genuinely parallel (Q9). A note on the diagram says this.
- **Lab 6 sequence diagram:** `Sequence_Diagrams/SD_UC-19_View_Parcel_Tracking_History.png`
- **Activity diagram:** `Activity_Diagrams/AD_UC-19_View_Parcel_Tracking_History.pdf`
