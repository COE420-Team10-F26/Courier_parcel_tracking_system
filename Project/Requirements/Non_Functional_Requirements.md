# Non-Functional Requirements

### Student 1 Contributions

| **NFR ID** | **Category** | **Non-Functional Requirement** | **Contributor** |
| --- | --- | --- | --- |
| NFR-01 | Performance | The tracking page shall show the current status within 2 seconds for at least 95% of requests. | Sarhan (b00100669) |
| NFR-02 | Performance | The login process shall finish within 3 seconds for at least 95% of login attempts. | Sarhan (b00100669) |
| NFR-03 (draft) | Usability | Registration and booking should be quick for a first-time user. Need to pin down a measurable time and how it's tested. | Sarhan (b00100669) |
| NFR-04 (draft) | Usability | Form errors shown clearly next to the field. Need to word this so it's verifiable. | Sarhan (b00100669) |
| NFR-05 (draft) | Reliability | If the system loses connection during a booking or status update, no partial or duplicate record should be saved. Still need to phrase this as something measurable/testable. | Sarhan (b00100669) |

### Student 2 Contributions
| **NFR ID** | **Category** | **Non-Functional Requirement** | **Contributor** |
| --- | --- | --- | --- |
| NFR-01 | Security | The system shall store 100% of all user passwords using a salted, adaptive hashing algorithm (e.g., bcrypt or Argon2). Passwords shall never be stored, logged, or transmitted in plain text. | Kevin (b00098368)|
| NFR-02 | Security | After 30 mins of inactivity, the system should terminate the user’s session and all data between the browser and the server would be transmitted through HTTPs. | Kevin (b00098368)|
| NFR-03 | Maintainability | All code changes shall be made on a feature branch and merged into the main branch only after review by at least one other team member. Each merge shall have a meaningful commit message. | Kevin (b00098368)|
| NFR-04 | Security | The system shall lock a user account for 15 minutes after 4 consecutive failed login attempts and shall show the same generic error message ("Invalid email or password") for a wrong email and a wrong password. | Kevin (b00098368)|
| NFR-05 | Maintainability | The system shall write every unhandled error to an error log with a timestamp, the affected function, and the error message, so that a developer can locate the failing function from the log entry. | Kevin (b00098368)|