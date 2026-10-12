# Integration and Consistency Check

## Data Flow Modeling

| Check | Result | Notes |
| --- | --- | --- |
| The Context Diagram represents the proposed system correctly | Yes | The whole Courier Parcel Tracking System is shown as one process (0) with only external entities and data flows, no internal processes or data stores. |
| External entities are consistent with UML Use Case actors | Yes | Customer, Field Staff and Admin are the same three actors used in the Lab 3 Use Cases and Use Case Diagram. |
| Level-0 DFD uses the same relevant external entities | Yes | Customer, Field Staff and Admin appear in both diagrams, and no new entity is added in Level-0. |
| Context Diagram and Level-0 DFD are balanced | Yes | All 22 external data flows in the Context Diagram appear in the Level-0 DFD with the same name and direction (see the table below). |
| Level-0 contains at least five meaningful processes | Yes | P1 Manage Accounts and Login, P2 Book Parcel, P3 Assign/Monitor Parcels, P4 Update Parcel Status, P5 Tracking and Notifications. |
| Level-0 contains at least two relevant data stores | Yes | D1 Users, D2 Parcels, D3 Status History. |
| Core Processes Matrix contains the same major processes as the Level-0 DFD | Yes | P1 to P5 have the same numbers and names in the matrix and the Level-0 DFD. |
| DFD processes are consistent with Functional Requirements and Use Cases | Yes | See the mapping table below. |

### Balancing: Context Diagram vs Level-0 DFD

| Entity | Data Flow | Direction | Level-0 Process |
| --- | --- | --- | --- |
| Customer | Registration details | Customer → System | P1 |
| Customer | Login credentials | Customer → System | P1 |
| Customer | Booking details | Customer → System | P2 |
| Customer | Tracking number | Customer → System | P5 |
| Customer | Booking confirmation and tracking number | System → Customer | P2 |
| Customer | Parcel status and history | System → Customer | P5 |
| Customer | Status notification | System → Customer | P5 |
| Field Staff | Login credentials | Field Staff → System | P1 |
| Field Staff | Status update | Field Staff → System | P4 |
| Field Staff | Pickup confirmation | Field Staff → System | P4 |
| Field Staff | Proof of delivery | Field Staff → System | P4 |
| Field Staff | Assigned pickups and deliveries | System → Field Staff | P3 |
| Field Staff | Assignment notification | System → Field Staff | P3 |
| Admin | Login credentials | Admin → System | P1 |
| Admin | Staff account details | Admin → System | P1 |
| Admin | Account status change | Admin → System | P1 |
| Admin | Parcel assignment | Admin → System | P3 |
| Admin | Parcel ID | Admin → System | P5 |
| Admin | User list | System → Admin | P1 |
| Admin | Parcel list | System → Admin | P3 |
| Admin | Delay alert | System → Admin | P3 |
| Admin | Parcel tracking history | System → Admin | P5 |

All the other flows in the Level-0 DFD (for example Status change from P4 to P5, and the reads and writes to D1, D2 and D3) are internal, so they don't appear in the Context Diagram.

### Processes vs Functional Requirements and Use Cases

| Process | Functional Requirements | Use Cases |
| --- | --- | --- |
| P1 Manage Accounts and Login | Sarhan FR-01, FR-02; Kevin FR-01, FR-02, FR-05 | Register Account, Log In, View and Search User Accounts, Deactivate/Reactivate User Account, Create Staff/Admin Account, Change Temporary Password |
| P2 Book Parcel | Sarhan FR-03, FR-04 | Book Parcel, Generate Tracking Number |
| P3 Assign/Monitor Parcels | Kevin FR-03, FR-04; Ahmed FR-02, FR-05 | View All Parcels on Dashboard, Assign Parcels to Field Staff, Receive Assignment Notification, View Dashboard Delay Alerts |
| P4 Update Parcel Status | Hammad FR-01 to FR-05; Ahmed FR-03 | View Assigned Pickups, View Assigned Deliveries, Update Parcel Status, Confirm Pickup, Record Proof of Delivery |
| P5 Tracking and Notifications | Sarhan FR-05; Ahmed FR-01, FR-04 | Track Parcel, Receive Status Notification, View Parcel Tracking History, Search Tracking History by Parcel ID |

Note: the Lab 3 delay requirement (Ahmed FR-05) says a parcel is delayed when it goes past its expected delivery window. For Lab 4 we made this a specific rule: a parcel with no status change for 48 hours is flagged as delayed. P3 in the Core Processes Matrix and T6 in the WBS both use this rule.

## Project Scheduling

| Check | Result | Notes |
| --- | --- | --- |
| WBS activities represent actual project work | Yes | The 12 tasks are the actual modules of this system (accounts, booking, assignment, status updates, tracking and notifications), not a generic Analysis → Design → Coding → Testing list. |
| Gantt Chart activities correspond to WBS activities | Yes | Same 12 tasks, durations and dependencies as the WBS, entered in GanttProject. |
| Task dependencies are logically defined | Yes | For example, booking (T5) needs the database (T2) and login (T4), status updates (T7) need booking (T5), and integration (T10) waits for every module (T6, T8, T9). |
| Time estimates are realistic and project-specific | Yes | Between 2 and 7 days per task. Bigger modules and testing get more time. |
| Expected Times are correctly calculated | Yes | Every task has one duration estimate from the WBS, used as E in the network diagram. |
| Network diagram reflects the WBS dependencies | Yes | Every arrow in the network diagram matches a predecessor in the WBS. |
| ES, EF, LS, LF and Slack calculations are correct | Yes | Checked by hand and with GanttProject (see Project_Scheduling/Critical_Path.md). |
