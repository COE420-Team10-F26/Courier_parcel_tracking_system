# Team Class Inventory

| Class ID | Class | Purpose | Related use cases | Contributor |
| --- | --- | --- | --- | --- |
| C01 | User (abstract) | Holds the login details and account state shared by all users, including credentials, activation state, temporary-password state, failed login attempts, and lock time. | UC-02, UC-11 | Sarhan Alzaabi (b00100669) |
| C02 | Customer | Registers, books parcels, and tracks them by tracking number. | UC-01, UC-03, UC-05 | Sarhan Alzaabi (b00100669) |
| C03 | Parcel | Represents a booked shipment, including its tracking number, dimensions, weight, current status, timestamps, and expected delivery window. | UC-03, UC-04, UC-05, UC-14, UC-19, UC-21 | Sarhan Alzaabi (b00100669) |
| C04 | Recipient | Stores the receiver's name, phone, address, and emirate for a parcel. | UC-03 | Sarhan Alzaabi (b00100669) |
| C06 | Administrator | Manages user accounts, creates staff/admin accounts, sets staff zones, views parcels, and assigns pending pickups to field staff. | UC-06, UC-07, UC-08, UC-09, UC-10, UC-20, UC-21 | Kevin Alex (b00098368) |
| C07 | Assignment | Represents one bulk assignment action in which an administrator assigns pending-pickup parcels to one field staff member. | UC-08, UC-09 | Kevin Alex (b00098368) |
| C08 | Zone | Represents the emirate set by an administrator for a field staff member and used to match parcels and view workload by area. | UC-08, UC-09, UC-10 | Kevin Alex (b00098368) |
| C11 | FieldStaff | A delivery staff member who views assigned parcels, updates delivery status, records failed attempts, and confirms delivery with proof. | UC-12, UC-13, UC-14, UC-15, UC-16 | Muhammad Hammad Khan (b00094579) |
| C12 | DeliveryAttempt | Records one parcel delivery attempt, including its number, time, outcome, and failure reason; after three failed attempts the parcel is returned to sender. | UC-14 | Muhammad Hammad Khan (b00094579) |
| C13 | ProofOfDelivery | Records delivery evidence: the recipient's name, signature, Emirates ID, and delivery time. | UC-16 | Muhammad Hammad Khan (b00094579) |
| C14 | ParcelStatus (enum) | Defines the parcel statuses: Booked, Pending Pickup, Assigned, Picked Up, In Transit, Out for Delivery, Attempted, Delivered, Delayed, and Returned to Sender. | UC-14, UC-15, UC-16 | Muhammad Hammad Khan (b00094579) |
| C16 | StatusHistoryEntry | Stores one timestamped parcel status transition, including an optional previous status, required new status, optional actor, and location. | UC-19, UC-21 | Ahmed El Sabagh (b00095506) |
| C17 | Notification (abstract) | Holds shared notification content, recipient, delivery timestamps, and delivery/retry state for all notification types. | UC-17, UC-18 | Ahmed El Sabagh (b00095506) |
| C18 | StatusNotification | Notifies a customer when a parcel's status changes. | UC-17 | Ahmed El Sabagh (b00095506) |
| C19 | AssignmentNotification | Sends a field staff member one summary notification for a bulk assignment. | UC-18 | Ahmed El Sabagh (b00095506) |
| C20 | DelayAlert | Represents an active or resolved dashboard alert for an undelivered parcel with no status change for 48 hours. | UC-20 | Ahmed El Sabagh (b00095506) |

## Modeling notes

- `Administrator`, `Customer`, and `FieldStaff` inherit from the shared abstract `User`.
- Each `FieldStaff` member works in exactly one `Zone`, set by an administrator.
- Parcels reach field staff through `Assignment`; there is no direct parcel-to-field-staff handling association.
- The parcel's status remains its `status` attribute and uses the `ParcelStatus` enumeration.
- `StatusNotification` and `AssignmentNotification` inherit from `Notification`.
- Each bulk `Assignment` has exactly one `AssignmentNotification` summary.
- `StatusHistoryEntry` belongs to a `Parcel`; the `User` who made the change is optional for system-generated changes.
- A parcel is delayed when it is undelivered and has had no status change for 48 hours.
- Duplicates and conflicts found while merging, and how each was resolved, are recorded in `Integration_Notes.md`.
- The class IDs use the team-assigned ranges: C01-C04, C06-C08, C11-C14 and C16-C20 (16 classes). Each row keeps the ID from the member's individual inventory, so it stays traceable.
