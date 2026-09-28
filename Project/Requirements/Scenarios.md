# Scenarios

### Student 1 Contributions (Sarhan)

| **Scenario ID** | **Scenario Title** | **Actor/Stakeholder** | **Scenario Description** |
| --- | --- | --- | --- |
| S-01 | Customer registers and books a parcel | Customer (Abdulla Qayed Al Ahli) | Abdulla registers with his name, email and phone number. He logs in and books a parcel to Omar Khalid in Abu Dhabi, entering the parcel's weight, size and description. The system confirms the booking and gives him a tracking number, for example CPT-000123. |
| S-02 | Customer tracks a booked parcel | Customer (Abdulla Qayed Al Ahli) | The next day Abdulla goes to the tracking page and types the tracking number wrongly as CPT-000132. The system tells him no parcel was found with that number. He corrects it to CPT-000123 and sees the current status "Picked Up" along with the earlier status "Booked". Later that day he checks again and sees the status "Out for Delivery". |

### Student 2 Contributions (Kevin)
| **Scenario ID** | **Scenario Title** | **Actor/Stakeholder** | **Scenario Description** |
| --- | --- | --- | --- |
| S-03 | Admin assigning parcel to field staff | Admin | 1. Amina, an admin, logs into the system using her admin credentials.<br>2. She views the list of parcels with status "Pending Pickup" that haven't yet been assigned to a field staff member.<br>3. She reviews the available field staff and their current workload (e.g., Ahmed – 12 parcels assigned, zone: Abu Dhabi; Fatima – 8 parcels assigned, zone: Dubai).<br>4. Amina selects Alex's parcel (CPT-2026-000482, destined for Al Manara) and assigns it to Ahmed, since it falls within his delivery zone.<br>5. The system updates the parcel's status to "Assigned" and notifies Ahmed that a new parcel has been added to his delivery list.<br>6. Amina repeats this process for the remaining unassigned parcels. |


### Student 3 Contribution (Hammad - b00094579)

| **Scenario ID** | **Scenario Title** | **Actor/Stakeholder** | **Scenario Description** |
| --- | --- | --- | --- |
| S-02 | A field staff member delivery round | Field staff | Let’s say field staff member abc logs into the field staff at the start of his shift or job day, and he sees 2 parcels assigned to him for pickup and 3 for delivery. He go to sender house to pickup the parcel with some parcel number, he collects it and then confirm the pickup in the app which also record the timestamp and update the status. Then he arrives to the delivery address and give parcel to the reciver and get proof of parcel delivery like Emirates id or signatures which auto update the delivery status to delivered and also trigger customer notification. And lets say on his next delivery, if no one answers, he marks the delivery as attempted and should add the note to retry any other time or next day. |

### Student 4 Contribution (Ahmed El Sabagh - b00095506)

| **Scenario ID** | **Scenario Title** | **Actor/Stakeholder** | **Scenario Description** |
| --- | --- | --- | --- |
| S-01 | Delayed Parcel Triggers Alerts and History Log | Customer, Field Staff, Administrator | Ahmed books a parcel from Dubai to Sharjah. The system generates tracking number TRK-88214 and sends Ahmed a booking confirmation notification. Field staff member Maria is assigned the pickup and is notified. Maria picks up the parcel, and the system logs a "Picked Up" status entry with a timestamp. As the parcel moves through the network, its status changes to "In Transit" and then "Out for Delivery", each change notifies Ahmed and appends a new tracking-history entry. The delivery runs 6 hours past its expected window, so the system raises a delay alert on the administrator dashboard. Maria delivers the parcel and submits proof of delivery; the system logs the final "Delivered" status and notifies Ahmed, who later reviews the complete timestamped history. |
