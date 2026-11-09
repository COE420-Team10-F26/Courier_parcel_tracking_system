# Activity Diagram Review

Answers to the 12 review questions in the Lab 7 sheet, for each of the four activity diagrams. Each question has one owner, who fills in its table and commits it. If a check finds a problem, write what was found, and after the diagram owner fixes it, update the answer (for example: "found X, fixed Tuesday").

## Q1. Does each Activity Diagram correspond to one of the use cases selected in Lab 6?
**Owner:** Kevin. Check against the Lab 6 sequence diagrams.

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | _to be filled by Kevin_ |
| AD UC-09 Assign Parcels to Field Staff | _to be filled by Kevin_ |
| AD UC-16 Record Proof of Delivery | _to be filled by Kevin_ |
| AD UC-19 View Parcel Tracking History | _to be filled by Kevin_ |

## Q2. Is the workflow consistent with the relevant functional requirements and use case description?
**Owner:** Sarhan. Check against the Lab 3 FRs, NFRs, use cases and scenarios, and the Lab 4 process rules.

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. The diagram matches the UC-03 description and S-01. Validation covers the FR-03 checks and P2's booking rules. On `[invalid]` the errors are shown next to the fields and the entered data is kept (NFR-04), and nothing is created. On `[valid]` the parcel is created as Booked, gets its CPT-XXXXXX tracking number (FR-04, `<<include>>` UC-04 by R-01) and is set to Pending Pickup with a status history entry, as P2 and the status table require (FR-18). The Pending Pickup notification is sent after the history entry (FR-16, P5 rule 5) and carries the NFR-18/NFR-19 note. The confirmation shows the tracking number and the 24 h / 48 h delivery window (FR-03, S-01). Being logged in (UC-02) is a precondition, as in the UC-03 description, so it is not drawn. No problems found. |
| AD UC-09 Assign Parcels to Field Staff | Mostly yes. The diagram matches the UC-09 description and S-03. The admin sees all parcels with status, zone and staff, and the active staff with workload and zones (FR-08). The admin selects several Pending Pickup parcels and one staff member and submits them in one action (FR-09). Only active staff are shown for selection, so parcels can only be assigned to active field staff accounts (P3). Validation allows only Pending Pickup parcels, or Delayed while Pending Pickup (P3 and the Delayed row of the status table). For each parcel the status is set to Assigned and a history entry is recorded (FR-18), any active delay alert is resolved (status table), and the customer gets an Assigned notification (FR-09, FR-16, P5 rule 5). The staff member then gets one summary notification with the parcel count (FR-09, FR-17, `<<include>>` UC-18 by R-05). Found: the note required on every diagram with a notification is missing ("send() hands the notification to the notification queue and returns at once; failures are retried for up to 24 h (NFR-18) and never block status-history logging (NFR-19) or the rest of the workflow."), so the diagram does not show that notification failures never block the history entry. |
| AD UC-16 Record Proof of Delivery | Yes. The diagram matches the UC-16 description and S-04. The staff member picks a parcel from their own assigned list, so only parcels assigned to them can be updated (FR-12, P4). Before the proof is created, the status is checked: only a parcel that is Out for Delivery, or Delayed while Out for Delivery, can be delivered (P4 status order and the Delayed row of the status table), and any other status is rejected, so no step is skipped. The recipient's full name, signature and Emirates ID are captured as a proof of delivery, and the parcel is marked Delivered only if the proof is complete (FR-15, P4); an incomplete proof is discarded and the parcel keeps its status. Delivered is set through UC-14 (`<<include>>` by R-07), a status history entry is recorded (FR-18) and any active delay alert is resolved (status table). The Delivered notification is sent after the history entry (FR-16, P5 rule 5, `<<include>>` UC-17 by R-08) and carries the NFR-18/NFR-19 note. No problems found. |
| AD UC-19 View Parcel Tracking History | _to be filled by Sarhan_ |

## Q3. Does the diagram contain a clearly identifiable initial node and final node?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. One initial node at the top of the Customer lane, and two activity final nodes: one after `[cancel]` (nothing is booked) and one after "View confirmation with tracking number and delivery window". Every path ends at one of them. |
| AD UC-09 Assign Parcels to Field Staff | Yes. One initial node at the top of the Admin lane, and two activity final nodes: one after `[cancel]` in the Admin lane and one after "show confirmation" in the System lane. Every path ends at one of them. |
| AD UC-16 Record Proof of Delivery | Yes. One initial node at the top of the Field Staff lane, and three activity final nodes: after "parcel is not out for delivery", after `[stop]` (the parcel keeps its status), and after the join that follows the Delivered notification and confirmation. Every path ends at one of them. |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q4. Are the actions named clearly and arranged in a logical order?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. Every action is a short verb phrase without the actor's name (for example "Enter pickup, recipient and parcel details", "Validate booking details (FR-03)", "Record status history entry"), and they follow the booking order: enter, submit, validate, create, generate the tracking number, set Pending Pickup, record history, notify and calculate the window, view the confirmation. |
| AD UC-09 Assign Parcels to Field Staff | Yes. Every action is a verb phrase without the actor's name, in a logical order: request the list, show the parcels and staff, select parcels and staff, submit, create and validate the assignment, then per parcel set Assigned, record history, resolve any delay alert and notify the customer, then the summary and the confirmation. Minor: capitalisation is inconsistent ("Select Parcels" and "Submit" against "show confirmation" and "record history"), and the team convention name is "Send ... notification" with a capital S. |
| AD UC-16 Record Proof of Delivery | Yes. Every action is a verb phrase without the actor's name (for example "Select parcel", "Check proof is complete", "Set status to Delivered (updateDeliveryStatus, UC-14)"), in a logical order: select the parcel, enter and submit the proof, check the status, create and check the proof, set Delivered, record history, resolve any delay alert, then notify and confirm. |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q5. Are control-flow arrows used correctly?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. Every control flow has an open arrowhead pointing in the direction of the workflow, every action has one incoming and one outgoing flow, and the flows are drawn horizontally or vertically with no crossings. The dashed lines are note anchors, not control flows. |
| AD UC-09 Assign Parcels to Field Staff | Yes. Every control flow has an arrowhead pointing in the direction of the workflow, and the loop back from "More parcels" and the `[try again]` flow both enter a merge. Minor: a few flows are drawn diagonally (from "Select Parcels", out of "Active delay alert", from "resolve delay alert" and the `[cancel]` flow to the final node); they are still correct but would be easier to read straightened. |
| AD UC-16 Record Proof of Delivery | Yes. Every control flow has an open arrowhead pointing in the direction of the workflow, every action has one incoming and one outgoing flow, and the flows are horizontal or vertical. The `[retry]` flow goes back into the merge before "Enter recipient name, signature and Emirates ID". The dashed line is the note anchor, not a control flow. |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q6. Are decision branches labeled with meaningful guard conditions?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. Both decisions have complementary guards in [ ]: `[valid]` / `[invalid]` after validation, and `[correct and resubmit]` / `[cancel]` after the errors are shown. |
| AD UC-09 Assign Parcels to Field Staff | Partly. `[try again]` / `[cancel]` are written in [ ], and every decision is labeled with its question, so the meaning is clear. Found: the other guards are written without brackets: valid / invalid, yes / no after "Active delay alert" and yes / no after "More parcels". They should be `[valid]` / `[invalid]`, `[active delay alert]` / `[no active delay alert]` and `[more parcels]` / `[all parcels done]`. |
| AD UC-16 Record Proof of Delivery | Yes. All four decisions have complementary guards in [ ]: `[Out for Delivery, or Delayed while Out for Delivery]` / `[any other status]`, `[complete]` / `[incomplete]`, `[retry]` / `[stop]`, and `[active delay alert]` / `[no active delay alert]`. |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q7. Do alternative paths lead to appropriate outcomes?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. `[invalid]` shows the errors and keeps the data, then either returns to the details (`[correct and resubmit]`) or ends with nothing created (`[cancel]`). `[valid]` ends with the booking confirmed and the customer notified. |
| AD UC-09 Assign Parcels to Field Staff | Yes. invalid shows "invalid parcels, assignment not confirmed", then either returns to the start (`[try again]`) or ends (`[cancel]`). valid assigns every parcel, notifies each customer and the staff member, and ends with the confirmation. The delay-alert branches both rejoin before the customer notification. |
| AD UC-16 Record Proof of Delivery | Yes. `[any other status]` rejects the proof and ends. `[incomplete]` discards the proof, then either returns to entering it (`[retry]`) or ends with the parcel keeping its status (`[stop]`). `[complete]` ends with the parcel Delivered, the customer notified and the proof shown. The delay-alert branches both rejoin before the fork. |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q8. Are merge nodes used appropriately when alternative paths rejoin?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. One merge, before "Enter pickup, recipient and parcel details", joins the initial flow and the `[correct and resubmit]` flow. |
| AD UC-09 Assign Parcels to Field Staff | Yes. Three merges: one before "request parcel list" (initial flow and `[try again]`), one before "set next parcel to Assigned" ("confirm assignment" and the loop back from "More parcels"), and one after the delay-alert decision ("resolve delay alert" and the no branch). |
| AD UC-16 Record Proof of Delivery | Yes. Two merges: one before "Enter recipient name, signature and Emirates ID" ("Select parcel" and `[retry]`), and one after the delay-alert decision ("Resolve delay alert" and `[no active delay alert]`). |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q9. Are fork and join nodes used correctly when parallel activities exist?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. One fork after "Record status history entry" and one matching join before "View confirmation". The two branches are genuinely parallel: "Send Pending Pickup notification" hands the notification to the queue and returns at once (see the note), and calculating the delivery window does not depend on it. |
| AD UC-09 Assign Parcels to Field Staff | Yes. There is no fork or join: the summary notification is sent and then the confirmation is shown, in the same order as the Lab 6 sequence diagram, so nothing is drawn as parallel. |
| AD UC-16 Record Proof of Delivery | Yes. One fork after the delay-alert merge and one matching join before the final node. "Send Delivered notification" and "Show confirmation and proof" are genuinely parallel, because send() returns at once (see the note), and both come after the history entry. |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q10. Are swimlanes used appropriately to distinguish responsibilities, where needed?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. Three lanes: Customer (enter, submit, the resubmit-or-cancel choice, view confirmation), System (validate, show errors, create, tracking number, status, history, delivery window) and Notification (send the Pending Pickup notification). |
| AD UC-09 Assign Parcels to Field Staff | Mostly. Admin, System and Notification lanes each hold the right actions: the Admin selects and submits and chooses to try again, the System shows, validates and updates, and the Notification lane sends both notifications. Found: the System lane is titled "System (Administrator)", which reads as if the Administrator performs the system actions; it should be titled "System". |
| AD UC-16 Record Proof of Delivery | Yes. Three lanes: Field Staff (select, enter, submit, the retry-or-stop choice), System (show, check, create, set status, record history, resolve the alert, show the confirmation) and Notification (send the Delivered notification). Found: the "Retry?" decision was in the System lane although retrying is the staff member's choice; fixed by moving it, with its `[stop]` final node, to the Field Staff lane. |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q11. Are the modeled activities consistent with the system's intended behavior and the other project models?
**Owner:** Kevin. Check against the Lab 6 sequence diagrams and the Lab 5 class operations.

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | _to be filled by Kevin_ |
| AD UC-09 Assign Parcels to Field Staff | _to be filled by Kevin_ |
| AD UC-16 Record Proof of Delivery | _to be filled by Kevin_ |
| AD UC-19 View Parcel Tracking History | _to be filled by Kevin_ |

## Q12. Are there any missing activities, unnecessary steps, contradictory paths, or unreachable actions?
**Owner:** Kevin. Use Sarhan's Q2 findings for the missing activities part.

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | _to be filled by Kevin_ |
| AD UC-09 Assign Parcels to Field Staff | _to be filled by Kevin_ |
| AD UC-16 Record Proof of Delivery | _to be filled by Kevin_ |
| AD UC-19 View Parcel Tracking History | _to be filled by Kevin_ |
