# Use Cases

### Student 1 Contributions (Sarhan)

| **UC ID** | **Use Case Name** | **Primary Actor** | **Short Description** | **Contributor** |
| --- | --- | --- | --- | --- |
| UC-01 | Register Account | Customer | A new customer creates an account by entering name, email, phone number and password. | Sarhan (b00100669) |
| UC-02 | Log In | Customer | A registered user logs in and is taken to the dashboard for their role. | Sarhan (b00100669) |
| UC-03 | Book Parcel | Customer | A logged-in customer books a parcel by entering recipient and parcel details. | Sarhan (b00100669) |
| UC-04 (draft) | Generate Tracking Number | Customer | Tied to booking, needs its own short description still. | Sarhan (b00100669) |
| UC-05 (draft) | Track Parcel | Customer | Customer looks up parcel status, description still rough. | Sarhan (b00100669) |

### Use Case Relationships (Student 1, so far)

| **Relationship ID** | **Base Use Case** | **Related Use Case** | **Relationship** | **Justification** |
| --- | --- | --- | --- | --- |
| R-01 | UC-03 Book Parcel | UC-02 Log In | `<<include>>` | A customer can only book a parcel after logging in. |
| R-02 (draft) | UC-03 Book Parcel | UC-04 Generate Tracking Number | `<<include>>` | Every confirmed booking gets a tracking number, so this always happens as part of booking. Wording still rough. |
| R-03 (draft) | UC-05 Track Parcel | UC-02 Log In | `<<include>>` | Tracking is only available to logged-in customers. Wording still rough. |



### Student 2 Contributions (Kevin)

| UC ID | Use Case Name | Primary Actor | Short Description | Contributor |
| --- | --- | --- | --- | --- |
| UC-01 | View and Search User Accounts | Admin | The admin views a list of all registered users with name, email, role, and account status, and can search by name or email and filter by role. | Kevin (b00098368)|
| UC-02 | Deactivate/Reactivate User Account | Admin | The admin deactivates or reactivates a customer or field staff account. A deactivated user cannot log in, and their parcel records are retained. | Kevin (b00098368)|
| UC-03 | View All Parcels on Dashboard | Admin | The admin views all parcels on the dashboard with each parcel's current status and assigned field staff member. | Kevin (b00098368)|
| UC-04 | Assign Parcels to Field Staff | Admin | The admin selects one or more "Pending Pickup" parcels and assigns them to a single field staff member. Parcel statuses change to "Assigned" and the field staff member is notified. | Kevin (b00098368)|
| UC-05 | Create Staff/Admin Account | Admin | The admin creates a field staff or admin account by entering name, email, and role. The system generates a temporary password that must be changed at first login. | Kevin (b00098368)|

### Use Case Relationships (Student 2)

| **Relationship ID** | **Base Use Case** | **Related Use Case** | **Relationship** | **Justification** |
| --- | --- | --- | --- | --- |
| R-01 | UC-01 View and Search User Accounts | Search/Filter Users (part of UC-01) | `<<extend>>` | Searching and filtering are optional; the admin can just browse the full list. |
| R-02 | UC-02 Deactivate/Reactivate User Account | UC-01 View and Search User Accounts | `<<include>>` | The admin must locate and select a user from the list before changing their status. |
| R-03 | UC-04 Assign Parcels to Field Staff | UC-03 View All Parcels on Dashboard | `<<include>>` | Assignment always starts from the parcel list, since the admin picks "Pending Pickup" parcels from it |
| R-04 | Log In | UC-05 Change Temporary Password | `<<extend>>` | It only happens conditionally, when the account is on a temporary password at first login. |
| R-05 | UC-04 Assign Parcels to Field Staff | Notify Field Staff | `<<include>>` | FR-04 requires that every assignment sends the field staff member one notification listing how many parcels were added. It always happens, so it's part of the base behaviour. |

