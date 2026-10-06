# Exercise 3: Core Processes Matrix

### Required Format

| Process No. and Name | Input Data | Output Data | Rules |
| :--- | :--- | :--- | :--- |
| **P1 –** | | | |
| **P2 –** | | | |
| **P3 –** | | | |
| **P4 – Update Parcel Status** | Status update (Field Staff)<br>Pickup confirmation (Field Staff)<br>Proof of delivery: recipient name/signatures/ID (Field Staff)<br>Parcel records (D2 Parcels) | Updated parcel record, including pick time and proof of delivery (D2 parcels)<br>Status history entry (D3 status History) | • Only logged in field staff member can update a status, and only on parcels assigned to them, from their assigned task list.<br><br>• Statuses move forward in specific order:<br>Assigned -> Picked Up -> In Transit -> Out for Delivery -> Delivered.<br><br>• Skipping a step or going backwards is not allowed and a failed visit can be recorded as delivery attempted, after which the parcel returns to out for delivery.<br><br>• Confirming pickup changes the status to Picked Up and records the pickup time.<br><br>• A parcel can only be marked delivered after the proof of delivery is captured.<br><br>• Every status change updates the parcel's current status in D2 and it also creates a time stamp entry in D3 containing the status along with the timestamp and the responsible actor/location.<br><br>• P4 does not send notification itself. P5 reads D3 status history and then it sends the notification to the customer. |
| **P5 –** | | | |