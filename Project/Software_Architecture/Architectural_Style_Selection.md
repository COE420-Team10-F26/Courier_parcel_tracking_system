# Architectural Style Selection

**Project:** Courier Parcel Tracking System (Team 10)
**Lab:**     8
**Contributor:** Ahmed El Sabagh (b00095506)

## Selected Style: Layered Architecture


| Layer | Components | What the layer does |
| --- | --- | --- |
| Application | CO1 Account Management, CO2 Administration & Assignment, CO3 Field Delivery | The workflows of each actor: the customer's account and requests (CO1), the admin's user, zone and assignment work (CO2), and the field staff member's pickups and deliveries (CO3). The customer, admin and field staff apps call this layer. |
| Domain | CO4 Parcel Management | The core business rules about a parcel: booking, tracking numbers, recipient details, every status change, the delivery window and the 48-hour delay check. |
| Services | CO5 Tracking & Monitoring, CO6 Notification | Shared services that the layers above use: parcel statuses, status history and delay alerts (CO5), and building and sending notifications with retries (CO6). |
| External | Database (D1 Users, D2 Parcels, D3 Status History), Email/SMS Gateway | Outside the system. Each component reads and writes its data in the Database; only CO6 uses the Email/SMS Gateway. |

We use the **relaxed** form of layering: a layer can use any layer below it, not only the one directly below. For example, CO2 calls CO5 and CO6 directly to show delay alerts and send the staff member's assignment summary, without passing through CO4.

## Features of the Style

- **Components are grouped into layers by how general they are.** Actor-specific workflows are at the top, parcel rules in the middle and shared services at the bottom.
- **Dependencies go one way.** A component only calls components in lower layers (or, in the Application layer, in the order CO3 → CO2 → CO1). A lower layer never knows about the layers above it.
- **Each layer offers its services through interfaces.** Components talk only through the provided and required interfaces in `Component_Descriptions.md` (for example `IParcelService`, `IStatusHistory`, `INotification`), not by reaching into another component's classes.
- **Lower layers can be reused by every layer above.** CO4 serves all three Application components, CO5 serves CO2, CO3 and CO4, and CO6 serves CO2 and CO4.
- **A change in one layer stays in that layer** as long as its interfaces stay the same.

## Why It Fits Our Project

- **Three actors, one shared parcel.** Customers, admins and field staff each have their own workflow, but they all work on the same parcel. The layered style gives each actor their own Application component (CO1, CO2, CO3) and puts the shared parcel rules once in CO4, so booking, assigning and delivering cannot each change a status in a different way. CO4 is the only place a parcel's status changes.
- **Every status change follows the same steps.** A status change always sets the status, records a history entry, resolves any active delay alert and sends a notification (Labs 6 and 7). Putting that sequence in CO4, on top of CO5 and CO6, means it is written once and every use case that changes a status (UC-03, UC-09, UC-14, UC-15, UC-16) gets it the same way.
- **History and notifications are services, not workflows.** Status history, delay alerts and notifications are used by several components and do not depend on who triggered them, so they belong in a bottom Services layer that anything above can use.
- **It matches our team and project size.** We are four students building one system. A layered design is simple to explain, draw and split between members, and it maps directly onto the use cases and classes we already have from Labs 3 to 7. More distributed styles (for example microservices) would add network calls, separate deployments and data copying that this project does not need.
- **It matches our earlier models.** The Level-0 DFD already separates account handling (P1), booking (P2), assignment (P3), status updates (P4) and tracking and notifications (P5), and the Lab 6 sequence diagrams already show the actor's object calling `Parcel`, which then calls status history and notifications. The layers follow the same direction of calls.

## How the Style Supports the Design Goals

| Goal | How the layered style supports it |
| --- | --- |
| **G1 Modularity / Maintainability** (NFR-08, NFR-10, NFR-19) | Each component has one clear responsibility and narrow interfaces that pass IDs and plain values, not the caller's objects (rule 1), so a component can change inside without affecting the others. The notification channels live only in CO6 and the 48-hour threshold only in CO5, so changing them in configuration (NFR-08) touches one place. Because CO5 and CO6 are in the same layer and do not depend on each other (rule 5), status-history logging and notification sending are independent, as NFR-19 requires. Errors can be logged per component with the component and function name (NFR-10), which makes the failing function easy to find. |
| **G2 Reliability** (NFR-05, NFR-11, NFR-12, NFR-18) | Each failure is handled in the layer that owns it, so it does not spread. A parcel and its recipient are created together inside CO4, so an interrupted booking can be rolled back as one unit with no partial or duplicate record (NFR-05). Queued status updates and proof of delivery are retried by CO3 on the field staff device (NFR-11, NFR-12) and only reach CO4 once they succeed. A down Email/SMS channel only affects CO6, which queues and retries for up to 24 hours (NFR-18), while CO4 and CO5 carry on recording the status change. |
| **G3 Security** (NFR-06, NFR-07, NFR-09) | Login, passwords, sessions and account locking are only in CO1, so there is one place to apply salted hashing (NFR-06), the 30-minute session timeout (NFR-07) and the lock after 4 failed logins (NFR-09). Because interfaces pass IDs (rule 1), passwords and session data are never passed to other components as parameters, and the `User` objects CO1 returns to CO2 expose no password-hash or session fields. Lower layers only receive a `userId`, and CO6 only reads a user's email and phone from D1. |

## Pros and Cons

**Pros**
- **Easy to understand and maintain.** Each layer has a clear job, so a developer knows where a change belongs (G1).
- **Changes stay local.** A component can be rewritten behind the same interface without changing the components above it.
- **Shared rules are written once.** The status-change steps, history and notifications are not repeated in every workflow.
- **Failures are contained.** A problem in a lower service, such as a notification channel being down, does not stop the layers above (G2).
- **Easy to test layer by layer.** CO5 and CO6 have no dependencies on the other components, so they can be tested on their own, and the layers above can be tested with stubs of the interfaces below.
- **Easy to split between team members.** Each member can own a component and agree only on its interfaces.

**Cons**
- **Some requests only pass through a layer.** For example, the customer's tracking request goes through CO1 to CO4 without much happening in CO1. This adds a little code and an extra call.
- **Performance overhead.** Because interfaces pass IDs instead of objects, a component sometimes has to look the parcel up again in a lower layer. This could matter for NFR-01 (tracking page within 2 seconds) and NFR-17 (1,000,000 history records), so lookups by `parcelId` and tracking number need database indexes.
- **Some changes cut across all layers.** Adding a new parcel status touches `ParcelStatus` in CO5, the rules in CO4 and the screens in CO1 to CO3.
- **The one-way rule limits some calls.** A lower layer cannot call back up, so CO4 cannot tell CO2 that a parcel became Delayed. CO2 instead reads the active alerts from CO5 when the admin dashboard refreshes (NFR-20). Inside the Application layer, CO1 and CO2 cannot call CO3.
- **The layers are deployed together.** Scaling only the notification service for NFR-16 (10,000 notifications per hour) is harder than in a distributed style. CO6's outgoing queue helps, because sending happens outside the request that changed the status.

## Rules That Keep Dependencies Pointing Down

1. **Interfaces between components pass IDs** (`parcelId`, `userId`, `customerId`, `staffId`, `assignmentId`) **and plain values, not objects from the calling component.** An operation may return the providing component's own classes (for example `IParcelService.getParcel()` returns a `Parcel`), since the caller already depends on that component. A lower component never needs a class from a higher one, so it never depends upward. For example, `IDelayAlert.evaluate()` takes the time of the last status change instead of a `Parcel`.
2. **Inside the Application layer the only direction is CO3 → CO2 → CO1.** Field Delivery reads its assignments from CO2 (`IAssignmentQuery`), and CO2 uses CO1's account operations (`IUserAdministration`). CO1 never calls CO2 or CO3, and CO2 never calls CO3, so there are no cycles between the three.
3. **`ParcelStatus` is in CO5,** the lowest layer, so CO2, CO3 and CO4 can all use the statuses without depending on a component above them.
4. **The 48-hour delay check is in CO4.** `Parcel` holds the time of its last status change, so CO4 runs the check, sets the parcel to Delayed and calls CO5 (`IDelayAlert`) to raise or resolve the alert, the same as in our Lab 6 sequence diagrams. CO5 never calls back into CO4.
5. **CO5 and CO6 do not depend on each other** (NFR-19). CO6 gets a user's contact details by `userId` from D1, and the status is passed to it as text, so it never uses CO5's classes. CO4 calls CO5 and CO6 separately, so a notification failure cannot block a history entry.

With these rules every dependency in the component diagram is either downward or one-way inside the Application layer:

| From | Depends on | Direction |
| --- | --- | --- |
| CO1 | CO4 | Down (Application → Domain) |
| CO2 | CO1 | One-way inside Application |
| CO2 | CO4, CO5, CO6 | Down |
| CO3 | CO2, CO1 | One-way inside Application |
| CO3 | CO4, CO5 | Down |
| CO4 | CO5, CO6 | Down (Domain → Services) |
| CO5 | none (Database only) | n/a |
| CO6 | Email/SMS Gateway (and D1) | Down to external |
