# Sarhan Alzaabi — Class Inventory

| Class ID | Class | Purpose | Related use cases | Contributor |
| --- | --- | --- | --- | --- |
| C01 | User (abstract) | Holds the login details and account state every user shares (credentials, active/deactivated, temporary password, failed logins and lock time). Customer, Administrator and FieldStaff inherit from it. | UC-02, UC-11 | Sarhan Alzaabi (b00100669) |
| C02 | Customer | Registers, books parcels and tracks them by tracking number. | UC-01, UC-03, UC-05 | Sarhan Alzaabi (b00100669) |
| C03 | Parcel | A booked shipment: tracking number, weight/dimensions, current status, timestamps and the expected delivery window. | UC-03, UC-04, UC-05 | Sarhan Alzaabi (b00100669) |
| C04 | Recipient | The receiver's name, phone, address and emirate for one parcel (the emirate decides the 24h/48h delivery window). | UC-03 | Sarhan Alzaabi (b00100669) |

## Modeling notes

- `User` is abstract: every user is a `Customer`, `Administrator` or `FieldStaff`, never just a `User`.
- `Customer` books `Parcel`: 1 to 0..*.
- `Parcel` is delivered to `Recipient`: 1 to 1, as a composition (the recipient details only exist as part of that parcel).
- `Parcel` uses `ParcelStatus` for its status attribute.
- `Administrator`, `FieldStaff` and `ParcelStatus` are shown as name-only reference classes because their details are defined by other team members.
