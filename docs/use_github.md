# Best Practice of Developing SMV Projects with Github

## Use Branches
For most of projects the nature of data development is always ad-hocish. Although we adopted the software development techniques, managing multiple branches and adding an integration step is an overkill. Also branching encourages code ownership and discourages full coordination when there is no team leader's enforcing or process enforcing.

We recommend to manage project around the "master" branch only.

It will force team members to coordinate and communicate through code in daily bases.

Although we recommended to use master branch only in the "remote" repo (the one on github.com), developers should feel free to use branches on their local repo.

## Use Github Issues
Also because of the ad-hoc nature of data application development, we may not need the full ticket system to manage tasks. Instead we can consider github issue system as our ticket system.

It's a good habit to create "issues" for the work one is working on and assign to himself, and close the issue when the task is done.

If a task can be finished in a day (doesn't matter whether anyone picks up it today or not), we call it type-1 issue, otherwise it's a type-2 issue. For type-2 issue, some "design document" need to be created as a "comment" item of the original issue. The "design comment" will help the developer to break down the task and also help others to learn more on that task.

A type-2 task can get even bigger so that some full design document should be part of the code itself (under /docs dir). In that case, create design document should be a task (an "issue"), either type-1 or type-2 by itself, and multiple tasks will be created for that worksrteam later.
