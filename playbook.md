# Playbook

Here is a rough playbook of how one could imagine achieving the goal.

It should be no surprise that many projects have already taken some of the initial steps. In this playbook we build off of common practices to achieve the end goal.

 1. **Document SaaS deployment**
    1. Write up all the steps (actual commands) you know about deploying your SaaS.
    2. Start the document with an empty environment (e.g. a VM or cluster) and finish with the service running and a brief description of how to access it.
    3. Make sure to clearly describe requirements and expectations (e.g. you need to have OpenShift 4.6 cluster or base images need to contain python 3.9).
 2. **Test your Saas deployment documentation**
    1. Sit down with someone unfamiliar with deploying your SaaS, but is a capable developer and/or admin.
    2. Have them try out each step you documented.
    3. Write down any steps that are missing, or hand wavy.
    4. Rinse repeat: Walk through the deployment again until everything is documented.
 3. **Automate your SaaS deployment**
    1. Using a tool like Ansible, AWX, OpenShift, podman, scripting, and/or other
    2. Automate as much as possible of your deployment - ideally, any contributor should be able to perform the deployment with only a single command.
    3. On the first pass skip steps that are deemed “unautomatable”.
    4. Try to consolidate unautomatable steps into “Prerequisites” or another large group of tasks.
    5. Have your CI use as many parts of this automation to deploy as possible — this means you should test your deployment process during development of your service.
 4. **Use your automation to (re)deploy a staging environment**
    1. Run the automation to deploy another parallel copy of the SaaS as a second “staging” environment.
    2. Have this staging environment use the latest merged SaaS code.
    3. If you have already deployed a staging environment then redeploy it.
    4. If you’re unable to deploy a second copy of the SaaS, then fix the SaaS and/or the deployment automation, and repeat until successful.
    5. Deploy to staging as often as possible: ideally after merging any change.
 5. **Shard your production workload, so some hits staging environment**
    1. Choose a sharding strategy for your workload. This might be sharded along lines of certain users, or certain clients, certain parts of your data.
    2. Implement such sharding in your SaaS, and redirect some of your real production workload to your staging environment.
       1. Subdomain: Openshift Router Sharding
       2. By user: have a group of early adopters — alpha channel
       3. By data: mirror some traffic to production and staging environment without affecting your users
    3. This fundamentally changes your “staging environment” to have many properties of a “canary deployment” … it’s really a branch.
       1. You might choose blue/green or A/B deployments as an interim step to get here, but the end goal is to be continuously running part of your production workload in this “canary deployment”.
 6. **On demand redeployment and sharding configuration**
    1. Ensure that the canary deployment can be redeployed by any team member on demand.
    2. Ensure that any team member can change the config of the sharding, and choose which production workload is serviced by the canary deployment.
 7. **Include non-merged code in the canary deployment**
    1. Define requirements for cross-checking code so that it can run in production, despite not yet being merged. This might include:
       1. Cross check for not leaking data outside of the service.
       2. Cross check code for not leaking any credentials.
       3. Cross check code for not deleting data.
       4. Cross check code for not distributing user loaded content.
    2. Define a pre-review or cross-check process in your Git Forge.
    3. This is similar (or identical to) the typical “ok to test” workflow used with CI.
    4. Change the canary deployment automation so that specific pull requests which are not yet merged can be included in the staging environment.
    5. When combined with the “sharding” above, this results in certain unmerged work being run in production.
    6. Change your Git Forge workflow so that pull requests can be easily included in the canary deployment.
       1. Pro tip: having the interface in a pull request (bot) makes it as close to the change as possible - the contributor will appreciate this.

 8. **N canary deployments for N team members**
    1. By updating the deployment scripts and sharding logic, make it so that each team member can have their own canary deployment, redeployable, and handling some portion of production workload.
    2. Ensure team members can create a new canary deployment for a specific pull request, or set of changes, handling some specific production workload.
    3. Do cost projections for having N deployments, and look for non-scalable infrastructure expenses.
    4. Remove the stock shared canary deployment.

 9. **Non-team members can create canary deployment**
    1. Add tooling and remove obstacles so that folks outside the team, but within the company, can create a staging environment.
    2. Adjust the cross check requirements above for running unmerged code as necessary. It may be necessary to make them more strict here.

 10. **Outsiders can create staging environments that service production workload**  
    1. Remove remaining obstacles so that outside contributors can create staging environments for contributed work, and run production workload on them.  
    2. Could imagine this includes:
       1. Code review policy (since outside code is run, access to data)
       2. Cost projections (and adjustments based on what’s learned)
       3. Legal, Terms of Service

