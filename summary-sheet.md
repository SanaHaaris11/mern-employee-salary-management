Summary Sheet
1) What does this HRMS exist to deliver, and to whom?

This HRMS exists to deliver correct employee salary and payroll information by keeping employee records, salary details, and related workforce data accurate and usable. The person who depends on it most is the employee / worker, because when payroll data is wrong, their monthly wages are directly affected.

2) What is the single most dangerous bug pattern you found across your testing?

The most dangerous bug pattern is data changes not propagating correctly to downstream payroll outputs. For example, if an employee’s salary is updated but the payroll or salary view still uses the old value, the system appears to work while silently producing incorrect salary information that can affect workers and create manual correction work for payroll staff.

3) Which test in your suite are you most proud of, and why?

The test I am most proud of is the salary update → downstream salary/payslip verification test, because it protects the most business-critical dependency in the system. It would help prevent a production failure where an employee’s updated salary is saved in one place but not reflected in the next payroll calculation, which directly protects the worker and the payroll operator.

4) What did you choose NOT to automate, and why was that the right call?

I chose not to automate lower-risk visual checks, cosmetic UI issues, and broad exploratory scenarios that change frequently during development. That was the right call because the assignment is about protecting the flows where failure harms workers or payroll operations, and I wanted automation time to go toward salary, employee lifecycle, and authentication risks instead of unstable low-impact checks.

5) What’s one thing in your submission that you’re not fully confident about, and why did you include it anyway?

I am not fully confident about every downstream payroll dependency in this specific HRMS because the codebase is new to me and some business rules are not explicitly documented. I still included my regression and risk assumptions because part of quality work is making the dependency risks visible early, even when the system’s behavior is not fully documented yet.

6) What changed between your first approach and your final submission?

My first approach was more generic and feature-based: I was thinking in terms of employee CRUD, login, and form validation. My final submission changed after I reframed the system around payroll risk, which made me prioritize salary propagation, employee onboarding into payroll, employee exit handling, and regression protection over general UI coverage.

7) What do you not know yet about quality engineering or construction payroll that you would need to learn before doing this job well?

I would need to learn three things more deeply:

the actual payroll rules and edge cases used in construction environments, especially overtime, deductions, and attendance-linked pay rules;
the full dependency map between employee records, salary data, attendance, and payroll outputs in the real product;
the team’s deployment and release workflow so that the quality process I propose fits how developers actually ship changes.
8) If you had one more day, what would you test that you didn’t get to?

If I had one more day, I would test payroll-related negative and edge scenarios around salary history, duplicate updates, invalid salary values, and employee deactivation affecting downstream salary records. These are important because they protect the payroll operator from silent data corruption and protect employees from incorrect salary output caused by edge-case changes.
