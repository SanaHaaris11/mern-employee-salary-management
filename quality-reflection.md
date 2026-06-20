Quality Reflection
1) Honest question: when I write code in my own projects, do I write tests?

My honest answer is: not consistently yet.

Most of my academic and personal projects were built with the goal of getting the feature working first — for example login, booking flow, CRUD pages, or database integration. In those projects, I usually tested by running the application manually, checking the UI, trying valid and invalid inputs, and fixing what broke. I did not have a disciplined habit of writing automated tests for every project.

What changed for me while doing this assignment is that I felt the difference between “I tested this once” and “this system is protected if someone changes it next week.” In a payroll-related HRMS, that difference matters because a bug is not just a broken page — it can become a worker’s missing wage. So I would not claim that I already have a strong testing habit, but I can say that this assignment made the gap very visible to me. If I join a QA-focused role, one of the first habits I want to build is: whenever I touch a critical flow, I should leave behind at least one automated check that protects it the next time.

2) Describe a time you shipped something that was not fully tested, or found a bug in production that should have been caught earlier

In one of my project builds, I completed the main functionality and focused heavily on making the UI and flow work end-to-end. I tested the happy path multiple times, but I did not test enough edge cases after changing connected modules. Later, I found that one small change in one part of the system affected another flow that I had assumed would continue working. The issue was not hard to fix, but the real lesson for me was that I was testing pages, not dependencies.

That experience connects directly to this assignment. In an HRMS, salary, employee status, attendance, and payroll outputs are connected. A change that looks small in one module can silently break something downstream. Because of that, I would approach quality here less as “did this page work?” and more as “what other payroll outcome depends on this data, and how do I prove that still works after a change?”

3) Take a stand: what does this team need MOST right now?

If I had to pick one thing only, I would choose:

A small, fast regression suite for payroll-critical flows running on every pull request.

I am choosing that over a broader “QA process” because the team’s biggest current problem is not lack of opinions about quality — they already have opinions. The PM wants reliability without slowing delivery, the senior developer trusts memory and experience, and the new developer is afraid to touch payroll because there is no safety net. A fast regression suite is the one thing that directly addresses all three.

It protects the payroll flows that hurt workers if they break, gives the new developer confidence to make changes without guessing, and is easier to get buy-in for than a heavy process change. I do not think the team needs a large testing program first. I think they need a reliable way to stop the same classes of production bugs from escaping again.

4) How would I get the senior developer to care about quality without being condescending?

In my first week, I would not start by telling him that the team “needs more testing” or that his current way is wrong. He has three years of codebase knowledge, and dismissing that would be a mistake. Instead, I would start from the bugs the team already felt this month and from the pain the new developer is already experiencing.

I would say something like:

“You already know where the payroll bugs usually happen. I don’t want to replace that knowledge — I want to capture the most dangerous parts of it in a way that protects the team when you are busy, when a new developer makes a change, or when the company scales from 2 sites to 8. If we can turn two or three of the recurring break points into fast checks, your workflow changes very little, but the team stops depending entirely on your memory.”

That matters because I think the best way to get his buy-in is not to sell testing as process, but to show respect for what he already knows and turn that knowledge into a reusable safety net. I would also keep the first tests very targeted and fast, so the result feels like leverage rather than overhead.

5) Connect your test strategy to something personal — a system or habit you built for yourself

A simple example from my own life is how I organize work when I have multiple deadlines at once. If I rely only on memory, I miss things. So I usually break work into smaller checkpoints, keep the most important items visible first, and review the high-risk tasks again before
