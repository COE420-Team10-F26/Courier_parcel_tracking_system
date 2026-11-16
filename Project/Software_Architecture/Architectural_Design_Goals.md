# Architectural Design Goals

**Project:** Courier Parcel Tracking System (Team 10)
**Lab:**     8

| Field | G1 | G2 | G3 |
| --- | --- | --- | --- |
| **Goal ID** | G1 | G2 | G3 |
| **Design Goal** | Modularity / Maintainability | Reliability | Security |
| **Description** | The system is split into components with clear responsibilities and narrow interfaces, so configuration and error handling can be changed or fixed in one place and components stay independent of each other. | The system does not lose or corrupt data when something fails. Interrupted submissions leave no partial or duplicate records, field updates and proof of delivery are queued and retried, and failed notifications are retried. | Passwords, sessions and accounts are protected so that only legitimate users can access the system. |
| **Related Requirement** | NFR-08, NFR-10, NFR-19 | NFR-05, NFR-11, NFR-12, NFR-18 | NFR-06, NFR-07, NFR-09 |
| **Priority** | Medium | High | High |
| **Justification** | NFR-08 requires the notification channels and the 48-hour delay threshold to be changeable without code changes, and NFR-10 requires every unhandled error to be logged. Both are easier to meet when configuration and logging are handled in well-bounded components. NFR-19 requires status-history logging and notification sending to be independent, so Tracking & Monitoring (CO5) and Notification (CO6) must not depend on each other. | Customers and admins rely on parcel statuses and proof of delivery being correct. NFR-05, NFR-11 and NFR-12 stop connection losses from causing duplicate or missing records, and NFR-18 keeps notifications from being lost when a channel is down. Keeping status handling in CO5 and notification retries in CO6 limits the effect of a failure to one component. | The system stores user accounts and passwords. NFR-06 (salted hashing), NFR-07 (30-minute session timeout and HTTPS) and NFR-09 (account lock after 4 failed logins) protect them. Keeping registration, login, passwords and account locking in Account Management (CO1), with user data in the Users database (D1), keeps these controls in one place. |
