# Letter to Director: Rioel Vehicle Tracker Concept

Oct 13, 2026 · @Rimsha Shakya

Dear \[Director's name\],

I am writing to share a concept project I have been developing during my internship at ACM Removals, as part of my studies at Yoobee. I would like your feedback on it and, if you agree it has value, your support to test it properly.

While working with our vehicle GPS exports, I noticed that turning raw trip data into payroll-ready start and finish times takes a lot of manual checking. The reports come in several formats, vehicles are shared or swapped, some workers travel in a colleague's vehicle, and a missing trip or a stop at home can easily shift a worker's hours by a quarter of an hour or more.

My concept is the **Rioel Vehicle Tracker**, a web dashboard that reads our GPS exports and works out each driver's IN and OUT time for every day. Key parts of it:

- It accepts our existing report formats (individual Excel files, the Summary Trip Report in Excel or PDF) and combines them without double-counting trips.
- It applies our pay rules: a fortnightly pay period, a 30-minute break only on shifts over four hours, and long-travel and depot-return rules.
- It produces a fortnightly timesheet report in Word and HTML, with the arrival address, leaving address and site for each day.
- It checks fuel purchases and written timesheets against the GPS data, and matches workers without a vehicle to the supervisor's schedule.
- Every IN and OUT carries a confidence rating and a plain-English reason, so staff can see why a time was chosen. Doubtful days go into a review queue where an admin can verify them or correct a time, with the reason recorded.

I believe this could save time for whoever prepares payroll, make hours more consistent from one fortnight to the next, and give us a clear record to point to if a worker or auditor questions a time. It never hides uncertainty: where the GPS data is incomplete, the dashboard says so and asks for a person to confirm. It is a decision-support tool, and a person still approves the final hours.

I should be clear that this is a concept built and tested on sample and uploaded GPS data. It has not yet been validated against a full fortnight of real payroll, and I have not measured the time it saves.

With your permission, I would like to:

1. Demonstrate the dashboard to you in a short meeting, at a time that suits you.
2. Run it alongside the current process for one pay period, comparing its hours with the hours actually paid.
3. Report back on where it matched, where it differed, and whether it is worth developing further.

I would also appreciate your guidance on handling staff location data, so the project respects privacy and company policy. Thank you for your time and for the opportunity to learn at ACM Removals.

Kind regards,

Rimsha Shakya Intern, ACM Removals (Yoobee) rimshashakya11@gmail.com
