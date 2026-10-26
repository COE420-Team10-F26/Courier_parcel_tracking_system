# Integration Notes

Contributor: Sarhan Alzaabi (b00100669)

These notes record what happened when the four individual class models were merged into the consolidated class diagram (`Class_Diagram/consolidated_class_dig.drawio`): which classes were duplicated, which design decisions conflicted, how each was resolved, and what was changed in earlier labs to keep everything consistent.

## 1. Individual models that were merged

| Member | Area | Classes |
| --- | --- | --- |
| Sarhan Alzaabi (b00100669) | Customer (registration, login, booking, tracking) | C01 User, C02 Customer, C03 Parcel, C04 Recipient |
| Kevin Alex (b00098368) | Admin and assignment | C06 Administrator, C07 Assignment, C08 Zone |
| Muhammad Hammad Khan (b00094579) | Field staff | C11 FieldStaff, C12 DeliveryAttempt, C13 ProofOfDelivery, C14 ParcelStatus |
| Ahmed El Sabagh (b00095506) | Notifications and tracking history | C16 StatusHistoryEntry, C17 Notification, C18 StatusNotification, C19 AssignmentNotification, C20 DelayAlert |

The consolidated model has 16 classes. Before anyone started, `User` and `Parcel` were shared with the team so all four models used the same attribute names, which kept the merge small.

## 2. Duplicate classes

| Class | Owner (full version) | Also appeared in | Resolution |
| --- | --- | --- | --- |
| User | Sarhan | Kevin (full copy), Hammad (reference box), Ahmed (reference box) | One `User`, Sarhan's version; Hammad's inheritance and Ahmed's `recipient`/`actor` associations now point to it. |
| Customer, Recipient | Sarhan | Kevin (full copies) | One copy each, Sarhan's version. |
| Parcel | Sarhan | Kevin (full copy), Hammad (reference box), Ahmed (reference box) | One `Parcel`, Sarhan's version; every association from the other models now points to it. |
| Administrator | Kevin | Sarhan (reference box) | Kevin's full `Administrator`. |
| FieldStaff | Hammad | Sarhan (reference box), Kevin (reference box) | Hammad's full `FieldStaff`. |
| Zone | Kevin | Hammad (reference box) | Kevin's full `Zone`. |
| Assignment | Kevin | Ahmed (reference box) | Kevin's full `Assignment`. |
| ParcelStatus | Hammad | Sarhan, Kevin and Ahmed (reference boxes) | Hammad's «enumeration» with the 10 statuses and its two methods. |

## 3. Conflicting design decisions

| # | Conflict | Decision | Reason |
| --- | --- | --- | --- |
| 1 | `FieldStaff` works in `Zone`: Hammad had 0..1, Kevin had 1 | Exactly **1** zone per field staff member | The admin always sets a zone when the account is created (FR-10), so a field staff member is never without one. |
| 2 | Parcel status was both an attribute (`Parcel.status`) and an association (Hammad's "has current status") | Kept the **`status` attribute**, typed `ParcelStatus`, with a «use» dependency; removed the association | The same fact was shown twice. |
| 3 | `StatusHistoryEntry`'s statuses were both typed attributes and an association to `ParcelStatus` (Ahmed's "statuses") | Kept the **typed attributes**; removed the association | Same double-modelling as decision 2. The attribute types already show the link to `ParcelStatus`. |
| 4 | Parcels reached field staff two ways: a direct "handles" association and through `Assignment` | Kept the **`Assignment` route** (`Assignment` contains `Parcel`, is given to `FieldStaff`); removed "handles" | Parcels are only given to staff through an assignment (FR-09), so the direct link was redundant. |
| 5 | Workload appeared three times: `FieldStaff.currentWorkload`, `FieldStaff.getWorkload()` and `Administrator.getStaffWorkload()` | Kept **`Administrator.getStaffWorkload()`** only | A stored count can get out of sync with the real assignments, and one method is enough; the admin is the one who uses workload when assigning (FR-08). |
| 6 | `Assignment.parcelCount` stored the number of parcels | **Removed**; `getParcelCount()` counts the parcels in the "contains" association | Same out-of-sync risk as decision 5. `AssignmentNotification.parcelCount` is kept, because it is a snapshot of what the notification said when it was sent. |
| 7 | Kevin's "monitors" association between `Administrator` and `Parcel` | **Removed** | Viewing parcels is already the `viewAllParcels()` method, so the association added nothing. |
| 8 | Kevin's «use» dependencies from `Administrator` to `FieldStaff`, `Zone`, `Parcel` and `DelayAlert` | **Not drawn** in the consolidated diagram | They only repeat types that are already visible in the method signatures, and drawing them crossed most of the diagram. The only dependency kept is `Parcel` «use» `ParcelStatus`. |
| 9 | `ProofOfDelivery` used a proof image (`proofImagePath`), and a leftover `recievedByName` (sic) attribute | Uses **`recipientName`, `signature` and `emiratesId`**, no photo | This was fixed in Lab 3 after instructor feedback (FR-15). |
| 10 | Hammad's `ParcelStatus` methods `getDisplayName()` and `isFinal()` were missing from the first merge | **Kept** | They are part of Hammad's enumeration and `isFinal()` is useful for Delivered and Returned to Sender. |
| 11 | Kevin's individual inventory used IDs C01-C03 | Changed to **C06-C08** | C01-C04 were already Sarhan's; the team ranges keep every ID unique and traceable. |
| 12 | Naming differed between members (`notificationID` vs `userId`, `*` vs `0..*`) | In the consolidated diagram, standardised to **`...Id`** for every ID attribute and **`0..*`** for every "many" multiplicity | One consistent style across the whole diagram. |
| 13 | Some association names differed between the individual diagrams and read less clearly as phrases | Renamed in the consolidated diagram: recipient → **is sent to**, about parcel → **is about**, history of → **records change of**, actor → **made by**, given to → **is given to** | Each association now reads as a sentence from source to target (e.g. "StatusHistoryEntry made by User"). The multiplicities and meaning are unchanged. |
| 14 | `ProofOfDelivery.notes` was not set by `confirmDelivery()` and is not part of FR-15 | **Removed** in the consolidated diagram | Proof of delivery is exactly name, signature and Emirates ID (FR-15); an attribute nothing sets would always be empty. |

## 3b. Tidy-ups to the individual diagrams

While integrating, some small presentation fixes were made to the individual diagrams so they meet the lab's notation requirements. The classes and design in each member's model were not changed.

- **All:** each diagram title shows the member's name and AUS ID; reference boxes name Hammad's classes as "(Hammad)".
- **Kevin:** added the missing 0..* multiplicity at the `Assignment` end of "given to".
- **Hammad:** exported in high resolution without the draw.io grid, removed the leftover `recievedByName`, wrote `signature` in lowercase, and labelled `ParcelStatus` as «enumeration».
- **Ahmed:** moved the multiplicities from inside the association labels to the line ends, attached both notes to their classes, and re-laid out the lines so none overlap. His `statuses` association to `ParcelStatus` is kept as he designed it; it is only replaced during the merge (decision 3).

## 4. Gaps found by the consistency check and in review

| Gap | Fix |
| --- | --- |
| UC-19 (tracking history) had no method returning a parcel's history | Added `Parcel.getStatusHistory(): List<StatusHistoryEntry>` |
| UC-21 (admin looks up history by parcel ID) had no way to find a parcel by its ID | Added `Administrator.searchParcelHistory(parcelId: int): List<StatusHistoryEntry>` |
| UC-20 (delay alerts) had no admin method to view alerts | Added `Administrator.viewDelayAlerts(): List<DelayAlert>` |
| UC-12 / FR-11 (view pickups) needs the sender's address, but `Parcel` only had `senderEmirate` | Added `Parcel.senderAddress`, and `Customer.bookParcel()` now takes `senderAddress` and `senderEmirate`; booking collects both (see the updates to previous labs) |
| Lab 4 P3 says "reassigning is allowed only before pickup", but there was no method for it | Added `Administrator.reassignParcel(parcel: Parcel, staff: FieldStaff): Assignment`. "contains" stays 0..1 on the `Assignment` side because a parcel belongs to at most one current assignment; reassigning moves it to a new one before pickup. An assignment whose last parcel is reassigned is cancelled, so no assignment is left with 0 parcels and "1..*" still holds (shown in a note) |

## 5. Classes without a data store

`Notification`, `StatusNotification`, `AssignmentNotification` and `DelayAlert` have no matching data store in the Lab 4 DFD. They do have IDs and state (`deliveryStatus`, `retryUntil`, `isActive`), so this is a deliberate design choice:

- A notification only needs to exist while it is being delivered. The notification service keeps it in its outgoing queue until it is sent or its 24-hour retry window ends (NFR-18), and the ID identifies it while it is in that queue. Nothing in the requirements asks for old notifications to be kept or viewed later.
- A `DelayAlert` lives on the admin dashboard only while it is active. Its ID identifies it on the dashboard, and `resolve()` sets `isActive` to false at the parcel's next real status update, after which it is no longer shown. Every alert can be rebuilt from D2 Parcels (an undelivered parcel whose `lastStatusChangeAt` is more than 48 hours ago), so it needs no separate store. Setting the status to DELAYED does not reset `lastStatusChangeAt`, so it does not restart the 48-hour check.

If the system later needs a notification history, a "D4 Notifications" data store would be added to the Level-0 DFD and the Core Processes Matrix.

## 6. Changes made to previous labs

The lab sheet allows earlier labs to be revised when inconsistencies are found. These changes are in the repository and in the updated reports in `Documents/`:

- **Lab 4 (Oct 25):** the Core Processes Matrix rules were brought in line with the fixed Lab 3 requirements and this class model. P1 now says the admin sets each field staff member's zone. P2 has the full booking validation and delivery window. P3 sets undelivered parcels with no change for 48 hours to Delayed. P4 has the Attempted status with a mandatory note, max 3 attempts, then Returned to Sender. P5 uses the standard status names. The D1 and D2 data store descriptions now cover zones, recipient details, assignments and delivery attempts.
- **Lab 3 and Lab 4 (Oct 27):** booking now collects the sender's pickup address and emirate, matching `Parcel.senderAddress` and `Parcel.senderEmirate`. Updated FR-03, UC-03 and the P2 booking details input.

## 7. Final model summary

- 16 classes, covering all three actors and all 21 team use cases.
- Two genuine inheritance hierarchies: `User` → `Customer`, `Administrator`, `FieldStaff`, and `Notification` → `StatusNotification`, `AssignmentNotification`.
- One composition: `Parcel` ◆ `Recipient`.
- One enumeration: `ParcelStatus`, with the 10 statuses from Lab 3.
