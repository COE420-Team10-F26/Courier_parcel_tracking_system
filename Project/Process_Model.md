# Software Process Model

Model selection: Incremental model

Justification: The parcel tracking system splits naturally into separate working chunks, registration and login, booking, tracking numbers, status updates, notifications, the admin side. Building it in increments means the team gets a working piece to test early instead of writing every module and hoping it all fits together right before the deadline. The requirements are already laid out fairly clearly in the project description, so there isn't much risk of the whole thing changing halfway through. That rules out needing something as heavy as full Scrum with sprints and roles. At the same time doing it all in one long Waterfall style build is risky for a four person team with a fixed deadline. If something breaks near the end there's no room left to fix it.

Model overheads: Testing has to happen after every increment instead of just once at the very end, and that adds up as the semester goes on. Deciding how to group the features into increments also takes planning time before anyone actually starts coding, time a straight Waterfall build wouldn't need upfront.

Strategy for managing overheads: Set a fixed weekly check in where each increment gets tested and merged before the next one starts, so the testing time gets spread out instead of piling up near submission. Group the features into increments in the first week, before any code gets written, so that planning cost only happens once.

Model drawbacks: The core structure and database need to be decided early since every increment builds on top of them. That leaves little room to majorly restructure things once the team is a few increments deep. If a design flaw only shows up later, fixing it can mean redoing parts of already finished increments instead of just the current one.

Strategy for addressing drawbacks: Spend real time on the schema and overall structure before writing any feature code, not just a quick sketch. Treat any change to that structure as a team decision instead of something one person edits on their own branch. If a schema change turns out to be necessary partway through, whoever finds the issue flags it to the group before merging anything, so nobody's work breaks without warning.
