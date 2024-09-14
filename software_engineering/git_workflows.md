# Git Workflows

## Centralised Workflow
<img src="https://github.com/user-attachments/assets/b560d0ca-18bb-42de-9118-7f2a8a48d95f" height="200">

The Centralised workflow uses a single central repository where all team members directly pull and push changes into the main repository.

Pros:
- Easy and simple integration with everyone working on the same branch
- Less configuration and management of branches

Cons:
- Risk of merge conflicts due to concurrent changes with no isolation
- Potential bugs due to direct commits without integration tests 

## Feature Branch Workflow
<img src="https://github.com/user-attachments/assets/5f7085cc-d3c6-43cf-bf76-1274fa1f2ca7" height="200">

Feature Branch workflow involves creating a new feature branch for each feature or bug fix, branching off from the main branch.
Features can be developed independently and merged into the main branch after code review and testing.
CI/CD can be triggered upon a pull requests from feature branch to main branch.

Pros:
- Isolation of developments
- Enabled code reviews and tests

Cons:
- Overheads with merge and branch management
- Integration delay due to merging and testing

## Gitflow Workflow
<img src="https://github.com/user-attachments/assets/65cedd21-8f01-4845-b298-b89b921e0fe5" height="200">

Gitflow workflow uses two long-lived branches; main & develop, where main refers to production-ready code and develop refers to integration of features.
Additionally, Gitflow makes use of the following branches:
- Feature branch: diverges from the develop branch. Upon completion, it merged into the develop branch.
- Release branch: diverges from the develop branch and is dedicated for release purpose only. No new feature is added and only release-related tasks are done. When it's ready to ship, it's merged into the main branch with a version tag. And it is also merged into the develop branch.
- Hot fix branch: diverges from the main branch and is for a quick bug fix. Once completed, it gets merged into both main (with version tag) and develop (or current release) branches.

Pros:
- Clear separation between development phases
- Parallel development of release and features
- Management of releases with a dedicated branch and testing before deployment

Cons:
- Complexity and overheads due to multiple branch management
- Slow development and delivery due to rigid structure

## Forking Workflow
Forking workflow involves developers forking the official server-side repository to have their own server-side repository.
Afterwards, their own server-side repository is cloned to the local environment for development purpose.
Similar to Feature Branch workflow or Gitflow workflow, developers work on feature branches and push changes to their forked repository.
To contribute their changes to the main project, developers need to raise a pull request to the official repository.
This workflow is often seen in open-source projects.
