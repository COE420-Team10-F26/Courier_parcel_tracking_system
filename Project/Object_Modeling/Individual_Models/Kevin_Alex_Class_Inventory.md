# Kevin Alex — Class Inventory

| Class ID | Class | Purpose | Related use cases | Contributor |
| --- | --- | --- | --- | --- |
| C06 | Administrator | Manages accounts and oversees deliveries: searches/deactivates/reactivates users, creates staff/admin accounts with a temp password, sets each staff member's zone, views all parcels and assigns Pending Pickup parcels to field staff. | UC-06, UC-07, UC-08, UC-09, UC-10 | Kevin Alex (b00098368)|
| C07 | Assignment | One bulk assignment action: the set of Pending Pickup parcels an admin gives to one field staff member at one time (one summary notification per assignment). | UC-08, UC-09 | Kevin Alex (b00098368) |
| C08 | Zone | The one emirate an admin sets for a field staff member; used by the admin to match parcels to staff and see workload by area (never assigned automatically). | UC-08, UC-09, UC-10 | Kevin Alex (b00098368) |


## Modeling notes

- `Administrator` inherits from the abstract `User`, so credentials and account state come from `User`.
- `Administrator` creates `Assignment`: 1 to 0..*. Each `Assignment` is given to exactly one `FieldStaff` and contains one or more Pending Pickup `Parcel`s.
- `Parcel` reaches `FieldStaff` only through `Assignment`. There is no direct "handles" link and no admin "monitors" association, since viewing parcels is the `viewAllParcels()` method.
- Each `Assignment` has exactly one `AssignmentNotification` summary.
- `FieldStaff` works in exactly 1 `Zone`. `Zone` is a single emirate, set by the admin and never assigned automatically.
- Workload comes from `Administrator.getStaffWorkload()`, so `FieldStaff` has no `currentWorkload`.
- `Administrator` has dashed «use» dependencies on `FieldStaff`, `Zone` and `Parcel` because they appear in its method signatures.




