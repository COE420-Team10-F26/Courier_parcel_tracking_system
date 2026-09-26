# Functional Requirements

### Student 1 Contributions (Sarhan)

| **FR ID** | **Functional Requirement** | **Source Scenario/Stakeholder** | **Contributor** |
| --- | --- | --- | --- |
| FR-01 | The system shall allow a new user to register as a customer by entering full name, email, phone number and a password of at least 8 characters, and shall reject the registration if the email is already registered or any field is empty. | S-01, Customer | Sarhan (b00100669) |
| FR-02 | The system shall log in a registered user using email and password, open the dashboard that matches the user's role (customer, field staff or administrator), and show a general error message when the email or password is wrong. | S-01, S-02, Customer | Sarhan (b00100669) |
| FR-03 | The system shall allow a logged-in customer to book a parcel by entering the recipient's name, phone number and address and the parcel's weight, dimensions and description, and shall not confirm a booking when a mandatory field is empty or the weight is not a positive number. | S-01, Customer | Sarhan (b00100669) |
| FR-04 | The system shall automatically generate a unique tracking number in the format CPT-XXXXXX for every confirmed booking and display it to the customer on the booking confirmation page. | S-01, Customer | Sarhan (b00100669) |
| FR-05 | The system shall allow a logged-in customer to enter a tracking number and shall show the parcel's current status and its full status history with the date and time of each update, or a "parcel not found" message when the number does not exist. | S-02, Customer | Sarhan (b00100669) |

### Student 2 Contributions (Kevin)

| **FR ID** | **Functional Requirement** | **Source Scenario/Stakeholder** | **Contributor** |
| --- | --- | --- | --- |
| FR-01 | The system must allow an admin to view a list of all registered users showing each user's name, email, role (customer, field staff, admin), and account status (active/deactivated), and to search by name or email and filter by role. | Admin | Kevin (b00098368)|
| FR-02 | The system must allow an admin to deactivate and reactivate customer and field staff accounts. A deactivated user must be unable to log in, and that user's existing parcel records must be retained. | Admin | Kevin (b00098368) |
| FR-03 | The system must display, on the admin dashboard, a list of all parcels along with their current status and assigned field staff member. | S01, Admin | Kevin (b00098368)|
| FR-04 | The system must allow an admin to select multiple parcels with status "Pending Pickup" and assign them to a single field staff member in one action. The system must change each selected parcel's status to "Assigned" and send the field staff member one notification listing the number of parcels added. | S01, Admin | Kevin (b00098368)|
| FR-05 | The system must allow an admin to create a field staff or admin account by entering the user's name, email, and role. The system must generate a temporary password and require the user to change it at first login. | Admin | Kevin (b00098368)|
