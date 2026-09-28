# Non-Functional Requirements

### Student 1 Contributions

| **NFR ID** | **Category** | **Non-Functional Requirement** | **Contributor** |
| --- | --- | --- | --- |
| NFR-01 | Performance | The tracking page shall show the current status and status history within 2 seconds for at least 95% of requests when up to 50 users are using the system at the same time. | Sarhan (b00100669) |
| NFR-02 | Performance | The login process shall finish within 3 seconds for at least 95% of login attempts when up to 50 users are using the system at the same time. | Sarhan (b00100669) |
| NFR-03 | Usability | A first-time customer shall be able to register and book a parcel in under 5 minutes without any instructions, verified by a test with at least 5 people who have not used the system before. | Sarhan (b00100669) |
| NFR-04 | Usability | Errors on the registration and booking forms shall be shown next to the field that has the problem, in plain words that say what to fix, and the other data already typed shall be kept. This shall be verified by submitting each form with invalid data. | Sarhan (b00100669) |
| NFR-05 | Reliability | If the connection is lost while a customer is submitting a booking or the system is updating a parcel's status, the system shall not save a partial or duplicate record, verified by disconnecting the network mid-submission during testing. | Sarhan (b00100669) |

### Student 2 Contributions
| **NFR ID** | **Category** | **Non-Functional Requirement** | **Contributor** |
| --- | --- | --- | --- |
| NFR-01 | Security | The system shall store 100% of all user passwords using a salted, adaptive hashing algorithm (e.g., bcrypt or Argon2). Passwords shall never be stored, logged, or transmitted in plain text. | Kevin (b00098368)|
| NFR-02 | Security | After 30 mins of inactivity, the system should terminate the user’s session and all data between the browser and the server would be transmitted through HTTPs. | Kevin (b00098368)|
| NFR-03 | Maintainability | All code changes shall be made on a feature branch and merged into the main branch only after review by at least one other team member. Each merge shall have a meaningful commit message. | Kevin (b00098368)|
| NFR-04 | Security | The system shall lock a user account for 15 minutes after 4 consecutive failed login attempts and shall show the same generic error message ("Invalid email or password") for a wrong email and a wrong password. | Kevin (b00098368)|
| NFR-05 | Maintainability | The system shall write every unhandled error to an error log with a timestamp, the affected function, and the error message, so that a developer can locate the failing function from the log entry. | Kevin (b00098368)|


### Student 3 Contribution (Hammad - b00094579)

| **NFR ID** | **Category** | **Non-Functional Requirement** | **Contributor** |
| --- | --- | --- | --- |
| NFR-01 | Reliability | If a field staff member loses network connection after submitting a status update, the app should queue the update locally and try to do it after 30 sec of reconnecting with no data loss. | Muhammad Hammad Khan (b00094579) |
| NFR-02 | Reliability | Proof of delivery photo must be stored locally in the device and if upload fails, it should try 3 times to upload the proof again. | Muhammad Hammad Khan (b00094579) |
| NFR-03 | Reliability | A status update submitted by the field staff should reflect in customer and admin views within 5 sec under normal network conditions. | Muhammad Hammad Khan (b00094579) |
| NFR-04 | Portability | The field staff interface **must** function properly on both android and IOS without any loss of its core functionalities. | Muhammad Hammad Khan (b00094579) |
| NFR-05 | Portability | The field staff interface should render correctly on both, mobiles and tablets (may be even on like laptop or desktop) without any loss of usability. | Muhammad Hammad Khan (b00094579) |

### Student 4 Contribution (Ahmed El Sabagh - b00095506)

| **NFR ID** | **Category** | **Non-Functional Requirement** | **Contributor** |
| --- | --- | --- | --- |
| NFR-01 | Scalability | The notification service shall support up to 10,000 status-change notifications per hour with a queuing delay of no more than 2 minutes. | Ahmed (b00095506) |
| NFR-02 | Scalability | The tracking-history store shall support at least 1,000,000 parcel history records without the tracking page's load time exceeding 2 seconds. | Ahmed (b00095506) |
| NFR-03 | Robustness | If an outbound notification channel is temporarily unavailable, the system shall queue the notification and retry for up to 24 hours before marking it failed. | Ahmed (b00095506) |
| NFR-04 | Robustness | A failure in the notification-sending component shall not prevent a status-history entry from being recorded; the two shall be independent. | Ahmed (b00095506) |
| NFR-05 | Scalability | The administrator dashboard shall support at least 50 concurrent administrators viewing live delay alerts with no more than a 3-second increase in refresh latency. | Ahmed (b00095506) |
