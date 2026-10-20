# Ahmed El Sabagh — Class Inventory

| Class ID | Class | Purpose | Related use cases | Contributor |
| --- | --- | --- | --- | --- |
| C16 | StatusHistoryEntry | Stores one timestamped parcel status transition, including an optional previous status, required new status, optional actor, and location. | UC-19, UC-21 | Ahmed El Sabagh (b00095506) |
| C17 | Notification (abstract) | Shared notification content, recipient, delivery timestamps, and delivery/retry state for all notification types. | UC-17, UC-18 | Ahmed El Sabagh (b00095506) |
| C18 | StatusNotification | Notifies a customer of a parcel status transition. | UC-17 | Ahmed El Sabagh (b00095506) |
| C19 | AssignmentNotification | Sends the field staff member one summary notification for a bulk assignment. | UC-18 | Ahmed El Sabagh (b00095506) |
| C20 | DelayAlert | Represents an active or resolved dashboard alert for an undelivered parcel with no status change for 48 hours. | UC-20 | Ahmed El Sabagh (b00095506) |

## Modeling notes

- `StatusNotification` and `AssignmentNotification` inherit from `Notification`.
- Each bulk `Assignment` has exactly one `AssignmentNotification` summary.
- `Notification` uses the shared `User` class for its recipient; status notifications also reference `Parcel`, and assignment notifications reference `Assignment`.
- `StatusHistoryEntry` belongs to a `Parcel`, records a `ParcelStatus`, and may reference the `User` who made the change. The user reference is optional for system-generated changes.
- Notification delivery retries may continue for up to 24 hours. History recording is independent of notification delivery.
- A parcel is delayed when it is undelivered and has had no status change for 48 hours.
- `User`, `Parcel`, `Assignment`, and `ParcelStatus` are shown as name-only reference classes because their details are defined by other team members.
