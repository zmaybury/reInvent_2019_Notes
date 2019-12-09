[Talk List](./README.md)
# DOP342-R - [REPEAT] Amazon's approach to building resilient services

One of the biggest challenges of building services and systems is predicting the future. Changing load, business requirements, and customer behavior can all change in unexpected ways. In this talk, we look at how AWS builds, monitors, and operates services that handle the unexpected. Learn how to make your own services handle a changing world, from basic design principles to patterns you can apply today.

## Ownership: closing the DevOps loop
The Loop
- Build
- Fail*
- Analyze causes of failure
- Change practices

Risks
- Outright project failure
- Moving too slowly (unable to meet business goals & objectives)
- Outages

How AWS does this
- Teams run what they build
- Principal engineers are builders (avoid non-practitioner architects)
- Everybody has operational responsibility
- Correction of Error (COE) revies connect broad sets of builders and operators
- Centralized Ops teams can work, but need to close loop for people and for technology

## Operational Safety
> How and why should the operators have compensated for design errors they did not know about?
### Kind Learning Environments
- Things we learn match environment well
- More experience means better predictions and better judgement
### Wicked Learning Environments
- Things we learn don't match the environment
- Our expeirence leads us to do the wrong thing

What does AWS do?
- Build your systems to be kind! (and usable)
- CoE process doesn't settle for operator error
    - Doesn't teach anything, adds fear and stakes for failure higher
- Reviewing tooling is critical work for the most senior engineers
- Operators are easy to blame
    - They made the mistake (that is, managers and architects have no responsibility)
    - We're fired them (that is, the issue is fixed with little effort)
    - They no longer work here (that is, no need to be worried)
    - Leadership must reject this thinking

## Service Stability
What it means to be stable at scale
- The moment the system starts experincing instability, it will not stop without correction
- Example loop
    - System becomes overloaded
    - Load increases latency (from contention)
    - Timeouts cause failures
    - Retries add load (for transient errors, this is a best practice)
    - guaranteed "stable" deadlock
- What does AWS do to combat this?
    - Limit queue size
    - Limit retries
    - Exponentially backoff on retry
        - Need to be careful here though, as time to recovery get long
    - Limit retries if possible
    - End-to-End back pressure

## Jitter
Randomness added to reduce spiky traffic
- choose a random-ish delay for retries to better utilize the system
- What is the best way to add Jitter?
    > randint(0, base * 2 ** attempt)
    - base: in milliseconds
    - attempt: times we've already backed off
- At AWS?
    - Always jitter when using backoff
    - Always jitter periodic work (timers, cron, etc.)
        - large percent of requests may happen in first second of the minute (high average, but poor utilization), consider spreading that work out, especially if an end user is not waiting for the result
    - Consider adding jitter to all work

## Takeaway
Success requires both culture and technology