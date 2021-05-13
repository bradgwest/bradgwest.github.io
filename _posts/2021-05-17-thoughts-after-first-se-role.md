---
title: "Observations On Software Engineering"
---

I recently left my first software development job after two and a
half years in the role. During that time I primarily focused on
building data pipelines for processing analytic events of a SaaS platform.
This post outlines some lessons I learned.

### The Two Phases of a Job Role

I see two phases to learning a new role:

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
Unless you're nearly certain of your ability to complete a customer request,
say "No". Saying "No" is primarily
beneficial to your _customer_, and secondarily beneficial to _you_. "No"
clearly sets expectations and frees you to work on higher priority items which,
if you're prioritizing correctly, are _more_ beneficial to your customer[^no].

[^no]: Saying "No" to one-off user requests is essential to building self-serve
data platforms, and an excellent example of how "No" benefits your customers.
The team I left fielded endless one-off requests for data, which required
extending data pipelines. This work precluded building a self-serve platform,
which, in my view, was the only way to scale data engineering's impact, and
ultimately better serve our customers.

If you agree to a request and fail to deliver in a timely manner, you leave your
customer in a cycle of missed expectations: "We'll get to it in Q1"..."We
missed Q1, but it's on our roadmap for the last half of Q2"..."Other projects
came up, so now we're thinking Q1 of next year". Had the customer known this
would be their reality, they almost certainly would have made different
decisions, likely solving their problem in another way.

A backlog of "Won't Do" tickets is a historical artifact of your prioritization
decisions; it's not an indication that your team is failing. It's also a list
of unsolved problems that should guide future efforts.

If you're correctly prioritizing work, then "No" is easily justified and
understood by your customers, simply point to higher priority work.

### Ruthless Prioritization (Solve the Right Problems)

Choosing what to work on is the most important decision you can make. I'd venture
to say that it's far more important than how you actually execute on those
problems[^decisions]. It's important because the set of all potential problems
is unbounded, while the amount of resources available to devote to those
problems is depressingly finite. It's difficult to build a
prioritization function which optimizes for the correct variable (some form
of customer value), and which is both flexible and stable in the face of new
information and user requests. Success in this realm comes from a host of
factors: commitment to organizational goals, patient leadership, deep
understanding of customer needs, and an excellent technical vision for the
software platform.

[^decisions]: It's important to execute well, but in software there are almost
always multiple valid solutions to the same problem, which is why choosing the
correct problem is more important (and more difficult) than designing the correct
solution.

Without ruthless prioritization, a few things happen:
* Team goals change too easily in response to new information and customer requests.
  Developers abandon unfinished work, and soon feel directionless, unsure if
  their current projects will be abandoned in favor of new initiatives.
* With frequent context switching, developers can't become experts in the
  technologies the use and software they build. The team doesn't compound
  its collective knowledge.
* External trust in the team fades as it abandons work.
* The comprehensive technical vision for the team's software becomes murky, and
  eventually disappears -- the team becomes reactive, rather than proactive.

Problems in software development don't exist in isolation. Solving C depends
on solving A _and_ B. Ruthless prioritization ensures that C is indeed the most
important problem to solve, and that resources are allocated to
problems A and B in support of solving C. This is form of compounding
resource investment, and you've likely moved up the abstraction hierarchy by
solving A and B. Teams should maximize their resource investments,
which requires prioritizing and finishing the correct work.

### First, Build It The Right Way

If you can't build it the right way, then don't build it at all. It doesn't
matter how urgent the request is, unless it's an existential threat (i.e.
security or compliance). The reasoning is simple: once you
write the software, you will not meaningfully change it. As soon as it "works",
other processes/software will begin to depend on it, and the difficulty of
rewriting it grows exponentially. The initial design
flaws are baked into the very foundation of what you're building, and you should
act as though it's impossible to eliminate those flaws. When you do refactor, it
will be because you're responding to either a severe issue, or many months of
headaches caused by the initial poor design. It is
_always_ faster and cheaper to build it correctly the
first time around, no matter how many deadlines you need to miss.

### Organizational Structure Matters - Data Engineering is a Platform Team

Data engineering is an immature sub-discipline of software engineering. As
such, the industry lacks a core set of best practices. That's especially true
in how organizations structure their data engineering squads. Should there be
a core data engineering team, pods of data engineers distributed amongst product
teams, or a hybrid approach? And, if there's a central team, where does it live?
Under the Product sub-org? Alongside Data Science? Different solutions
work different companies, but I'm sure of one thing: the
core Data Engineering team should be treated as a platform team, similar to
the DevOps/PlatOps/Infrastructure groups. This distinction reflects what
I see as the mission of a core data engineering squad: to build and maintain a
set of extensible tools and data products that enable a diverse user base to
store, analyze, and report on data. It also reflects the importance of data in
company operations -- data should be primary concern for application
architecture, and this requires that the data engineering squad is at the core
of platform decisions, not a outsider trying to voice concerns from the
sidelines.

By treating Data Engineering as a platform team, you make clear the expectation
that it builds tooling and platform software as opposed to ad-hoc data products.
This distinction is critical for Data Engineering to scale with the
organization. Data products need to be built by the users that understand those
domains best, product engineers, data scientists, ML engineers, or analysts.
Data engineers build and maintain the abstractions that enable self-service,
components like schema standards, data discoverability tooling, data quality
frameworks, data lakes, query engines, streaming frameworks, job schedulers,
workflow engines.

Failing to treat Data Engineering as a platform team means those engineers will
drown in an endless stream of ad-hoc data requests[^etl], and, more importantly, will
lack influence to create lasting data solutions at the application level.

[^etl]: [Engineers Shouldn't Write ETL](https://multithreaded.stitchfix.com/blog/2016/03/16/engineers-shouldnt-write-etl/)

### Summary - Did I Learn Anything Technical?

I didn't intend this post to be entirely non-technical. I learned countless
technical lessons in the last two and half years[^technical_lessons]. That the
final product is entirely non-technical leaves me with a closing thought: the
technical components of building software is, in many ways, an implementation
detail in operation of a successful team. This isn't to say that
there aren't extremely difficult technical challenges. If you're a lucky
(and talented) developer, you are working on truly bleeding edge software. But
not every company is on the bleeding edge, and not every team at a bleeding edge
company is itself on that edge. For most developers building a SaaS product.
the technical
challenges are solved problem with known solutions. The challenge lies in
developing effective processes for smoothly implementing those known solutions.

I don't find this organizational process stuff more interesting than developing,
quite the contrary. But I do think it's incredibly important.
Every day we're each capable of making horrible non-technical decisions
that lead to miserable developers and poor outcomes. I'd like to think that
these lessons are a step in the direction of better decision making.

[^technical_lessons]: A smattering:

    * Tests are not for you, they're a friendly note to future developers:
    "don't break this API".
    * If it runs in production, it better have an integration test (not to
    mention unit tests).
    * Don't roll your own job scheduling solution, use a distributed cron, or
    a battle-tested DAG scheduler.
    * Do not use timestamps, even those to microsecond precision, as unique
    identifiers in distributed systems. You will have collisions.
    * Build trust in your systems by making them observable.
