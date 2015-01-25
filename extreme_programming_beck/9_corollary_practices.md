### Corollary Practices

These practices may be difficult/dangerous to implement before preliminary work with the primary practices detailed in chapter 7. Use your judgement.

* Real Customer Involvement
  * Make people whose lives/businesses are affected by your system par of the team
  * The point of this practice is to reduce wasted effort
  * Including custerms fosters trust and encourages continued improvement

* Incremental Deployment
  * When replacing a legacy system, gradually take over its workload beginning very early on in the project
  * Implementing scaffolding between the two systems is the price you pay for insurance against big changes going wrong

* Team Continuity
  * Keep effective teams together
  * "Ignoring the value of relationships and trust just to simplify the scheduling problem is false economy."

* Shrinking Teams
  * As a team grows in capability, keep its workload constant, but gradually reduce the number of team members
  * Strive to improve efficiency until some of the team members are idle — shrink the team and continue

* Root-Cause Analysis
  * When a defect is found or a mistake occurs, eliminate its cause such that the team will never make the same mistake again
  * This will involve writing an automated system-level test and an accompany unit test with the smallest possible scope that reproduces the defect
  * Use the technique of the ['Five Whys'](http://en.wikipedia.org/wiki/5_Whys)
  * The root cause is almost always a people problem

* Shared Code
  * Anyone on the team can improve any part of the system at any time

* Code and Tests
  * Maintain only the code and the tests are permanent artifacts
  * Rely on social mechanisms for important history of the project

* Single Code Base
  * Temporary branches sahould never live longer than a few hours
  * Multiple code streams are a huge source of waste

* Daily Deployment
  * Put new software into production every night
  * If you are out of sync with what is in production you risk making decisions without accurate feedback
  * For this practice to work, there are many prerequisites:
    * Defect rate is down to a handful per year
    * Build environment is smoothly automated
    * Deployment tools are automated
    * Ability to roll out incrementally and roll back in case of failure
    * Highly developed trust within the team and with customers

* Negotiated Scope Contract
  * Write contracts that fix time, costs, and quality, but call of an ongoing negotiation of precise scope of the system
  * Sign a series of short contracts instead of one long one
  * Negotiated scope contracts are a mechanism for aligning the interests of suppliers and customers to encourage communication and feedback

* Pay-Per-Use
  * Charge for every time the system is used — money is the greatest form of feedback
  * You can drive improvement by connecting money flow directly to software development
  * Pay-per-release isn't a good option: the supplier is motivated to keep shipping releases with new functionality, whilst the customer wants fewer releases with more functionality, in order to avoid the pain of upgrades
  * If you can't implement a pay-per-use model, then go for a subscription model. Then the team can focus on retention rates


If your team has a problem that isn't solved by one of the primary or corollary practices, then look back at the values and principles in order to come up with an appropriate solution.

