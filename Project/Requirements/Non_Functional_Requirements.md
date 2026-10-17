# Non-Functional Requirements

### Student 1 Contributions

| **NFR ID** | **Category** | **Non-Functional Requirement** | **Contributor** |
| --- | --- | --- | --- |
| NFR-01 | Performance | The tracking page shall show the current status and expected delivery window within 2 seconds under normal 4G/broadband conditions (at least 10 Mbps) for at least 95% of requests when up to 50 users are using the system at the same time. | Sarhan (b00100669) |
| NFR-02 | Performance | The login process shall finish within 3 seconds under normal 4G/broadband conditions (at least 10 Mbps) for at least 95% of login attempts when up to 50 users are using the system at the same time. | Sarhan (b00100669) |
| NFR-03 | Usability | A first-time customer shall be able to register and book a parcel in under 5 minutes without any instructions, verified by a test with at least 5 people who have not used the system before. | Sarhan (b00100669) |
| NFR-04 | Usability | Errors on the registration and booking forms shall be shown next to the field that has the problem, in plain words that say what to fix, and the other data already typed shall be kept. This shall be verified by submitting each form with invalid data. | Sarhan (b00100669) |
| NFR-05 | Reliability | If the connection is lost while a customer is submitting a booking or the system is updating a parcel's status, the system shall not save a partial or duplicate record, verified by disconnecting the network mid-submission during testing. | Sarhan (b00100669) |

### Student 2 Contributions
| **NFR ID** | **Category** | **Non-Functional Requirement** | **Contributor** |
| --- | --- | --- | --- |
| NFR-01 | Security | The system shall store 100% of all user passwords using a salted, adaptive hashing algorithm (e.g., bcrypt or Argon2). Passwords shall never be stored, logged, or transmitted in plain text. | Kevin (b00098368)|
| NFR-02 | Security | After 30 minutes of inactivity, the system shall terminate the user's session, and all data between the browser and the server shall be transmitted over HTTPS. Protected actions (such as booking or updating status) use the existing authenticated session and do not ask for a new login; once the session expires, the user must log in again. | Kevin (b00098368)|
| NFR-03 | Maintainability | Notification channels (email/SMS) and the delay threshold (48 hours) shall be stored in configuration, so they can be changed without modifying or redeploying code. | Kevin (b00098368)|
| NFR-04 | Security | The system shall lock a user account for 15 minutes after 4 consecutive failed login attempts and shall show the same generic error message ("Invalid email or password") for a wrong email and a wrong password. | Kevin (b00098368)|
| NFR-05 | Maintainability | The system shall write every unhandled error to an error log with a timestamp, the affected function, and the error message, so that a developer can locate the failing function from the log entry. | Kevin (b00098368)|


### Student 3 Contribution (Hammad - b00094579)

| **NFR ID** | **Category** | **Non-Functional Requirement** | **Contributor** |
| --- | --- | --- | --- |
| NFR-01 | Reliability | If a field staff member loses network connection after submitting a status update, the app should queue the update locally and try to do it after 30 sec of reconnecting with no data loss. | Muhammad Hammad Khan (b00094579) |
| NFR-02 | Reliability | Proof-of-delivery data (recipient name, signature, Emirates ID) must be stored locally on the device, and if upload fails, the app retries up to 3 times before prompting a manual sync. | Muhammad Hammad Khan (b00094579) |
| NFR-03 | Reliability | A status update submitted by the field staff should reflect in customer and admin views within 5 sec under normal 4G/broadband conditions (at least 10 Mbps). | Muhammad Hammad Khan (b00094579) |
| NFR-04 | Portability | The field staff interface **must** function properly on Android 10+ and iOS 15+ without any loss of its core functionalities. | Muhammad Hammad Khan (b00094579) |
| NFR-05 | Portability | The field staff interface should render correctly on mobiles, tablets and desktops (screen widths 360px to 1920px) without any loss of usability. | Muhammad Hammad Khan (b00094579) |

### Student 4 Contribution (Ahmed El Sabagh - b00095506)

| **NFR ID** | **Category** | **Non-Functional Requirement** | **Contributor** |
| --- | --- | --- | --- |
| NFR-01 | Scalability | The notification service shall support up to 10,000 status-change notifications per hour with a queuing delay of no more than 2 minutes. | Ahmed (b00095506) |
| NFR-02 | Scalability | The tracking-history store shall support at least 1,000,000 parcel history records without the tracking page's load time exceeding 2 seconds. | Ahmed (b00095506) |
| NFR-03 | Robustness | If an outbound notification channel is temporarily unavailable, the system shall queue the notification and retry for up to 24 hours before marking it failed. | Ahmed (b00095506) |
| NFR-04 | Robustness | A failure in the notification-sending component shall not prevent a status-history entry from being recorded; the two shall be independent. | Ahmed (b00095506) |
| NFR-05 | Scalability | The administrator dashboard shall support at least 50 concurrent administrators viewing live delay alerts with no more than a 3-second increase in refresh latency. | Ahmed (b00095506) |
