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
- **Main flow:** _to be filled by Kevin_
- **Decisions and guards:** _to be filled by Kevin_
- **Merges:** _to be filled by Kevin_
- **Forks and joins:** _to be filled by Kevin_
- **Lab 6 sequence diagram:** `Sequence_Diagrams/SD_UC-09_Assign_Parcels_to_Field_Staff.png`
- **Activity diagram:** `Activity_Diagrams/AD_UC-09_Assign_Parcels_to_Field_Staff.pdf`

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
