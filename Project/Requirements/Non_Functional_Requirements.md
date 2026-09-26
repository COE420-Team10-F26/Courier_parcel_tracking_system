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