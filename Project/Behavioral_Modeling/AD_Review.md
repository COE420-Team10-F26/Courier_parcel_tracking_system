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
| AD UC-09 Assign Parcels to Field Staff | Mostly yes. The diagram matches the UC-09 description and S-03. The admin sees all parcels with status, zone and staff, and the active staff with workload and zones (FR-08). The admin selects several Pending Pickup parcels and one staff member and submits them in one action (FR-09). Only active staff are shown for selection, so parcels can only be assigned to active field staff accounts (P3). Validation allows only Pending Pickup parcels, or Delayed while Pending Pickup (P3 and the status table, revision R1). For each parcel the status is set to Assigned and a history entry is recorded (FR-18), any active delay alert is resolved (R1), and the customer gets an Assigned notification (FR-09, FR-16, P5 rule 5). The staff member then gets one summary notification with the parcel count (FR-09, FR-17, `<<include>>` UC-18 by R-05). Found: the note required on every diagram with a notification is missing ("send() hands the notification to the notification queue and returns at once; failures are retried for up to 24 h (NFR-18) and never block status-history logging (NFR-19) or the rest of the workflow."), so the diagram does not show that notification failures never block the history entry. |
| AD UC-16 Record Proof of Delivery | _to be filled by Sarhan_ |
| AD UC-19 View Parcel Tracking History | _to be filled by Sarhan_ |

## Q3. Does the diagram contain a clearly identifiable initial node and final node?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | _to be filled by Mohammed_ |
| AD UC-09 Assign Parcels to Field Staff | _to be filled by Mohammed_ |
| AD UC-16 Record Proof of Delivery | _to be filled by Mohammed_ |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q4. Are the actions named clearly and arranged in a logical order?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | _to be filled by Mohammed_ |
| AD UC-09 Assign Parcels to Field Staff | _to be filled by Mohammed_ |
| AD UC-16 Record Proof of Delivery | _to be filled by Mohammed_ |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q5. Are control-flow arrows used correctly?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | _to be filled by Mohammed_ |
| AD UC-09 Assign Parcels to Field Staff | _to be filled by Mohammed_ |
| AD UC-16 Record Proof of Delivery | _to be filled by Mohammed_ |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q6. Are decision branches labeled with meaningful guard conditions?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | _to be filled by Mohammed_ |
| AD UC-09 Assign Parcels to Field Staff | _to be filled by Mohammed_ |
| AD UC-16 Record Proof of Delivery | _to be filled by Mohammed_ |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q7. Do alternative paths lead to appropriate outcomes?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | _to be filled by Mohammed_ |
| AD UC-09 Assign Parcels to Field Staff | _to be filled by Mohammed_ |
| AD UC-16 Record Proof of Delivery | _to be filled by Mohammed_ |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q8. Are merge nodes used appropriately when alternative paths rejoin?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | _to be filled by Mohammed_ |
| AD UC-09 Assign Parcels to Field Staff | _to be filled by Mohammed_ |
| AD UC-16 Record Proof of Delivery | _to be filled by Mohammed_ |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q9. Are fork and join nodes used correctly when parallel activities exist?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | _to be filled by Mohammed_ |
| AD UC-09 Assign Parcels to Field Staff | _to be filled by Mohammed_ |
| AD UC-16 Record Proof of Delivery | _to be filled by Mohammed_ |
| AD UC-19 View Parcel Tracking History | _to be filled by Mohammed_ |

## Q10. Are swimlanes used appropriately to distinguish responsibilities, where needed?
**Owner:** Mohammed

| Diagram | Answer |
| --- | --- |
| AD UC-03 Book Parcel | _to be filled by Mohammed_ |
| AD UC-09 Assign Parcels to Field Staff | _to be filled by Mohammed_ |
| AD UC-16 Record Proof of Delivery | _to be filled by Mohammed_ |
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
