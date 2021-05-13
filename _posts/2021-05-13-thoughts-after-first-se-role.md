---
title: "Observations On Software Engineering: Two Years In"
---

I recently left my first software development job after two and a
half years in the role. During that time I primarily focused on
building data systems for processing analytic events of a SaaS platform.
This post shares a few lessons I learned.

### Say No

Saying "No" is the most valuable skill you can learn in software development.
Unless you're nearly certain of your ability to complete a customer request,
say "No". "No" primarily
benefits your _customer_, and secondarily benefits _you_. "No"
clearly sets expectations and frees you to work on higher priority items which,
if you're prioritizing correctly, are _more_ beneficial to your customer[^no].

[^no]: Saying "No" to one-off user requests is essential to building self-serve
    data platforms and is an excellent example of how "No" benefits your customers.
    Endless one-off requests for data require
    extending data pipelines. This work precludes building a self-serve platform,
    which, in my view, is the only way to scale data engineering's impact and
    better serve customers.

If you agree to a request and fail to deliver in a timely manner, you leave your
customer in a cycle of missed expectations: "We'll get to it in Q1"..."We
missed Q1, but it's on our roadmap for the last half of Q2"..."Other projects
came up, so now we're thinking Q4". A customer able to forsee this outcome would
certainly make different decisions, likely solving their problem in another way.

A backlog of "Won't Do" tickets is a historical artifact of prioritization
decisions, not an indication your team fails to deliver. It's also a list
of unsolved problems that should guide future efforts.
If you're correctly prioritizing work, then you can easily justify "No" to your
customers by pointing to higher priority work and your supporting evidence[^evidence].

[^evidence]: OKRs, RFDs, etc.

### Prioritize Ruthlessly (Solve the Right Problems)

Choosing what to work on is the most important decision you make. It's more
important than how you execute on that
work[^decisions]. Choosing what to work on is important because the set of all
potential problems
is unbounded, while the amount of resources available to devote to those
problems is quite finite. It's challenging to build a
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
  Developers feel directionless, unsure if
  leadership will abandon current projects in favor of new initiatives.
* Frequent context switching means developers don't become experts in the
  technologies they use and software they build. The team doesn't compound
  its collective knowledge.
* External stakeholders lose trust in the team as it abandons work.
* The comprehensive technical vision for the team's software erodes and
  eventually disappears as the team becomes reactive, rather than proactive.

Problems in software development don't exist in isolation. Solving _C_ depends
on solving _A_ and _B_. Ruthless prioritization ensures _C_ is indeed the most
important problem to solve, and that the organization correctly allocates
resources to
problems _A_ and _B_. This is form of compounding on
resource investment. Teams should maximize their resource investments,
which requires prioritizing and finishing the correct work.

### First, Build It The Right Way

If you can't build it the right way, then don't build it at all. It doesn't
matter how urgent the request (the one exception being if the request addresses
an existential threat, i.e. security or compliance). Once you
write the software, you're abandoning your ability to meaningfully change it.
As soon as it "works",
other processes (software or otherwise) will depend on it, and the difficulty of
rewriting grows exponentially. Initial design
flaws are baked into the foundation of what you're building, and you should
act as though it's impossible to eliminate those flaws. When you do refactor, it
will be because you're responding to either a severe issue or many months of
headaches caused by the initial poor design. It is
_always_ faster and cheaper to build it correctly the
first time around, no matter how many deadlines you need to miss.

### Get to Phase Two

I see two phases to learning a new job role:

In Phase One you learn to perform the job as proficiently as your peers. This
phase completes when you can debug your codebase with ease, respond to alerts,
review PRs, comment intelligently on architectural changes, recognize design
anti-patterns, etc. In other words, you're a productive member of your team.
Phase One is easy and your teammates and managers should expect you to complete it
in a reasonable time frame. If your team doesn't hold this expectation, then
they don't care about technical excellence, and you should leave.

In Phase Two you recognize problems and build solutions[^phase_two].

[^phase_two]: Examples:
    * **Problem**: Slow development velocity and poor data quality. **Solution**: Implement
    data quality checks and data pipeline integration tests.
    * **Problem**: Inconsistent data use. **Solution**: Schema standardization and data
    discoverability tooling.

Phase Two requires creative thinking, which you don't cultivate in Phase One.
Technical excellence is table stakes for Phase Two; the crux lies in
understanding problems, choosing the correct
solution, and implementing that solution in a timely matter. This is much
harder, but also more fun as it's problem-solving on a grander scale than
the routine work of Phase One.

I've just described the conceptual difference between a junior and senior
engineer. Unfortunately, years of experience is taken to be the proxy for the
Phase One-Two boundary. It's a poor proxy, which is why you have senior
engineers who lack vision for their team.

### Structure the Organization Correctly (Make Data Engineering a Platform Team)

Data engineering is an immature sub-discipline of software engineering. As
such, the industry lacks a core set of best practices. That's especially true
in how organizations structure their data engineering squads. Should there be
a core data engineering team, pods of data engineers distributed amongst product
teams, or a hybrid approach? And, if there's a central team, where does it live?
Under Product, alongside Data Science, or something else? Different
solutions
work for different companies, but I'm sure of one thing: _the
core Data Engineering team should be treated as a platform team, similar to
the DevOps/PlatOps/Infrastructure groups_.

This distinction reflects what
I see as the mission of a core data engineering squad: to build and maintain a
set of extensible tools and data products that enable a user base to
utilize data for a diverse set of needs. It also reflects the importance of data in
company operations -- data should be a primary concern for application
architecture, which requires that the data engineering squad voice strong opinions
on platform matters.

By treating Data Engineering as a platform team, you make clear the expectation
that it builds tooling and platform software, not ad-hoc data products.
This critical distinction enables Data Engineering to scale with the
organization. Scaling requires data products built by the users that understand
those domains best: product engineers, data scientists, ML engineers, or analysts.
Data engineers, on the other hand, build and maintain the abstractions that
enable self-service
components: schema standards, data discoverability tooling, data quality
frameworks, data lakes, query engines, streaming frameworks, job schedulers,
workflow engines, etc.

Failing to treat Data Engineering as a platform team means those engineers will
drown in an endless stream of data requests[^etl], and, more importantly, will
lack influence to create lasting data solutions at the application level.

[^etl]: [Engineers Shouldn't Write ETL](https://multithreaded.stitchfix.com/blog/2016/03/16/engineers-shouldnt-write-etl/)

### Summary (Did I Learn Nothing Technical)?

I didn't intend this post to be entirely non-technical. I learned countless
technical lessons in the last two and half years[^technical_lessons]. That the
final product is entirely non-technical leaves me with a closing thought: the
technical challenges of building software are, in many ways, implementation
details to a successful company or team. This isn't to say that
there aren't extremely difficult technical challenges. If you're a lucky
(and talented) developer, you work on transformational software. But
not every company and team is on the bleeding edge. For most developers building a
SaaS product, the technical problems have known solutions. The challenge lies in
developing effective processes for smoothly implementing those known solutions.

I don't find this organizational process stuff more interesting than developing,
quite the contrary. But I do think it's incredibly important.
Every day we're each capable of making horrible non-technical decisions
that lead to miserable developers and poor outcomes. Hopefully these
lessons lead to better decision making.

[^technical_lessons]: A smattering:

    * Tests are not for you, they're a friendly note to future developers:
    "Please don't break this API".
    * If it runs in production, it needs integration tests (and unit tests).
      Integration tests speed up development by ensuring safety.
    * Don't roll your own job scheduling solution. Use a distributed cron or
    a battle-tested DAG scheduler.
    * Don't use timestamps (of any precision) as unique
    identifiers in concurrent systems. They will collide.
    * Build trust in your systems by making them observable.
