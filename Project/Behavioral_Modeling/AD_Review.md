# Activity Diagram Review

Answers to the 12 review questions in the Lab 7 sheet, for each of the four activity diagrams. Each question has one owner, who fills in its table and commits it. If a check finds a problem, write what was found, and after the diagram owner fixes it, update the answer (for example: "found X, fixed Tuesday").

## Q1. Does each Activity Diagram correspond to one of the use cases selected in Lab 6?
**Owner:** Kevin. Check against the Lab 6 sequence diagrams. The corresponding sequence diagrams are placed within the report document under Documents/COE420_Lab6_Team10.docx

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. UC-03 Book Parcel is one of the four use cases selected in Lab 6 (Table 1.1, Customer as primary actor). It has its own sequence diagram (Figure 2.1), so the activity diagram models the same use case. |
| AD UC-09 Assign Parcels to Field Staff | Yes. UC-09 Assign Parcels to Field Staff is one of the four selected use cases (Table 1.1, Admin as primary actor). It has its own sequence diagram (Figure 2.2), so the activity diagram models the same use case. |
| AD UC-16 Record Proof of Delivery | Yes. UC-16 Record Proof of Delivery is one of the four selected use cases (Table 1.1, Field Staff as primary actor). It has its own sequence diagram (Figure 2.3), so the activity diagram models the same use case. |
| AD UC-19 View Parcel Tracking History | Yes. UC-19 View Parcel Tracking History is one of the four selected use cases (Table 1.1, Customer as primary actor). It has its own sequence diagram (Figure 2.4), so the activity diagram models the same use case. |

## Q2. Is the workflow consistent with the relevant functional requirements and use case description?
**Owner:** Sarhan. Check against the Lab 3 FRs, NFRs, use cases and scenarios, and the Lab 4 process rules.

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. The diagram matches the UC-03 description and S-01. Validation covers the FR-03 checks and P2's booking rules. On `[invalid]` the errors are shown next to the fields and the entered data is kept (NFR-04), and nothing is created. On `[valid]` the parcel is created as Booked, gets its CPT-XXXXXX tracking number (FR-04, `<<include>>` UC-04 by R-01) and is set to Pending Pickup with a status history entry, as P2 and the status table require (FR-18). The Pending Pickup notification is sent after the history entry (FR-16, P5 rule 5) and carries the NFR-18/NFR-19 note. The confirmation shows the tracking number and the 24 h / 48 h delivery window (FR-03, S-01). Being logged in (UC-02) is a precondition, as in the UC-03 description, so it is not drawn. No problems found. |
| AD UC-09 Assign Parcels to Field Staff | Yes, after a fix. The diagram matches the UC-09 description and S-03. The admin sees all parcels with status, zone and staff, and the active staff with workload and zones (FR-08). The admin selects several Pending Pickup parcels and one staff member and submits them in one action (FR-09). Only active staff are shown for selection, so parcels can only be assigned to active field staff accounts (P3). Validation allows only Pending Pickup parcels, or Delayed while Pending Pickup (P3 and the Delayed row of the status table). For each parcel the status is set to Assigned and a history entry is recorded (FR-18), any active delay alert is resolved (status table), and the customer gets an Assigned notification (FR-09, FR-16, P5 rule 5). The staff member then gets one summary notification with the parcel count (FR-09, FR-17, `<<include>>` UC-18 by R-05). Found: the note required on every diagram with a notification was missing ("send() hands the notification to the notification queue and returns at once; failures are retried for up to 24 h (NFR-18) and never block status-history logging (NFR-19) or the rest of the workflow."), so the diagram did not show that notification failures never block the history entry. Fixed: the note is now on the diagram, attached to "Send 1 summary notification to the staff"; its text covers both send actions. |
| AD UC-16 Record Proof of Delivery | Yes. The diagram matches the UC-16 description and S-04. The staff member picks a parcel from their own assigned list, so only parcels assigned to them can be updated (FR-12, P4). Before the proof is created, the status is checked: only a parcel that is Out for Delivery, or Delayed while Out for Delivery, can be delivered (P4 status order and the Delayed row of the status table), and any other status is rejected, so no step is skipped. The recipient's full name, signature and Emirates ID are captured as a proof of delivery, and the parcel is marked Delivered only if the proof is complete (FR-15, P4); an incomplete proof is discarded and the parcel keeps its status. Delivered is set through UC-14 (`<<include>>` by R-07), a status history entry is recorded (FR-18) and any active delay alert is resolved (status table). The Delivered notification is sent after the history entry (FR-16, P5 rule 5, `<<include>>` UC-17 by R-08) and carries the NFR-18/NFR-19 note. No problems found. |
| AD UC-19 View Parcel Tracking History | Yes. The diagram matches the UC-19 description. The customer enters a tracking number, and the system searches only the customer's own parcels, so a number that does not exist and a parcel that belongs to someone else both get "parcel not found" and nothing else (P5 rules 1 and 2). The customer can then try again or stop. When the parcel is found, its complete status history is shown in chronological order, with status, date/time and location or staff for each entry (FR-19, P5 rule 4, the entries recorded under FR-18), which also covers the history review in S-05. Nothing in the workflow sets a status or records history, as the note on the diagram says (P5 rule 7), so there is correctly no notification. Being logged in (UC-02) is a precondition, so it is not drawn. No problems found. |

## Q3. Does the diagram contain a clearly identifiable initial node and final node?
**Owner:** Sarhan (taken over from Mohammed)

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. One initial node at the top of the Customer lane, and two activity final nodes: one after `[cancel]` (nothing is booked) and one after "View confirmation with tracking number and delivery window". Every path ends at one of them. |
| AD UC-09 Assign Parcels to Field Staff | Yes. One initial node at the top of the Admin lane, and two activity final nodes: one after `[cancel]` in the Admin lane and one after "Show confirmation" in the System lane. Every path ends at one of them. |
| AD UC-16 Record Proof of Delivery | Yes. One initial node at the top of the Field Staff lane, and three activity final nodes: after "parcel is not out for delivery", after `[stop]` (the parcel keeps its status), and after the join that follows the Delivered notification and confirmation. Every path ends at one of them. |
| AD UC-19 View Parcel Tracking History | Yes. One initial node at the top of the Customer lane, and two activity final nodes in the Customer lane: one after `[done]` and one after "View full status history". Every path ends at one of them. |

## Q4. Are the actions named clearly and arranged in a logical order?
**Owner:** Sarhan (taken over from Mohammed)

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. Every action is a short verb phrase without the actor's name (for example "Enter pickup, recipient and parcel details", "Validate booking details (FR-03)", "Record status history entry"), and they follow the booking order: enter, submit, validate, create, generate the tracking number, set Pending Pickup, record history, notify and calculate the window, view the confirmation. |
| AD UC-09 Assign Parcels to Field Staff | Yes. Every action is a verb phrase without the actor's name, in a logical order: request the list, show the parcels and staff, select parcels and staff, submit, create and validate the assignment, then per parcel set Assigned, record history, resolve any delay alert and notify the customer, then the summary and the confirmation. Found: capitalisation was inconsistent ("Select Parcels" against "show confirmation" and "record history"), and the notification actions did not use the team name "Send ... notification". Fixed: every action now starts with a capital letter, "record history" is now "Record status history entry", and the notifications are "Send Assigned notification to the customer" and "Send 1 summary notification to the staff". |
| AD UC-16 Record Proof of Delivery | Yes. Every action is a verb phrase without the actor's name (for example "Select parcel", "Check proof is complete", "Set status to Delivered (updateDeliveryStatus, UC-14)"), in a logical order: select the parcel, enter and submit the proof, check the status, create and check the proof, set Delivered, record history, resolve any delay alert, then notify and confirm. |
| AD UC-19 View Parcel Tracking History | Yes. Every action is a short verb phrase without the actor's name (for example "Enter tracking number", "Get status history (oldest first)", "View full status history (status, date/time, location or staff)"), and the order is logical: enter, find, get the history, get each entry's text, view. Found: the note about getStatusHistory() was anchored to "Get next entry's transition text" instead of the action it describes. Fixed: it is now anchored to "Get status history (oldest first)". |

## Q5. Are control-flow arrows used correctly?
**Owner:** Sarhan (taken over from Mohammed)

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. Every control flow has an open arrowhead pointing in the direction of the workflow, every action has one incoming and one outgoing flow, and the flows are drawn horizontally or vertically with no crossings. The dashed lines are note anchors, not control flows. |
| AD UC-09 Assign Parcels to Field Staff | Yes. Every control flow has an arrowhead pointing in the direction of the workflow, and the loop back from "More parcels" and the `[try again]` flow both enter a merge. Minor: a few flows are drawn diagonally (from "Select parcels", out of "Active delay alert", from "Resolve delay alert" and the `[cancel]` flow to the final node); they are still correct but would be easier to read straightened. |
| AD UC-16 Record Proof of Delivery | Yes. Every control flow has an open arrowhead pointing in the direction of the workflow, every action has one incoming and one outgoing flow, and the flows are horizontal or vertical. The `[retry]` flow goes back into the merge before "Enter recipient name, signature and Emirates ID". The dashed line is the note anchor, not a control flow. |
| AD UC-19 View Parcel Tracking History | Yes. Every control flow has an open arrowhead pointing in the direction of the workflow and is drawn horizontally or vertically. The `[try again]` flow enters the first merge and the `[more entries]` flow enters the loop merge. The dashed line is the note anchor, not a control flow. |

## Q6. Are decision branches labeled with meaningful guard conditions?
**Owner:** Sarhan (taken over from Mohammed)

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. Both decisions have complementary guards in [ ]: `[valid]` / `[invalid]` after validation, and `[correct and resubmit]` / `[cancel]` after the errors are shown. |
| AD UC-09 Assign Parcels to Field Staff | Yes, after a fix. Every decision is labeled with its question and has complementary guards. Found: apart from `[try again]` / `[cancel]`, the guards were written without brackets (valid / invalid, yes / no after "Active delay alert" and yes / no after "More parcels"). Fixed: they are now `[valid]` / `[invalid]`, `[active delay alert]` / `[no active delay alert]` and `[more parcels]` / `[all parcels done]`. |
| AD UC-16 Record Proof of Delivery | Yes. All four decisions have complementary guards in [ ]: `[Out for Delivery, or Delayed while Out for Delivery]` / `[any other status]`, `[complete]` / `[incomplete]`, `[retry]` / `[stop]`, and `[active delay alert]` / `[no active delay alert]`. |
| AD UC-19 View Parcel Tracking History | Yes. All three decisions have complementary guards in [ ]: `[found and belongs to customer]` / `[not found or not own parcel]`, `[try again]` / `[done]`, and `[more entries]` / `[no more entries]`. |

## Q7. Do alternative paths lead to appropriate outcomes?
**Owner:** Sarhan (taken over from Mohammed)

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. `[invalid]` shows the errors and keeps the data, then either returns to the details (`[correct and resubmit]`) or ends with nothing created (`[cancel]`). `[valid]` ends with the booking confirmed and the customer notified. |
| AD UC-09 Assign Parcels to Field Staff | Yes. `[invalid]` shows "invalid parcels, assignment not confirmed", then either returns to the start (`[try again]`) or ends (`[cancel]`). `[valid]` assigns every parcel, notifies each customer and the staff member, and ends with the confirmation. The delay-alert branches both rejoin before the customer notification. |
| AD UC-16 Record Proof of Delivery | Yes. `[any other status]` rejects the proof and ends. `[incomplete]` discards the proof, then either returns to entering it (`[retry]`) or ends with the parcel keeping its status (`[stop]`). `[complete]` ends with the parcel Delivered, the customer notified and the proof shown. The delay-alert branches both rejoin before the fork. |
| AD UC-19 View Parcel Tracking History | Yes. `[not found or not own parcel]` shows "parcel not found" and nothing else, then either returns to entering a tracking number (`[try again]`) or ends (`[done]`). `[found and belongs to customer]` ends with the customer viewing the full history. |

## Q8. Are merge nodes used appropriately when alternative paths rejoin?
**Owner:** Sarhan (taken over from Mohammed)

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. One merge, before "Enter pickup, recipient and parcel details", joins the initial flow and the `[correct and resubmit]` flow. |
| AD UC-09 Assign Parcels to Field Staff | Yes. Three merges: one before "Request parcel list" (initial flow and `[try again]`), one before "Set next parcel to Assigned" ("Confirm assignment" and the `[more parcels]` loop), and one after the delay-alert decision ("Resolve delay alert" and `[no active delay alert]`). |
| AD UC-16 Record Proof of Delivery | Yes. Two merges: one before "Enter recipient name, signature and Emirates ID" ("Select parcel" and `[retry]`), and one after the delay-alert decision ("Resolve delay alert" and `[no active delay alert]`). |
| AD UC-19 View Parcel Tracking History | Yes. Two merges: one before "Enter tracking number" joins the initial flow and `[try again]`, and the loop merge before "Get next entry's transition text" joins the flow from "Get status history (oldest first)" and `[more entries]`. |

## Q9. Are fork and join nodes used correctly when parallel activities exist?
**Owner:** Sarhan (taken over from Mohammed)

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. One fork after "Record status history entry" and one matching join before "View confirmation". The two branches are genuinely parallel: "Send Pending Pickup notification" hands the notification to the queue and returns at once (see the note), and calculating the delivery window does not depend on it. |
| AD UC-09 Assign Parcels to Field Staff | Yes. There is no fork or join: the summary notification is sent and then the confirmation is shown, in the same order as the Lab 6 sequence diagram, so nothing is drawn as parallel. |
| AD UC-16 Record Proof of Delivery | Yes. One fork after the delay-alert merge and one matching join before the final node. "Send Delivered notification" and "Show confirmation and proof" are genuinely parallel, because send() returns at once (see the note), and both come after the history entry. |
| AD UC-19 View Parcel Tracking History | Yes. There is no fork or join, which is correct: each step needs the result of the one before it, and nothing is genuinely parallel. A note on the diagram explains this (read-only lookup, P5 rule 7). |

## Q10. Are swimlanes used appropriately to distinguish responsibilities, where needed?
**Owner:** Sarhan (taken over from Mohammed)

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. Three lanes: Customer (enter, submit, the resubmit-or-cancel choice, view confirmation), System (validate, show errors, create, tracking number, status, history, delivery window) and Notification (send the Pending Pickup notification). |
| AD UC-09 Assign Parcels to Field Staff | Yes, after a fix. Admin, System and Notification lanes each hold the right actions: the Admin selects and submits and chooses to try again, the System shows, validates and updates, and the Notification lane sends both notifications. Found: the System lane was titled "System (Administrator)", which reads as if the Administrator performs the system actions. Fixed: it is now titled "System". |
| AD UC-16 Record Proof of Delivery | Yes. Three lanes: Field Staff (select, enter, submit, the retry-or-stop choice), System (show, check, create, set status, record history, resolve the alert, show the confirmation) and Notification (send the Delivered notification). Found: the "Retry?" decision was in the System lane although retrying is the staff member's choice; fixed by moving it, with its `[stop]` final node, to the Field Staff lane. |
| AD UC-19 View Parcel Tracking History | Yes. Two lanes: Customer (enter the tracking number, the try-again-or-done choice, view the history) and System (find the parcel, show "parcel not found", get the history, get each entry's text). There is no Notification lane because no notification is sent, as the note says. |

## Q11. Are the modeled activities consistent with the system's intended behavior and the other project models?
**Owner:** Kevin. Check against the Lab 6 sequence diagrams and the Lab 5 class operations.

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | Yes. The order matches the Lab 6 UC-03 sequence diagram: validate, create the parcel, generate the tracking number, set Pending Pickup (history entry before the notification), calculate the delivery window, show the confirmation. `[invalid]` creates nothing, as in the `[else]` branch. The actions map to Lab 5 operations (`bookParcel()`, `isValidPhone()`, `generateTrackingNumber()`, `updateStatus()`, `getDeliveryWindowHours()`, `send()`). The sequence diagram's `[active delay alert]` check is not drawn, which is fine because it is always false for a new parcel. No problems found. |
| AD UC-09 Assign Parcels to Field Staff | Yes. The order matches the Lab 6 UC-09 sequence diagram: view parcels and workload, select an active staff member, create and validate the assignment, confirm it, then per parcel set Assigned and run the status-change block (history, delay alert, customer notification). Then one summary notification goes to the staff member and the confirmation is shown. `[invalid]` does not confirm the assignment, as in the `[else]` branch. The actions map to Lab 5 operations (`assignParcels()`, `addParcel()`, `validateParcels()`, `confirmAssignment()`, `updateStatus()`, `buildSummary()`). No problems found. |
| AD UC-16 Record Proof of Delivery | Yes. The order matches the Lab 6 UC-16 sequence diagram: pick the parcel, submit the proof, check the status, create and check the proof (`isComplete()`), set Delivered through UC-14 (`updateDeliveryStatus()`), then run the status-change block. An invalid status or an incomplete proof leaves the parcel unchanged, as in the two `[else]` branches. The actions map to Lab 5 operations of `FieldStaff`, `ProofOfDelivery` and `Parcel`. No problems found. |
| AD UC-19 View Parcel Tracking History | Yes. The order matches the Lab 6 UC-19 sequence diagram: enter the tracking number (`trackParcel()`), find the parcel among the customer's own parcels (`viewMyParcels()`), get the history (`getStatusHistory()`), get each entry's text in chronological order (`getTransition()`), then view it. A number that does not exist and a parcel owned by someone else both end in "parcel not found", as in the `[else]` branch. Nothing changes the parcel's status and no notification is sent, as in the sequence diagram. The actions map to Lab 5 operations (`Customer.trackParcel()`, `Customer.viewMyParcels()`, `Parcel.getStatusHistory()`, `StatusHistoryEntry.getTransition()`). The try-again-or-done choice is user-side behavior that the sequence diagram does not draw, and it does not contradict it. No problems found. |

## Q12. Are there any missing activities, unnecessary steps, contradictory paths, or unreachable actions?
**Owner:** Kevin. Use Sarhan's Q2 findings for the missing activities part.

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | No. Missing activities: none, as Q2 found no problems. Unnecessary steps: none, every action comes from FR-03, FR-04, FR-16, FR-18 or NFR-04. Contradictory paths: none, both decisions have complementary guards. Unreachable actions: none, every path ends at one of the two final nodes (Q3, Q7). |
| AD UC-09 Assign Parcels to Field Staff | No, after a fix. Missing activities: Q2 found the notification note missing, and it is now on the diagram. Nothing else is missing. Unnecessary steps: none, every action comes from FR-08, FR-09, FR-16, FR-17, FR-18 or the Delayed row of the status table. Contradictory paths: none, all guards are complementary (Q6). Unreachable actions: none, every path ends at one of the two final nodes (Q3, Q7). |
| AD UC-16 Record Proof of Delivery | No. Missing activities: none, as Q2 found no problems. Unnecessary steps: none, every action comes from FR-12, FR-15, FR-16, FR-18, P4 or the Delayed row of the status table. Contradictory paths: none, all four decisions have complementary guards (Q6). Unreachable actions: none, every path ends at one of the three final nodes (Q3, Q7). |
| AD UC-19 View Parcel Tracking History | No. Missing activities: none, as Q2 found no problems. Unnecessary steps: none, every action comes from FR-18, FR-19 or P5 rules 1, 2, 4 and 7. Contradictory paths: none, all three decisions have complementary guards (Q6). Unreachable actions: none, every path ends at one of the two final nodes (Q3, Q7). |
