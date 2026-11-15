# Component Descriptions

Contributor: Sarhan Alzaabi (b00100669)

The system is split into six components in three layers (see `Architectural_Style_Selection.md`). Each component below lists its responsibilities, the use cases and functional requirements it supports, and its provided and required interfaces with their operations. The interfaces are the ones drawn in `Component_Diagram/`.

Rules the interfaces follow:

1. Operations between components pass IDs (`parcelId`, `userId`, `customerId`, `staffId`, `assignmentId`) and plain values, not objects from the calling component. An operation may return the providing component's own classes, since the caller already depends on that component.
2. In the Application layer the only direction is CO3 → CO2 → CO1.
3. `ParcelStatus` is in CO5, the lowest layer, so the other components can use it without depending upward.
4. The 48-hour delay check runs in CO4, because `Parcel` holds the time of its last status change. CO4 sets the parcel to Delayed and asks CO5 to raise or resolve the alert.
5. CO5 and CO6 do not depend on each other (NFR-19). CO6 looks up a user's email and phone by `userId` in D1, and the status is passed to it as text.

## Overview

| ID | Component | Layer | Classes | Provided interfaces | Required interfaces |
| --- | --- | --- | --- | --- | --- |
| CO1 | Account Management | Application | User, Customer | IAuthentication, ICustomerServices, IUserAdministration | IParcelService |
| CO2 | Administration & Assignment | Application | Administrator, Assignment, Zone | IAdministration, IAssignmentQuery | IUserAdministration, IParcelService, IStatusHistory, IDelayAlert, INotification |
| CO3 | Field Delivery | Application | FieldStaff, DeliveryAttempt, ProofOfDelivery | IFieldDelivery | IAssignmentQuery, IParcelService |
| CO4 | Parcel Management | Domain | Parcel, Recipient | IParcelService | IStatusHistory, IDelayAlert, INotification |
| CO5 | Tracking & Monitoring | Services | StatusHistoryEntry, DelayAlert, ParcelStatus | IStatusHistory, IDelayAlert | none |
| CO6 | Notification | Services | Notification, StatusNotification, AssignmentNotification | INotification | IMessageGateway (Email/SMS Gateway) |

Besides the interfaces, CO3 has a plain dependency on CO1 (`FieldStaff` extends `User`) and on CO5 (it uses `ParcelStatus` when updating a status). Every component also depends on the Database, where it keeps its data (D1 Users, D2 Parcels, D3 Status History).

---

## CO1 Account Management

**Layer:** Application
**Classes:** User (abstract), Customer

**Responsibilities**
- Registers customers and stores all user accounts (customers, field staff and admins) in D1.
- Logs users in and out and routes each user to the dashboard for their role.
- Handles passwords: salted hashing, the forced change of a temporary password at first login, and locking an account for 15 minutes after 4 failed logins.
- Takes the customer's booking, tracking and history requests and passes them to CO4.
- Gives CO2 the account operations an admin needs (list, search, deactivate, reactivate, create).

**Supported functionality**
- Use cases: UC-01 Register Account, UC-02 Log In, UC-03 Book Parcel (customer side), UC-05 Track Parcel, UC-11 Change Temporary Password, UC-19 View Parcel Tracking History (customer side)
- Functional requirements: FR-01, FR-02, FR-03 (booking request), FR-05, FR-19 (by tracking number)
- Non-functional requirements: NFR-02, NFR-06, NFR-07, NFR-09

**Provided interface: IAuthentication** (used by the customer, admin and field staff apps)

| Operation | Parameters | Return type | From class |
| --- | --- | --- | --- |
| login | email: String, password: String | int | User.login() |
| logout | userId: int | void | User.logout() |
| changePassword | userId: int, oldPassword: String, newPassword: String | boolean | User.changePassword() |
| getRole | userId: int | String | User (role) |
| isTempPassword | userId: int | boolean | User (isTempPassword) |

`login()` returns the logged-in user's `userId`, or 0 if the email or password is wrong, or the account is locked or deactivated, so the apps and other components can use the ID (rule 1). `getRole()` gives "Customer", "FieldStaff" or "Administrator" for routing to the right dashboard, and `isTempPassword()` tells the app to start UC-11.

**Provided interface: ICustomerServices** (used by the customer app)

| Operation | Parameters | Return type | From class |
| --- | --- | --- | --- |
| register | fullName: String, email: String, phone: String, password: String | boolean | Customer.register() |
| bookParcel | customerId: int, recipientName: String, recipientPhone: String, recipientAddress: String, recipientEmirate: String, senderAddress: String, senderEmirate: String, weightKg: double, lengthCm: double, widthCm: double, heightCm: double, description: String | Parcel | Customer.bookParcel() |
| trackParcel | customerId: int, trackingNumber: String | Parcel | Customer.trackParcel() |
| viewMyParcels | customerId: int | List&lt;Parcel&gt; | Customer.viewMyParcels() |
| viewTrackingHistory | customerId: int, trackingNumber: String | List&lt;String&gt; | Parcel.getStatusHistory(), StatusHistoryEntry.getTransition() |

`bookParcel()` takes the recipient's details as values instead of a `Recipient` object, because the recipient is created inside CO4 with the parcel. `trackParcel()` and `viewTrackingHistory()` only return parcels that belong to the customer, otherwise the parcel is "not found".

**Provided interface: IUserAdministration** (used by CO2)

| Operation | Parameters | Return type | From class |
| --- | --- | --- | --- |
| getAllUsers | none | List&lt;User&gt; | Administrator.viewAllUsers() |
| searchUsers | keyword: String, role: String, isActive: boolean | List&lt;User&gt; | Administrator.searchUsers() |
| deactivateUser | userId: int | void | Administrator.deactivateUser() |
| reactivateUser | userId: int | void | Administrator.reactivateUser() |
| createAccount | fullName: String, email: String, phone: String, role: String, tempPassword: String | int | Administrator.createStaffAccount(), createAdminAccount() |

`createAccount()` saves the new field staff or admin account with the temporary password generated by CO2 (`Administrator.generateTempPassword()`) and returns the new `userId`.

**Required interface:** IParcelService (CO4), for booking, tracking, the customer's parcel list and the tracking history.

---

## CO2 Administration & Assignment

**Layer:** Application
**Classes:** Administrator, Assignment, Zone

**Responsibilities**
- Lets the admin view, search, deactivate and reactivate user accounts and create field staff and admin accounts (through CO1).
- Keeps each field staff member's zone (one emirate each), set by the admin.
- Shows the admin dashboard: all parcels with their status, destination zone and assigned staff, and each staff member's zone and workload.
- Bulk-assigns Pending Pickup parcels to one field staff member, and reassigns a parcel before pickup.
- Sends the field staff member one summary notification per assignment (through CO6).
- Gives the admin the delay alerts and the tracking history of any parcel (through CO5).
- Tells CO3 which parcels are assigned to a staff member and which zone they work in.

**Supported functionality**
- Use cases: UC-06 View and Search User Accounts, UC-07 Deactivate/Reactivate User Account, UC-08 View All Parcels on Dashboard, UC-09 Assign Parcels to Field Staff, UC-10 Create Staff/Admin Account, UC-18 Receive Assignment Notification (sending side), UC-20 View Dashboard Delay Alerts (admin side), UC-21 Search Tracking History by Parcel ID
- Functional requirements: FR-06, FR-07, FR-08, FR-09, FR-10, FR-17 (sending side), FR-19 (by parcel ID), FR-20 (dashboard side)
- Non-functional requirements: NFR-20

**Provided interface: IAdministration** (used by the admin dashboard)

| Operation | Parameters | Return type | From class |
| --- | --- | --- | --- |
| viewAllUsers | none | List&lt;User&gt; | Administrator.viewAllUsers() |
| searchUsers | keyword: String, role: String, isActive: boolean | List&lt;User&gt; | Administrator.searchUsers() |
| deactivateUser | userId: int | void | Administrator.deactivateUser() |
| reactivateUser | userId: int | void | Administrator.reactivateUser() |
| createStaffAccount | fullName: String, email: String, phone: String, emirate: String | int | Administrator.createStaffAccount() |
| createAdminAccount | fullName: String, email: String, phone: String | int | Administrator.createAdminAccount() |
| changeStaffZone | staffId: int, emirate: String | void | Administrator.changeStaffZone() |
| viewAllParcels | none | List&lt;Parcel&gt; | Administrator.viewAllParcels() |
| getUnassignedParcels | none | List&lt;Parcel&gt; | Administrator.getUnassignedParcels() |
| getStaffWorkload | staffId: int | int | Administrator.getStaffWorkload() |
| assignParcels | parcelIds: List&lt;Integer&gt;, staffId: int | Assignment | Administrator.assignParcels(), Assignment.confirmAssignment() |
| reassignParcel | parcelId: int, staffId: int | Assignment | Administrator.reassignParcel() |
| viewDelayAlerts | none | List&lt;DelayAlert&gt; | Administrator.viewDelayAlerts() |
| searchParcelHistory | parcelId: int | List&lt;StatusHistoryEntry&gt; | Administrator.searchParcelHistory() |

`assignParcels()` creates the assignment, checks that every parcel is Pending Pickup (or Delayed while Pending Pickup), sets each one to Assigned through `IParcelService.updateStatus()` and then sends the staff summary through `INotification.sendAssignmentNotification()`, the same order as the UC-09 sequence diagram. If any parcel is not valid, nothing is assigned. `reassignParcel()` is only allowed before pickup.

Used inside CO2 only: `Assignment.addParcel()`, `validateParcels()`, `getParcelCount()`, `getAssignedStaff()`, `Zone.coversEmirate()`, `Zone.getAssignedStaff()` and `Administrator.generateTempPassword()`.

**Provided interface: IAssignmentQuery** (used by CO3)

| Operation | Parameters | Return type | From class |
| --- | --- | --- | --- |
| getAssignedParcels | staffId: int | List&lt;Parcel&gt; | Assignment (contains), FieldStaff.viewAssignedParcels() |
| getStaffZone | staffId: int | Zone | FieldStaff.getZone(), Zone.getEmirate() |

**Required interfaces**
- IUserAdministration (CO1): user list, search, deactivate/reactivate, creating accounts
- IParcelService (CO4): the parcel lists, checking a parcel's status and setting parcels to Assigned
- IStatusHistory (CO5): history search by parcel ID
- IDelayAlert (CO5): the active delay alerts for the dashboard
- INotification (CO6): the assignment summary for the field staff member

---

## CO3 Field Delivery

**Layer:** Application
**Classes:** FieldStaff, DeliveryAttempt, ProofOfDelivery

**Responsibilities**
- Shows a field staff member the parcels assigned to them for pickup (with sender address) and for delivery (with recipient address).
- Confirms pickup, which sets the parcel to Picked Up and records the pickup time.
- Updates the parcel status through the delivery (In Transit, Out for Delivery, Attempted, Delivered).
- Records failed delivery attempts with a mandatory reason; after the 3rd failed attempt the parcel becomes Returned to Sender.
- Captures proof of delivery (recipient's name, signature and Emirates ID) before a parcel is marked Delivered.
- Keeps delivery attempts and proof of delivery with the parcel record in D2.

**Supported functionality**
- Use cases: UC-12 View Assigned Pickups, UC-13 View Assigned Deliveries, UC-14 Update Parcel Status, UC-15 Confirm Pickup, UC-16 Record Proof of Delivery
- Functional requirements: FR-11, FR-12, FR-13, FR-14, FR-15
- Non-functional requirements: NFR-11, NFR-12, NFR-13, NFR-14, NFR-15

**Provided interface: IFieldDelivery** (used by the field staff app)

| Operation | Parameters | Return type | From class |
| --- | --- | --- | --- |
| viewAssignedParcels | staffId: int | List&lt;Parcel&gt; | FieldStaff.viewAssignedParcels() |
| markPickedUp | staffId: int, parcelId: int | void | FieldStaff.markPickedUp() |
| updateDeliveryStatus | staffId: int, parcelId: int, newStatus: ParcelStatus | boolean | FieldStaff.updateDeliveryStatus() |
| recordFailedAttempt | staffId: int, parcelId: int, reason: String | DeliveryAttempt | FieldStaff.recordFailedAttempt(), DeliveryAttempt.recordOutcome() |
| confirmDelivery | staffId: int, parcelId: int, recipientName: String, signature: String, emiratesId: String | ProofOfDelivery | FieldStaff.confirmDelivery(), ProofOfDelivery.isComplete() |
| getZone | staffId: int | Zone | FieldStaff.getZone() |

`confirmDelivery()` first checks with `IParcelService.getEffectiveStatus()` that the parcel is Out for Delivery (or Delayed while Out for Delivery), as in the UC-16 sequence diagram. If the proof is incomplete it is discarded and the parcel keeps its status.

Used inside CO3 only: `DeliveryAttempt.isFinalAttempt()`, `DeliveryAttempt.getFailureReason()`, `ProofOfDelivery.getDeliveredAt()` and `ProofOfDelivery.getRecipientName()`.

**Required interfaces**
- IAssignmentQuery (CO2): the staff member's assigned parcels and zone
- IParcelService (CO4): parcel details and every status change

---

## CO4 Parcel Management

**Layer:** Domain
**Classes:** Parcel, Recipient

**Responsibilities**
- Creates a parcel from a booking after validating it (no empty fields, weight and dimensions in range, UAE phone number), with its recipient.
- Generates the unique tracking number (CPT-XXXXXX) and calculates the delivery window (24 hours in the same emirate, 48 hours otherwise).
- Stores parcels and recipients in D2 and answers parcel lookups for the other components.
- Is the only place a parcel's status changes. Every change records a history entry (CO5), resolves an active delay alert if there is one (CO5), and sends the customer a status notification (CO6), in that order.
- Runs the 48-hour delay check on undelivered parcels, sets them to Delayed and raises the alert in CO5.

**Supported functionality**
- Use cases: UC-03 Book Parcel, UC-04 Generate Tracking Number, UC-05 Track Parcel (lookup), UC-14 Update Parcel Status (status change), UC-17 Receive Status Notification (sending side), UC-19 View Parcel Tracking History (lookup), UC-20 View Dashboard Delay Alerts (detection)
- Functional requirements: FR-03, FR-04, FR-05, FR-16 (trigger), FR-18 (trigger), FR-20 (detection)
- Non-functional requirements: NFR-01, NFR-04, NFR-05

**Provided interface: IParcelService** (used by CO1, CO2 and CO3)

| Operation | Parameters | Return type | From class |
| --- | --- | --- | --- |
| createParcel | customerId: int, recipientName: String, recipientPhone: String, recipientAddress: String, recipientEmirate: String, senderAddress: String, senderEmirate: String, weightKg: double, lengthCm: double, widthCm: double, heightCm: double, description: String | Parcel | Parcel, Recipient.isValidPhone(), Parcel.generateTrackingNumber() |
| getParcel | parcelId: int | Parcel | Parcel |
| getParcelByTrackingNumber | trackingNumber: String | Parcel | Parcel |
| getParcelsByCustomer | customerId: int | List&lt;Parcel&gt; | Parcel |
| getParcelsByStatus | status: ParcelStatus | List&lt;Parcel&gt; | Parcel.getStatus() |
| getAllParcels | none | List&lt;Parcel&gt; | Parcel |
| updateStatus | parcelId: int, newStatus: ParcelStatus, actorId: int, location: String | void | Parcel.updateStatus() |
| getStatus | parcelId: int | ParcelStatus | Parcel.getStatus() |
| getEffectiveStatus | parcelId: int | ParcelStatus | Parcel.getStatus(), Parcel.getStatusHistory() |
| getStatusHistory | parcelId: int | List&lt;String&gt; | Parcel.getStatusHistory(), StatusHistoryEntry.getTransition() |
| getDeliveryWindowHours | parcelId: int | int | Parcel.getDeliveryWindowHours() |

`createParcel()` returns `null` if the booking is invalid, and nothing is saved. `getEffectiveStatus()` returns the status a parcel counts as: its current status, or, if it is Delayed, the status it had before the delay (from its latest history entry), as the status table says. `getStatusHistory()` returns each change as text, oldest first, which is what the customer sees in UC-19. `actorId` in `updateStatus()` is 0 for changes made by the system.

Used inside CO4 only: `Parcel.generateTrackingNumber()` and `Recipient.isValidPhone()` (in `createParcel()`). The status text sent to CO6 comes from `ParcelStatus.getDisplayName()`, and the delay check skips parcels where `ParcelStatus.isFinal()` is true (Delivered, Returned to Sender).

**Required interfaces**
- IStatusHistory (CO5): recording a history entry on every status change
- IDelayAlert (CO5): checking, raising and resolving delay alerts
- INotification (CO6): the customer's status notification on every status change

---

## CO5 Tracking & Monitoring

**Layer:** Services
**Classes:** StatusHistoryEntry, DelayAlert, ParcelStatus (enumeration)

**Responsibilities**
- Defines the ten parcel statuses (`ParcelStatus`) used by the whole system.
- Records a timestamped history entry for every status change (previous status, new status, actor, location) in D3, and returns a parcel's full history in chronological order.
- Decides whether a parcel is delayed (no status change for 48 hours, with the threshold kept in configuration, NFR-08), keeps delay alerts with the parcel record in D2, and resolves an alert at the parcel's next real status update.
- Works without CO6, so a notification failure never stops a history entry from being recorded (NFR-19).

**Supported functionality**
- Use cases: UC-14 Update Parcel Status (history entry), UC-19 View Parcel Tracking History, UC-20 View Dashboard Delay Alerts, UC-21 Search Tracking History by Parcel ID
- Functional requirements: FR-18, FR-19, FR-20
- Non-functional requirements: NFR-17, NFR-19

**Provided interface: IStatusHistory** (used by CO2 and CO4)

| Operation | Parameters | Return type | From class |
| --- | --- | --- | --- |
| recordEntry | parcelId: int, previousStatus: ParcelStatus, newStatus: ParcelStatus, actorId: int, location: String | void | StatusHistoryEntry |
| getHistory | parcelId: int | List&lt;StatusHistoryEntry&gt; | StatusHistoryEntry |
| getLatestEntry | parcelId: int | StatusHistoryEntry | StatusHistoryEntry |

**Provided interface: IDelayAlert** (used by CO2 and CO4)

| Operation | Parameters | Return type | From class |
| --- | --- | --- | --- |
| evaluate | lastStatusChangeAt: DateTime, now: DateTime | boolean | DelayAlert.evaluate() |
| raiseAlert | parcelId: int | DelayAlert | DelayAlert |
| hasActiveAlert | parcelId: int | boolean | DelayAlert (isActive) |
| resolveAlert | parcelId: int | void | DelayAlert.resolve() |
| getActiveAlerts | none | List&lt;DelayAlert&gt; | DelayAlert |

`evaluate()` takes the time of the parcel's last status change instead of the `Parcel` object, so CO5 does not depend on CO4 (rule 1). CO4 only calls it for undelivered parcels.

**Required interfaces:** none. CO5 only uses the Database.

---

## CO6 Notification

**Layer:** Services
**Classes:** Notification (abstract), StatusNotification, AssignmentNotification

**Responsibilities**
- Builds and sends the customer's status notification on every status change, and the field staff member's single summary for each assignment.
- Looks up the recipient's email and phone by `userId` in D1 and sends through the Email/SMS Gateway, using the channels set in configuration (NFR-08).
- Keeps each notification in its outgoing queue and retries for up to 24 hours before marking it failed (NFR-18).
- `send()` only hands the notification to the queue and returns at once, so it never blocks the status change or the history entry (NFR-19).

**Supported functionality**
- Use cases: UC-17 Receive Status Notification, UC-18 Receive Assignment Notification
- Functional requirements: FR-09 (notifications), FR-13 (Returned to Sender notification), FR-16, FR-17
- Non-functional requirements: NFR-16, NFR-18, NFR-19

**Provided interface: INotification** (used by CO2 and CO4)

| Operation | Parameters | Return type | From class |
| --- | --- | --- | --- |
| sendStatusNotification | customerId: int, trackingNumber: String, statusText: String | boolean | StatusNotification.buildMessage(), Notification.send() |
| sendAssignmentNotification | staffId: int, assignmentId: int, parcelCount: int | boolean | AssignmentNotification.buildSummary(), Notification.send() |

Both return `true` once the notification is queued. Retrying and marking a notification failed (`Notification.retry()`, `markFailed()`) happen inside CO6.

**Required interface: IMessageGateway** (provided by the external Email/SMS Gateway)

| Operation | Parameters | Return type |
| --- | --- | --- |
| sendEmail | to: String, subject: String, body: String | boolean |
| sendSms | phone: String, text: String | boolean |
