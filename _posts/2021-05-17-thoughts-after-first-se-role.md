---
title: "Observations On Software Engineering"
---

I recently left my first software development role after two and a
half years in the role. During that time I primarily focused on
building data pipelines for processing analytic events of a SaaS platform.
This post outlines some lessons I learned.

### The Two Phases of a Job Role

I see two phases to learning a new job role:

In Phase One you learn to perform the job as proficiently as your peers. This
phase completes when you can debug your codebase with ease, respond to alerts,
review PRs, comment intelligently on architectural changes, recognize design
anti-patterns, etc. In other words, you're a productive member of your team.
Phase One is easy and your teammates/managers should expect you to complete it
in a reasonable time frame. If your team doesn't hold this expectation, then
they don't care about technical excellence, and you should leave.

In Phase Two you recognize problems and build solutions[^phase_two].

[^phase_two]: Examples:
    * Problem: Slow development velocity and poor data quality. Solution: Implement
    data quality checks and data pipeline integration tests.
    * Problem: Inconsistent data use. Solution: Schema standardization and data
    discoverability tooling.

Phase Two requires creative thinking, which you don't cultivate in Phase One.
Technical excellence is table stakes for Phase Two; the crux is
understanding problems, reasoning to choose the correct
solution, and implementing that solution in a timely matter. This is much
harder, but it's also more fun; it's problem-solving on a far grander scale than
the routine work of Phase One.

I've just described the conceptual difference between a junior and senior
engineer. Unfortunately years of experience is taken to be the proxy for the
phase 1-2 boundary. It's a poor proxy, which is why you have senior
engineers who lack vision for their team.

### Say No

Saying "No" is the most valuable skill you can learn in software development.
If you lack extremely high confidence that you can complete a customer
request, then say "No". Saying "No" is primarily
beneficial to your _customer_, and only secondarily beneficial to _you_. "No"
clearly sets expectations and frees you to work on higher priority items which,
if you're prioritizing correctly, are _more_ beneficial to your customer[^no].

[^no]: Saying "No" to one-off user requests is essential to building self-serve
data platforms, and an excellent example of how "No" benefits your customers.
The team I left fielded endless one-off requests for data, which required
extending data pipelines. This work precluded building a self-serve platform,
which was, in my view, the only way to scale data engineering's impact. Other's
didn't share this vision, especially my insistence that we needed to drop the
vast majority of customer requests in order to free up dev bandwidth for
building a self-serve platform. This was part of my reasoning for leaving.

If you agree to a request and fail to deliver in a timely manner, you leave your
customer in a cycle of missed expectations: "We'll get to it in Q1"...
"We missed Q1, but it's on our roadmap for the last half of Q2"..."Other projects
came up, so now we're thinking Q1 of next year". Had the customer known this
would be their reality, they almost certainly would have made different
decisions, likely solving their problem in another way.

A backlog of "Won't Do" tickets is a historical artifact of your prioritization
decisions; it's not an indication that your team is failing. It's also a list
of unsolved problems that should guide future efforts.

If you're correctly prioritizing work, then "No" is easily justified and
understood by your customers, simply point to higher priority work.

### Ruthless Prioritization (Solve the Right Problems)

What you choose to work on is the most important decision you make. I'd venture
to say that it's far more important than how you execute on those problems.

### First, Build It The Right Way

### Organizational Structure Matters - Data Engineering is a Platform Team

### Tests Are Not For You


