# Functional Requirements

### Student 1 Contributions (Sarhan)

| **FR ID** | **Functional Requirement** | **Source Scenario/Stakeholder** | **Contributor** |
| --- | --- | --- | --- |
| FR-01 | The system shall allow a new user to register as a customer by entering full name, email, phone number and a password of at least 8 characters, and shall reject the registration if the email is already registered or any field is empty. | S-01, Customer | Sarhan (b00100669) |
| FR-02 | The system shall log in any registered user (customer, field staff or administrator) using email and password, open the dashboard that matches the user's role (customer, field staff or administrator), and show a general error message when the email or password is wrong. | S-01, S-02, Customer, Field Staff, Admin | Sarhan (b00100669) |
| FR-03 | The system shall allow a logged-in customer to book a parcel by entering the recipient's name, phone number and address and the parcel's weight, dimensions and description, and shall not confirm a booking when a mandatory field is empty, the phone number is not a valid UAE number (+971 followed by 9 digits), the weight is not more than 0 kg and at most 70 kg, or any dimension (length, width, height) is not more than 0 cm and at most 150 cm. On confirmation, the system shall show the expected delivery window: 24 hours from pickup if the recipient is in the same emirate as the sender, 48 hours if it is in a different emirate. | S-01, Customer | Sarhan (b00100669) |
| FR-04 | The system shall automatically generate a unique tracking number in the format CPT-XXXXXX for every confirmed booking and display it to the customer on the booking confirmation page. | S-01, Customer | Sarhan (b00100669) |
| FR-05 | The system shall allow a logged-in customer to enter a tracking number and shall show the parcel's current status and expected delivery window, or a "parcel not found" message when the number does not exist. The full status history is viewed separately (Ahmed's FR-04). | S-02, Customer | Sarhan (b00100669) |

### Student 2 Contributions (Kevin)

| **FR ID** | **Functional Requirement** | **Source Scenario/Stakeholder** | **Contributor** |
| --- | --- | --- | --- |
| FR-01 | The system must allow an admin to view a list of all registered users showing each user's name, email, role (customer, field staff, admin), and account status (active/deactivated), and to search by name or email and filter by role. | Admin | Kevin (b00098368)|
| FR-02 | The system must allow an admin to deactivate and reactivate customer and field staff accounts. A deactivated user must be unable to log in, and that user's existing parcel records must be retained. | Admin | Kevin (b00098368) |
| FR-03 | The system must display, on the admin dashboard, a list of all parcels along with their current status and assigned field staff member. | S-03, Admin | Kevin (b00098368)|
| FR-04 | The system must allow an admin to select multiple parcels with status "Pending Pickup" and assign them to a single field staff member in one action. The system must change each selected parcel's status to "Assigned" and send the field staff member one notification listing the number of parcels added. | S-03, Admin | Kevin (b00098368)|
| FR-05 | The system must allow an admin to create a field staff or admin account by entering the user's name, email, and role, plus the zone (emirate) for field staff. Zones are set by the admin, not allocated automatically. The system must generate a temporary password and require the user to change it at first login. | Admin | Kevin (b00098368)|


### Student 3 Contribution (Hammad - b00094579)

| **FR ID** | **Functional Requirement** | **Source Scenario/Stakeholder** | **Contributor** |
| --- | --- | --- | --- |
| FR-01 | The system should allow field staff member to view a list of parcels assigned to them for pickup, including senders address and parcel details | Field staff | Muhammad Hammad Khan (b00094579) |
| FR-02 | The system should allow a field staff member to view a list of parcels assigned to them for delivery including recipient address and parcel details. | Field staff | Muhammad Hammad Khan (b00094579) |
| FR-03 | The system should allow field staff to update the parcel delivery status (Picked Up, In Transit, Out for Delivery, Attempted, Delivered) from their assigned task list. When marking "Attempted", a note is mandatory; after the 3rd failed attempt the status becomes "Returned to Sender" and the customer is notified. | Field staff | Muhammad Hammad Khan (b00094579) |
| FR-04 | The system should allow a field staff member to confirm pick up for parcel from the sender and system should be able to record the pickup time. | Field staff | Muhammad Hammad Khan (b00094579) |
| FR-05 | The system shall require a field staff member to capture proof of delivery consisting of the recipient's full name, digital signature and Emirates ID before the system marks the parcel delivery complete. | Field staff | Muhammad Hammad Khan (b00094579) |

### Student 4 Contribution (Ahmed El Sabagh - b00095506)

| **FR ID** | **Functional Requirement** | **Source Scenario/Stakeholder** | **Contributor** |
| --- | --- | --- | --- |
| FR-01 | The system shall automatically send a notification to the customer whenever their parcel's status changes. | S-05, Customer | Ahmed (b00095506) |
| FR-02 | The system shall automatically send a notification to the assigned field staff member when new pickup or delivery tasks are assigned to them; a bulk assignment sends one summary notification. | S-05, Field Staff | Ahmed (b00095506) |
| FR-03 | The system shall record a timestamped status-history entry (status, timestamp, responsible actor/location) for a parcel every time its status changes. | S-05, Customer/Field Staff | Ahmed (b00095506) |
| FR-04 | The system shall allow a customer (by tracking number) and an administrator (by parcel ID) to view the complete chronological, timestamped status history of a parcel. | S-05, Customer, Administrator | Ahmed (b00095506) |
| FR-05 | The system shall display a real-time alert on the administrator dashboard when a parcel that has not been delivered has had no status change for 48 hours, and shall set the parcel's status to "Delayed". | S-05, Administrator | Ahmed (b00095506) |
