# User Stories

For simplicity, the user stories below assume:
 * Refer to “the SaaS” as the singleton
 * Sharding by user. That is: various users' workloads can have different behavior. Other options would be sharding based on data itself, or service levels, or other.
 * Pull requests are used. That is: pull requests are the means for contributing to a component of the SaaS. Other options may be available.
 * Some users become contributors. This is true of most open source projects. The contributors are also users of the SaaS. 

## Simple Contribution headed for Production

As a user of the SaaS I should be able to:

 * Understand that I can change the SaaS’s behavior by contributing to it
   * Perhaps by a “Fork me” link on in a web footer, in the documentation, or similar.
 * I discover the documentation for contributing to the SaaS
 * I can understand everything that’s running in the SaaS, the makeup of the service. I can see exactly what is running.
 * I can identify the component that I need to change for my contribution
   * Whether through documentation, map, logging, or intuitive naming.
   * Perhaps there are some components that are not contributable.
 * Next I locate the code repository for the component
 * *Optionally*: I understand how to build the component locally, and run it locally
   * This is useful for fast iteration, since the iteration loop below includes at least two people.
 * I then submit a pull request for my changed code to a component
 * Watch that pull request be built, tested automatically
 * A maintainer comes and cross-checks my pull request to  (at a minimum) check that it does not violate data, privacy, contractual or legal limitations. 
   * *Question*: Is this still a good step to do for internal team members?
   * Since pull requests may be a fundamental way of running and iterating on a contribution, we need to ensure that this review stage still enables people to experiment.
 * The change is deployed in the SaaS so that the changed behavior affects (only) my production workload.
   * *Discussed*: Lets aim to implement this via running the entire SaaS software stack again for the changed behavior. This incentivises two things:
     * Allows “bad ideas” to be iterated on, and then into “good ideas” before merging them.
     * Incentivises actively iterating on changes toward getting them merged, because otherwise the user gets “left behind”. The user doesn’t benefit from other recent work that landed in the SaaS.
 * *Optionally*: I iterate by updating the pull request and follow the steps above
   * Further testing, further cross-checks, further deployment … loop.
 * I see that data is gathered about how much workload is running using my changed behavior, and what kinds of actions.
 * I propose that the pull request is merged and becomes part of production behavior for everyone.
 * Further review is done by a maintainer, who looks at my code, the data gathered about how it has already run.
 * I may be asked to make further changes to my pull request, or “rebase” it to incorporate changes that merged since I branched.
   * *Discussed*: We need to incentivise the existing behavior with Open Source … where maintaining/running your own fork becomes expensive because it misses out on changes landed by others. 
 * Idea to match this incentive: Until my change is merged, I don’t see experience changes made by others, unless I put in effort to “rebase”.
 * My change becomes part of production behavior for everyone.

## Multiple pull requests working together

 * During the above workflow, I notice that I must also change another component.
 * I am able to open a second pull request, as above, have it be cross checked.
 * The second pull request is deployed in the SaaS so that the combined changed behavior of all my pull requests affects (only) my production workload.
 * The remainder of the process follows as described above.

## Multiple contributors working together

 * During either the iteration or review stages of the above workflow, I need other contributors to try out my work.
 * Other users are able to indicate that their production workload is affected by my (group of) pull requests.
 * The remainder of the process follows as described above.

