QA-301 – Overtime Entry Feature Specification
1. Feature goal

The goal of this feature is to allow a site manager / supervisor to log overtime for a worker at the end of the day in a way that is simple to use, safe for payroll, and reliable enough to prevent incorrect salary calculation later.

This feature matters because overtime is not just another data field. In a payroll system, overtime directly affects a worker’s wages. If overtime is entered incorrectly, duplicated, lost, or not linked to the right worker/date, the worker may be underpaid or overpaid in that month’s payroll.

2. Users
Primary user
Site Manager / Supervisor entering overtime for workers from the construction site
Secondary users affected
Payroll operator / HR admin who relies on overtime data for salary processing
Worker / employee whose salary is affected by overtime entries
Developers / support team who need clear rules to debug and maintain the feature
3. Core user story

As a site manager, I want to record overtime for a worker by selecting the worker, overtime date, number of overtime hours, and reason, so that payroll reflects the worker’s actual extra work accurately.

4. Required fields for overtime entry

Each overtime record must include:

Worker / Employee
Selected from the employee list
Required
Overtime date
Required
Cannot be empty
Overtime hours
Required
Must be a valid positive number
Should support values such as 1, 2, 3.5, etc. if half-hours are allowed
Must not accept negative values
Reason for overtime
Required
Free-text explanation for why overtime was worked
Should be trimmed before saving
Optional metadata
created by / supervisor id
created timestamp
last updated timestamp
status if approval workflow exists later
5. Acceptance criteria
AC1 – Site manager can create a valid overtime entry

Given a site manager is on the overtime entry screen
When they select a valid worker, enter a valid overtime date, valid overtime hours, and a reason, then submit
Then the overtime record should be saved successfully
And the user should see a success confirmation
And the saved record should be visible in the overtime list or worker detail history.

AC2 – Worker is mandatory

Given the user opens the overtime form
When they submit without selecting a worker
Then the system must block submission
And show a clear validation message such as:
“Please select a worker.”

AC3 – Overtime date is mandatory

Given the user fills other fields but leaves the overtime date empty
When they submit
Then the system must block submission
And show an error message for the missing date.

AC4 – Overtime hours must be valid

Given the user enters overtime hours
When the value is empty, zero, negative, non-numeric, or exceeds the allowed limit
Then the system must reject the submission with a clear validation message.

Examples of invalid values:

blank
0
-2
abc
extremely large values like 999
AC5 – Reason is mandatory

Given the user fills worker, date, and hours
When the reason field is empty or only spaces
Then the system must block submission and ask for a valid reason.

AC6 – Duplicate overtime for the same worker and date must be handled safely

Given an overtime record already exists for a worker on a given date
When another overtime entry is attempted for the same worker and same date
Then the system must not silently create duplicate conflicting records
And one of the following must happen based on business rule:

either reject the duplicate with a clear message
or allow edit/merge behaviour explicitly
but it must never create hidden duplicate overtime that changes payroll incorrectly
AC7 – Overtime must be linked to a real employee

Given a user tries to submit overtime for a worker id that does not exist or is inactive
Then the backend must reject the request and return a clear error.

AC8 – Overtime should not be allowed for future dates unless business explicitly allows it

Given the user selects a future date
When the business rule does not allow future overtime entry
Then the system must reject the submission and explain that overtime can only be logged for completed work dates.

AC9 – Success message and saved data must be consistent

Given a successful overtime submission
When the record is stored
Then the values shown in the confirmation / list / detail view must match what was submitted
And the record should remain visible after page refresh.

AC10 – Mobile usability must be supported

Because site managers may enter overtime from a phone at the construction site:

the form must be usable on mobile width screens
fields must remain readable without horizontal scrolling
submit button must be visible and tappable
date and worker selection should be practical on smaller screens
AC11 – Failed submission must not silently lose user input

Given the user submits the form and the request fails due to server error or connection issue
Then the system should preserve the entered values on screen where possible
And show a clear error message instead of clearing the form without explanation.

6. Construction / payroll-specific edge cases

These are the edge cases I would explicitly discuss before development because they affect payroll accuracy, not just UI validation.

Edge case 1 – Monthly overtime cap

If a worker already has 55 overtime hours in the current month and a supervisor enters 7 more, but the policy allows only 60 hours per month, what should happen?

Possible rules:

reject the new entry completely
allow only the remaining 5 hours
allow entry but flag it for payroll/HR review

This must be defined before development because it changes payroll outcomes.

Edge case 2 – Two supervisors log overtime for the same worker on the same day

At large sites, two supervisors may accidentally log overtime for the same worker and date.
The system must define whether:

duplicates are blocked
duplicate attempts trigger a warning
or multiple entries are allowed but merged into one total

Without a rule here, payroll can double-count overtime.

Edge case 3 – Inactive or exited employee

If a worker has left the company or been marked inactive, overtime should not be accepted unless there is a specific backdated correction flow. Otherwise the payroll system may continue to include exited workers.

Edge case 4 – Backdated correction after payroll cut-off

What if the overtime being entered belongs to a previous payroll period that is already closed?
Should it:

be blocked,
require admin approval,
or roll into the next payroll cycle as an adjustment?

This must be clarified because it affects payslip accuracy and auditability.

Edge case 5 – Network failure during mobile submission

A site manager may submit overtime from a construction site with unstable connectivity.
If the request fails halfway, the system must avoid two bad outcomes:

user thinks the record was saved when it was not
user retries and creates duplicate overtime entries
Edge case 6 – Decimal overtime values

If the business allows 1.5 or 2.5 hours, the validation and payroll calculation must support decimals consistently across UI, API, and salary computation.

Edge case 7 – Overtime entered for a holiday or non-working day

This may be valid in construction payroll, but if it is allowed, payroll may use a different rate. If the business handles holiday overtime differently, the feature needs a rule for it.

7. Questions I would ask the PM before development starts

These are the questions I would ask to avoid mid-sprint requirement changes later.

Business rules
What is the maximum overtime allowed per day and per month?
Are decimal overtime values allowed, such as 1.5 hours?
Can overtime be entered for past dates? If yes, how far back?
Can overtime be entered for future dates?
Can overtime be entered for inactive / exited workers for correction purposes?
What should happen if overtime is added after payroll for that period is already processed?
Is overtime approval required before it affects salary?
Duplicate handling
If two supervisors enter overtime for the same worker and date, should the second entry be blocked, merged, or flagged?
Payroll impact
Does overtime always increase salary at a fixed rate, or are there different rules by role / day / shift / holiday?
Should overtime appear immediately in salary preview / payslip calculations or only after approval?
UX / workflow
Is this feature mainly used on mobile, desktop, or both?
Should the form support bulk overtime entry for multiple workers in one shift?
Should a site manager be able to edit or delete overtime after submission?
What confirmation should the user see after successful submission?
Audit / traceability
Do we need to store who entered the overtime and when?
Do we need an approval / audit trail for payroll disputes later?
8. Test scenarios (Given / When / Then)
Scenario 1 – Successful overtime submission

Given a valid active worker exists
When the site manager enters a valid date, valid overtime hours, and reason
Then the overtime record is saved and visible in the system.

Scenario 2 – Missing worker

Given the overtime form is open
When the user submits without selecting a worker
Then the form should show a validation error and not save the record.

Scenario 3 – Missing date

Given the user fills all other fields
When they submit without entering the overtime date
Then submission should be blocked with an error.

Scenario 4 – Invalid overtime hours

Given the user enters 0, -1, abc, or a value above the allowed limit
When they submit
Then the system should reject the input with a clear validation message.

Scenario 5 – Blank reason

Given the user enters worker, date, and hours
When the reason field is empty or only spaces
Then the system should not save the record.

Scenario 6 – Duplicate overtime for same worker and same date

Given an overtime record already exists for worker A on date X
When another overtime record is submitted for worker A on date X
Then the system should apply the agreed duplicate-handling rule and not silently create conflicting overtime.

Scenario 7 – Inactive employee

Given an employee is inactive or exited
When the site manager attempts to log overtime for that employee
Then the system should reject the submission unless correction rules explicitly allow it.

Scenario 8 – Network failure during submission

Given the user has filled the form on a mobile device
When the network drops during submission
Then the user should see a failure message and the form data should not be silently lost.

Scenario 9 – Future date not allowed

Given future overtime dates are not allowed by business rule
When the user selects a future date and submits
Then the system should reject the submission.

Scenario 10 – Overtime exceeds monthly cap

Given the worker already has 55 overtime hours this month
When the supervisor tries to add 7 more hours and the monthly cap is 60
Then the system should apply the business rule for overtime limit handling and prevent payroll from using invalid overtime totals.

9. Launch blockers vs V2
Launch blockers (must be resolved before release)

These directly affect payroll correctness or trust in the feature.

Form allows submission without worker/date/hours/reason
Invalid overtime values such as negative hours or zero are accepted
Duplicate overtime can be created for the same worker/date without warning
Overtime can be saved against a non-existent or inactive employee
Failed submission clears the form and gives no clear result
Mobile layout is unusable for the primary user
Overtime is saved successfully in UI but not persisted correctly in backend
Overtime data can flow into payroll incorrectly because limits/duplicate rules are undefined
V2 / later improvements

These are useful, but not blockers for first release if payroll correctness is already protected.

Bulk overtime entry for multiple workers
Supervisor approval / HR approval workflow
Attachments or proof for overtime request
Offline-first saving / queued sync for low-connectivity sites
Advanced search/filter/history for overtime records
Analytics/reporting for overtime trends
holiday/weekend overtime rule customization in UI
10. My QA position on this feature

I would treat this feature as payroll-critical, not just “another form.”
The main risk is not whether the page renders — it is whether overtime data becomes wrong, duplicated, or lost before salary is processed. Because of that, I would prioritize:

strong field validation
duplicate protection
clear business rules for caps and backdated entries
backend checks, not only frontend checks
regression coverage where overtime affects salary calculations later
