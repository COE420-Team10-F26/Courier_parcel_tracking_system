# Muhammad Hammad Khan — Class Inventory

| Class ID | Class | Purpose | Related use cases | Contributor |
| --- | --- | --- | --- | --- |
| C11 | FieldStaff | Delivery staff member (inherits from `User`). Views the parcels assigned to them, marks them picked up, updates delivery status, records failed attempts and confirms delivery with proof. Works in the one zone the admin sets for them. | UC-12, UC-13, UC-14, UC-15, UC-16 | Muhammad Hammad Khan (b00094579) |
| C12 | DeliveryAttempt | One try to deliver a parcel: attempt number (max 3), time, success/failure and failure reason. A 3rd failed attempt sends the parcel to Returned to Sender. | UC-14 | Muhammad Hammad Khan (b00094579) |
| C13 | ProofOfDelivery | Evidence that a parcel was delivered: the recipient's name, their signature, their Emirates ID and the delivery time. | UC-16 | Muhammad Hammad Khan (b00094579) |
| C14 | ParcelStatus (enum) | The 10 fixed parcel statuses: Booked, Pending Pickup, Assigned, Picked Up, In Transit, Out for Delivery, Attempted, Delivered, Delayed, Returned to Sender. | UC-14, UC-15, UC-16 | Muhammad Hammad Khan (b00094579) |

## Modeling notes

- `FieldStaff` inherits from `User`, so the shared account fields are not repeated.
- `FieldStaff` handles `Parcel`: 0..1 to 0..*.
- `FieldStaff` performs `DeliveryAttempt`: 1 to 0..*.
- `Parcel` has attempts `DeliveryAttempt`: 1 to 0..3.
- `Parcel` is confirmed by `ProofOfDelivery`: 1 to 0..1.
- `FieldStaff` captures `ProofOfDelivery`: 1 to 0..*.
- `Parcel` has current status `ParcelStatus`: 0..* to 1.
- `FieldStaff` works in `Zone`: 0..* to 0..1. Only an admin sets a zone.
- `ProofOfDelivery` holds recipientName, signature and emiratesId (no photo), and delivery is only complete once all three are captured.
- `Parcel` and `Zone` are shown as name-only reference classes. Only `User` is abstract (italic).
