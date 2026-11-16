# Class Distribution

**Project:** Courier Parcel Tracking System (Team 10)
**Lab:**     8

All 16 classes from the consolidated Lab 5 class diagram are assigned to exactly one primary component. The architecture has six components in a layered style: Application (CO1–CO3), Domain (CO4) and Services (CO5–CO6).

## Class Distribution Table

| ID | Component Name | Layer | Responsibilities | Included Classes |
| --- | --- | --- | --- | --- |
| CO1 | Account Management | Application | Registration, login/logout, passwords and account locking | User, Customer |
| CO2 | Administration & Assignment | Application | Managing users and zones, assigning/reassigning parcels to field staff, workload | Administrator, Assignment, Zone |
| CO3 | Field Delivery | Application | Pickups, deliveries, status updates, failed attempts and proof of delivery | FieldStaff, DeliveryAttempt, ProofOfDelivery |
| CO4 | Parcel Management | Domain | Booking, tracking numbers, recipient details, status changes, delivery window and the 48h delay check | Parcel, Recipient |
| CO5 | Tracking & Monitoring | Services | Parcel statuses, status history and delay alerts | StatusHistoryEntry, DelayAlert, ParcelStatus |
| CO6 | Notification | Services | Building and sending status and assignment notifications with retries | Notification, StatusNotification, AssignmentNotification |

## Why Each Class Is in Its Component

| Class | Component | Reason |
| --- | --- | --- |
| User | CO1 | Base account class holding login credentials and account state, which CO1 manages. |
| Customer | CO1 | A user type that registers and logs in through the same account functions. |
| Administrator | CO2 | The user who manages users and zones and assigns parcels, so it sits with the administration functions. |
| Assignment | CO2 | Links a parcel to a field staff member and is created or changed when the administrator assigns or reassigns. |
| Zone | CO2 | Administrators manage zones, and zones are used to organise field staff and workload. |
| FieldStaff | CO3 | The actor who performs pickups and deliveries, so it belongs with the field delivery functions. |
| DeliveryAttempt | CO3 | Records each delivery try, successful or failed, which is created during field work. |
| ProofOfDelivery | CO3 | Captured by field staff when a delivery is confirmed, so it is created and owned by the delivery workflow. |
| Parcel | CO4 | Core domain entity that holds the booking, tracking number and delivery window, and runs the 48h delay check. |
| Recipient | CO4 | Recipient details belong to the parcel booking and are used only together with a parcel. |
| StatusHistoryEntry | CO5 | One record in a parcel's status history, which CO5 stores and reports. |
| DelayAlert | CO5 | Created and resolved by CO5 when CO4's delay check reports a delay, so alert handling stays in one place. |
| ParcelStatus | CO5 | Shared status enumeration placed in CO5 so every component can use it without an upward dependency. |
| Notification | CO6 | Base class for messages sent to users, with sending and retry behaviour. |
| StatusNotification | CO6 | Notification about a parcel status change, built and sent by CO6 from the status text it receives. |
| AssignmentNotification | CO6 | Notification about a new or changed assignment, built and sent by CO6. |
