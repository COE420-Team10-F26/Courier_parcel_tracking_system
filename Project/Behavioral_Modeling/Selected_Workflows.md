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
- **Main flow:** _to be filled by Sarhan_
- **Decisions and guards:** _to be filled by Sarhan_
- **Merges:** _to be filled by Sarhan_
- **Forks and joins:** _to be filled by Sarhan_
- **Lab 6 sequence diagram:** `Sequence_Diagrams/SD_UC-03_Book_Parcel.png`
- **Activity diagram:** `Activity_Diagrams/AD_UC-03_Book_Parcel.pdf`

## UC-09 Assign Parcels to Field Staff (Kevin Alex, b00098368)
- **Primary actor:** Admin
- **Swimlanes:** Admin, System, Notification
- **Main flow:** The Admin requests the parcel list. The System shows all parcels with status, zone and staff, then the unassigned Pending Pickup parcels. The Admin selects parcels. The System shows the active staff with workload and zones. The Admin selects staff and submits. The System creates the assignment, adds the parcels and validates them. If all parcels are valid, the System confirms the assignment. For each parcel the System sets the status to Assigned and records the status history entry. If there is an active delay alert, the System resolves it. The Notification lane then sends the Assigned notification to the customer. When no parcels remain, the System gets the parcel count and the assigned staff. The Notification lane sends 1 summary notification to the staff. The System shows the confirmation to the Admin and the flow ends.
- **Decisions and guards:** `all parcels valid` has two branches, `valid` and `invalid`. The `invalid` branch shows "invalid parcels, assignment not confirmed" and goes to the `Try again` decision, with the guards `[try again]` and `[cancel]`. `Active delay alert` has the guards `yes` (resolve the alert) and `no`. `More parcels` has the guards `yes` (next parcel) and `no` (staff summary).
- **Merges:** There are three merges. The first rejoins the initial flow and `[try again]` before "request parcel list". The second rejoins the `valid` path and the `More parcels` `yes` loop before "set next parcel to Assigned". The third rejoins the alert-resolved and no-alert paths before the customer notification.
- **Forks and joins:** None. The staff summary notification is sent before the confirmation is shown to the Admin, as in the Lab 6 sequence diagram, so the two steps are sequential.
- **Lab 6 sequence diagram:** `Sequence_Diagrams/SD_UC-09_Assign_Parcels_to_Field_Staff.png`
- **Activity diagram:** `Activity_Diagrams/AD_UC-09_Assign_Parcels_to_Field_Staff.drawio.png`

## UC-16 Record Proof of Delivery (Muhammad Hammad Khan, b00094579)
- **Primary actor:** Field Staff
- **Swimlanes:** Field Staff, System, Notification
- **Main flow:** _to be filled by Mohammed_
- **Decisions and guards:** _to be filled by Mohammed_
- **Merges:** _to be filled by Mohammed_
- **Forks and joins:** _to be filled by Mohammed_
- **Lab 6 sequence diagram:** `Sequence_Diagrams/SD_UC-16_Record_Proof_of_Delivery.png`
- **Activity diagram:** `Activity_Diagrams/AD_UC-16_Record_Proof_of_Delivery.pdf`

## UC-19 View Parcel Tracking History (Ahmed El Sabagh, b00095506)
- **Primary actor:** Customer
- **Swimlanes:** Customer, System
- **Main flow:** _to be filled by Ahmed_
- **Decisions and guards:** _to be filled by Ahmed_
- **Merges:** _to be filled by Ahmed_
- **Forks and joins:** _to be filled by Ahmed_
- **Lab 6 sequence diagram:** `Sequence_Diagrams/SD_UC-19_View_Parcel_Tracking_History.png`
- **Activity diagram:** `Activity_Diagrams/AD_UC-19_View_Parcel_Tracking_History.pdf`
