# GitOps
GitOps is a set of best practices where the entire code delivery process is controlled via Git, 
including infrastructure and application definition as code and automation to complete updates and rollbacks.

The Key GitOps Principles:
- The entire system (infrastructure and applications) is described declaratively (ex. Kubernetes).
- The canonical desired system state is versioned in Git.
- Changes approved are automated and applied to the system.
- Software agents ensure correctness and alert on divergence.

## How does GitOps work?
In the case of Kubernetes, GitOps deployments happen in the following manner:
1. A GitOps agent is deployed on the cluster.
2. The GitOps agent is monitoring one or more Git repositories that define applications and contain Kubernetes manifests (or Helm charts or Kustomize files).
3. Once a Git commit happens the GitOps agent is instructing the cluster to reach the same state as what is described in Git.
4. Developers, operators. and other stakeholders perform all changes via Git operations and never directly touch the cluster (or perform manual kubectl commands).

### Traditional deployment without GitOps
<img src="https://github.com/TravisH0301/learning/assets/46085656/9ffa9437-7823-42e4-b445-1ce67fa060a9" width="800px"> <br>
*Source: Codefresh*
1. A developer commits source code for the application.
2. A CI system builds the application and may also perform additional actions such as unit tests, security scans, static checks, etc.
3. The container image is stored in a Container registry.
4. The CI platform (or other external system) with direct access to the Kubernetes cluster creates a deployment using a variation of the “kubectl apply” command.
The application is deployed on the cluster.

### Streamlined deployment with GitOps
<img src="https://github.com/TravisH0301/learning/assets/46085656/093f1c50-67e9-43f1-84b3-682343fe4b76" width="800px"> <br>
*Source: Codefresh*
1. A developer commits source code for the application.
2. The CI system creates a container image that is pushed to a registry.
3. Nobody has direct access to the Kubernetes cluster. There is a second Git repository that has all manifests that define the application.
4. Another human or the CI system changes the manifests in this second Git repository.
4. A GitOps controller that is running inside the cluster is monitoring the Git repository and as soon as a change is made, it changes the cluster state to match what is described in Git.

## Benefits of GitOps
- Faster deployments
- Safer deployments
- Easier rollbacks
- Straightforward auditing
- Better traceability
- Eliminating configuration drift

On the other hand, GitOps is only limited to Declarative systems. 
And it can lead to higher workloads due to increased Git commit requirements.
Additionally, GitOps may have to be switched off during debugging.
