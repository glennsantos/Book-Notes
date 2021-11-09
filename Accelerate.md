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

----

software delivery performance predicts Lean product management practices

an experimental approach to product development is highly correlated with the technical practices that contribute to continuous delivery.

the ability of teams to try out new ideas...without requiring the approval of people outside the team, is an important factor in predicting organizational performance

lean product management practices positively impact software delivery performance, stimulate a generative culture, and decrease burnout.

----

improving key technical capabilities reduces deployment pain
> comprehensive test and deployment automation
> continuous integration
> shift left on security
> manage test data
> loosely coupled architectures
> can work independently
> use version control of everything required to reproduce production environments decrease their deployment pain

* Build systems that are designed to be deployed easily into multiple environments
* can detect and tolerate failures in their environments
* can have various components of the system updated independently
* Ensure that the state of production systems can be reproduced in an automated fashion from information in version control
* Build intelligence into the application and the platform so that the deployment process can be as simple as possible

Symptoms of burnout 
- feeling exhausted
- cynical
- ineffective
- little or no sense of accomplishment in your work
- feelings about your work negatively affecting other aspects of your life

Managers who want to avert employee burnout should concentrate their attention and efforts on
> Fostering a respectful, supportive work environment that emphasizes learning from failures rather than blaming
> Communicating a strong sense of purpose
> Investing in employee development
> Unblocking devs
> Giving employees time, space, and resources to experiment and learn
> employees must be given the authority to make decisions that affect their work and their jobs, particularly in areas where they are responsible for the outcomes.

six organizational risk factors that predict burnout 

1. Work overload: job demands exceed human limits.
2. Lack of control: inability to influence decisions that affect your job.
3. Insufficient rewards: insufficient financial, institutional, or social rewards.
4. Breakdown of community: unsupportive workplace environment.
5. Absence of fairness: lack of fairness in decision-making processes.
6. Value conflicts: mismatch in organizational values and the individual’s values.

fix the environment first, then the person


fostering a supportive and respectful work environment
> creating a blame-free environment
> learn from failures
> communicating a shared sense of purpose
> ask their teams how painful their deployments are and fix the things that hurt the most
> limiting work in process
> eliminating roadblocks for the team
> invest in developing the skills and capabilities of their teams
> providing people with the necessary support and resources (including time) to acquire new skills
> creating a work environment that supports experimentation, failure, and learning,
> creating space for employees to do new, creative, value-add work during the work week


human error is never the root cause of failure in systems

When organizational values and individual values aren’t aligned, you are more likely to see burnout 