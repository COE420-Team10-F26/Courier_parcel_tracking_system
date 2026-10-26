# Class Model Consistency and Traceability Check

Contributor: Muhammad Hammad Khan (b00094579)

Checked it against the consolidated class diagram, the Lab 3 consolidated requirements and the Lab 4 Core Processes Matrix and DFD (D1 Users, D2 Parcels, D3 Status History). FR and UC numbers are the team-level numbers.

## 1. Consistency with Requirements and Use Cases

| Class ID | Class | Use cases | FRs |
| --- | --- | --- | --- |
| C01 | User (abstract) | UC-02, UC-11 | FR-02, FR-07, FR-10 |
| C02 | Customer | UC-01, UC-03, UC-05 | FR-01, FR-03, FR-05 |
| C03 | Parcel | UC-03, UC-04, UC-05, UC-14 | FR-03, FR-04, FR-05, FR-13, FR-14 |
| C04 | Recipient | UC-03 | FR-03 |
| C06 | Administrator | UC-06 to UC-10 | FR-06 to FR-10 |
| C07 | Assignment | UC-08, UC-09 | FR-08, FR-09 |
| C08 | Zone | UC-08, UC-09, UC-10 | FR-08, FR-09, FR-10 |
| C11 | FieldStaff | UC-12 to UC-16 | FR-11 to FR-15 |
| C12 | DeliveryAttempt | UC-14 | FR-13 |
| C13 | ProofOfDelivery | UC-16 | FR-15 |
| C14 | ParcelStatus (enum) | UC-14, UC-15, UC-16 | FR-13 |
| C16 | StatusHistoryEntry | UC-19, UC-21 | FR-18, FR-19 |
| C17 | Notification (abstract) | UC-17, UC-18 | FR-16, FR-17 |
| C18 | StatusNotification | UC-17 | FR-16 |
| C19 | AssignmentNotification | UC-18 | FR-17, FR-09 |
| C20 | DelayAlert | UC-20 | FR-20 |

Every class traces to at least one use case and FR. UC-12 to UC-16 are only covered once FieldStaff is merged in, since it is still a placeholder. I saw 3 gaps:
- UC-19 and UC-21 (tracking history) have StatusHistoryEntry but no method that returns a parcel's history.
- UC-20 (delay alerts) has DelayAlert but no Administrator method to view alerts.
- UC-12 / FR-11 (view pickups) needs the sender's address, but Parcel only has `senderEmirate`, and neither registration nor booking (FR-01, FR-03) collects a sender address.

## 2. Consistency with Data Stores

| Data store | Classes | Result |
| --- | --- | --- |
| D1 Users | User, Customer, Administrator, FieldStaff, Zone | Yes |
| D2 Parcels | Parcel, Recipient, Assignment, ProofOfDelivery, DeliveryAttempt | Yes. Lab 4 does not say where attempts are stored, so they sit with the parcel record. |
| D3 Status History | StatusHistoryEntry | Yes |
| None | Notification, StatusNotification, AssignmentNotification, DelayAlert | No. These keep state (delivery status, retry time, active flag) but Lab 4 has no data store for them. Fix: add a store to the DFD and matrix, or state they are not stored. |

The Level-0 processes are also covered by the classes:
- P1 Manage Accounts and Login: User, Customer, Administrator, FieldStaff, Zone
- P2 Book Parcel: Customer, Parcel, Recipient
- P3 Assign/Monitor Parcels: Administrator, Assignment, AssignmentNotification, DelayAlert
- P4 Update Parcel Status: FieldStaff, DeliveryAttempt, ProofOfDelivery, Parcel, ParcelStatus, StatusHistoryEntry
- P5 Tracking and Notifications: Customer, Parcel, StatusHistoryEntry, StatusNotification

## 3. Relationships and Multiplicities

| Relationship | Multiplicity | Result | Reason |
| --- | --- | --- | --- |
| User generalizes Customer, Administrator, FieldStaff | n/a | Yes | Genuine generalization: all three share login and account state (UC-02, UC-11). |
| Notification generalizes StatusNotification, AssignmentNotification | n/a | Yes | Genuine generalization: shared content, recipient and retry state (FR-16, FR-17). |
| Customer books Parcel | 1 to 0..* | Yes | One customer, many parcels (FR-03) |
| Parcel is delivered to Recipient (composition) | 1 to 1 | Yes | Recipient exists only within one parcel |
| Administrator creates Assignment | 1 to 0..* | Yes | FR-09 |
| Assignment contains Parcel | 0..1 to 1..* | Yes | Bulk assignment has one or more parcels |
| Assignment is given to FieldStaff | * to 1 | Yes | Each assignment has one staff member |
| FieldStaff works in Zone | * to 1 | Yes | Admin always sets one zone (FR-10) |
| AssignmentNotification summarizes Assignment | 1 to 1 | Yes | One summary per bulk assignment |
| Notification has recipient User | * to 1 | Yes | One recipient each |
| StatusNotification is about Parcel | 0..* to 1 | Yes | Many notifications per parcel |
| StatusHistoryEntry is history of Parcel | 0..* to 1 | Yes | One entry per status change (FR-18) |
| StatusHistoryEntry has actor User | 0..* to 0..1 | Yes | Actor optional for system changes |
| DelayAlert alerts on Parcel | 0..* to 1 | Yes | FR-20 |
| FieldStaff performs DeliveryAttempt | 1 to 0..* | Pending merge | |
| Parcel has attempts DeliveryAttempt | 1 to 0..3 | Pending merge | Max 3 attempts (FR-13) |
| Parcel is confirmed by ProofOfDelivery | 1 to 0..1 | Pending merge | Proof exists only after delivery |
| FieldStaff captures ProofOfDelivery | 1 to 0..* | Pending merge | |
