# Use Cases

### Student 1 Contributions

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
| R-01 | UC-03 Book Parcel | UC-02 Log In | <<include>> | A customer can only book a parcel after logging in. |
| R-02 (draft) | UC-03 Book Parcel | UC-04 Generate Tracking Number | <<include>> | Every confirmed booking gets a tracking number, so this always happens as part of booking. Wording still rough. |
| R-03 (draft) | UC-05 Track Parcel | UC-02 Log In | <<include>> | Tracking is only available to logged-in customers. Wording still rough. |

