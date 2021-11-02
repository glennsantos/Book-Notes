Delivery Lead Time + Deployment Frequency = Quantity Metrics (Tempo)

Time to Restore Service + Change Fail Rate = Quality Metrics (Stability)

## Delivery Lead Time
> the time it takes to go from code committed to code successfully running in production
> Shorter is better
> measurement: from < 1 hour to more than six months


## Deployment Frequency 
a software deployment to production or to an app store.
> measurement: from on demand to fewer than once every six months

## Mean Time to Restore (MTTR))
> How quickly can service be restored?
> measurement: from < 1 hour to more than six months

## Change Failure Rate (CFR)
> percentage of changes for the primary application or service they work on either result in degraded service or subsequently require remediation 


----

High Performers
> On demand deployment frequency
> Less than one hour deliver lead time
> MTTR: < 1 hour
> CFR: 0 - 15%

there is no tradeoff between improving performance and achieving higher levels of stability and quality

## Delivery Performance Impact on Org Performance

> high-performing organizations were consistently twice as likely to exceed these goals as low performers.
> high performers were also twice as likely to exceed objectives in quantity of goods and services, operating efficiency, customer satisfaction, quality of products or services, and achieving organization or mission goals.

![](org-performance.jpeg)

delivery performance matters provides a strong argument against outsourcing the development of software that is strategic to your business

*In organizations with a learning culture, they are incredibly powerful.*

----

culture levels
1. interpretations: things people just "know"
2. values
3. artifacts: visible items like mission statements, procedures, etc 

Good information flow is critical to the safe and effective operation of high-tempo and high-consequence environments
- provides answers to the questions that the receiver needs answered
- timely
- presented in such a way that it can be effectively used by the receiver

best culture
> safe to deliver bad news
> high cooperation
> shared risk and responsibility
> cross functional collab encouraged
> failures are studied and means room for improvement

---

to implement continuous delivery
> It should be possible to provision our environments and build, test, and deploy our software in a fully automated fashion purely from information stored in version control
> keep branches short-lived 
> Automated unit and acceptance tests should be run against every commit

when developers are involved in creating and maintaining acceptance tests, there are two important effects
> code becomes more testable when developers write tests
> developers are responsible for the automated tests, they care more about them and will invest more effort into maintaining and fixing them

----

The goal of a loosely coupled architecture is to ensure that the available communication bandwidth isn’t overwhelmed by fine-grained decision-making at the implementation level, so we can instead use that bandwidth for discussing higher-level shared goals and how to achieve them.

As the number of developers increases, we found:

* Low performers deploy with decreasing frequency.
* Medium performers deploy at a constant frequency.
* High performers deploy at a significantly increasing frequency.

factors that predict high delivery performance
> goal-oriented generative culture
> modular architecture
> engineering practices that enable continuous delivery
> effective leadership

*When teams can decide which tools they use, it contributes to software delivery performance and, in turn, to organizational performance.*

Another finding in our research is that teams that build security into their work also do better at continuous delivery.

*Architects should collaborate closely with their users—the engineers who build and operate the systems through which the organization achieves its mission—to help them achieve better outcomes and provide them the tools and technologies that will enable these outcomes.*

----

high performers were spending 50% less time remediating security issues than low performers.

building security into software development not only improves delivery performance but also improves security quality. 
> Organizations with high delivery performance spend significantly less time remediating security issues.

infosec experts should contribute to the process of designing applications, attend and provide feedback on demonstrations of the software, and ensure that security features are tested as part of the automated test suite. 
> it’s much easier to make sure that the people building the software are doing the right thing
> information security teams simply don’t have the capacity to be doing security reviews when deployments are frequent.

----

Limit WIP
Visual Management
Feedback from Production
Lightweight Change Approvals

> visualize work via kanban

approval only for high-risk changes was not correlated with software delivery performance.
> Teams that reported no approval process or used peer review achieved higher software delivery performance.

external approvals were negatively correlated with lead time, deployment frequency, and restore time, and had no correlation with change fail rate.

**lightweight change approval process**
> pair programming
> intrateam code review

*This process can be used for all kinds of changes, including code, infrastructure, and database changes.*