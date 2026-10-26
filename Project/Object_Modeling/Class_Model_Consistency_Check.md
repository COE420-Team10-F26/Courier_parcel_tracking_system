# Class Model Consistency and Traceability Check

Contributor: Muhammad Hammad Khan (b00094579)

Checked it against the consolidated class diagram, the Lab 3 consolidated requirements and the Lab 4 Core Processes Matrix and DFD (D1 Users, D2 Parcels, D3 Status History). FR and UC numbers are the team-level numbers.

## 1. Consistency with Requirements and Use Cases

| Class ID | Class | Use cases | FRs |
| --- | --- | --- | --- |
| C01 | User (abstract) | UC-02, UC-11 | FR-02, FR-07, FR-10 |
| C02 | Customer | UC-01, UC-03, UC-05, UC-19 | FR-01, FR-03, FR-05, FR-19 |
| C03 | Parcel | UC-03, UC-04, UC-05, UC-14, UC-15, UC-19, UC-21 | FR-03, FR-04, FR-05, FR-13, FR-14, FR-18, FR-19 |
| C04 | Recipient | UC-03 | FR-03 |
| C06 | Administrator | UC-06 to UC-10, UC-20, UC-21 | FR-06 to FR-10, FR-19, FR-20 |
| C07 | Assignment | UC-08, UC-09 | FR-08, FR-09 |
| C08 | Zone | UC-08, UC-09, UC-10 | FR-08, FR-09, FR-10 |
| C11 | FieldStaff | UC-12 to UC-16 | FR-11 to FR-15 |
| C12 | DeliveryAttempt | UC-14 | FR-13 |
| C13 | ProofOfDelivery | UC-16 | FR-15 |
| C14 | ParcelStatus (enum) | UC-14, UC-15, UC-16 | FR-13, FR-14, FR-15 |
| C16 | StatusHistoryEntry | UC-19, UC-21 | FR-18, FR-19 |
| C17 | Notification (abstract) | UC-17, UC-18 | FR-16, FR-17 |
| C18 | StatusNotification | UC-17 | FR-16 |
| C19 | AssignmentNotification | UC-18 | FR-17, FR-09 |
| C20 | DelayAlert | UC-20 | FR-20 |

Every class traces to at least one use case and FR, and UC-12 to UC-16 are covered by the merged FieldStaff class. The check found these gaps, and all were fixed in the consolidated diagram:
- UC-19 (tracking history) had StatusHistoryEntry but no method that returns a parcel's history. Fixed: `Parcel.getStatusHistory(): List<StatusHistoryEntry>`.
- UC-21 (admin looks up history by parcel ID) had no way to find a parcel by its ID. Fixed: `Administrator.searchParcelHistory(parcelId: int): List<StatusHistoryEntry>`.
- UC-20 (delay alerts) had DelayAlert but no Administrator method to view alerts. Fixed: `Administrator.viewDelayAlerts(): List<DelayAlert>`.
- UC-12 / FR-11 (view pickups) needs the sender's address, but Parcel only had `senderEmirate`. Fixed: `Parcel.senderAddress`, and `Customer.bookParcel()` now takes `senderAddress` and `senderEmirate`. FR-03 (Lab 3) and P2 (Lab 4) were updated so booking collects the sender's pickup address and emirate.
- Lab 4 P3 allows reassigning a parcel before pickup, but there was no method for it. Fixed: `Administrator.reassignParcel(parcel: Parcel, staff: FieldStaff): Assignment`.

## 2. Consistency with Data Stores

| Data store | Classes | Result |
| --- | --- | --- |
| D1 Users | User, Customer, Administrator, FieldStaff, Zone | Yes |
| D2 Parcels | Parcel, Recipient, Assignment, ProofOfDelivery, DeliveryAttempt | Yes. Lab 4 does not say where attempts are stored, so they sit with the parcel record. |
| D3 Status History | StatusHistoryEntry | Yes |
| None | Notification, StatusNotification, AssignmentNotification, DelayAlert | Yes, by design. A notification only exists in the notification service's outgoing queue until it is sent or its 24-hour retry window ends (NFR-18), and its ID identifies it while it is queued. A DelayAlert only lives on the admin dashboard while it is active (its ID identifies it there, and resolve() clears it at the next real status update), and it can always be rebuilt from D2 (undelivered, last status change over 48 hours ago). Neither needs a permanent store; a D4 Notifications store would be added only if a notification history were required (see Integration_Notes.md). |

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
| Assignment is given to FieldStaff | 0..* to 1 | Yes | Each assignment has one staff member |
| FieldStaff works in Zone | 0..* to 1 | Yes | Admin always sets one zone (FR-10) |
| AssignmentNotification summarizes Assignment | 1 to 1 | Yes | One summary per bulk assignment |
| Notification is sent to User | 0..* to 1 | Yes | One recipient each |
| StatusNotification is about Parcel | 0..* to 1 | Yes | Many notifications per parcel |
| StatusHistoryEntry records change of Parcel | 0..* to 1 | Yes | One entry per status change (FR-18) |
| StatusHistoryEntry made by User | 0..* to 0..1 | Yes | Actor optional for system changes |
| DelayAlert alerts on Parcel | 0..* to 1 | Yes | FR-20 |
| FieldStaff performs DeliveryAttempt | 1 to 0..* | Yes | One staff member records many attempts; each attempt is made by one staff member (FR-13) |
| Parcel has attempts DeliveryAttempt | 1 to 0..3 | Yes | Max 3 attempts (FR-13) |
| Parcel is confirmed by ProofOfDelivery | 1 to 0..1 | Yes | Proof exists only after delivery |
| FieldStaff captures ProofOfDelivery | 1 to 0..* | Yes | Each proof is captured by the staff member who delivered the parcel (FR-15) |
