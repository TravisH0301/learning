# Identity and Access Management (IAM) in AWS
IAM enables cloud users to manage access to AWS services and resources securely.

## AWS Account Root User
A root user refers to an owner of the AWS account. Given the root user has access to all services and resources on AWS, it is recommended to create first IAM user and assign permissions to create other users. Then, additional IAM users can be created with permissions to perform tasks on AWS. The root user should only be used when performing limited number of tasks that only a root user can do.

## IAM Users
An IAM user is an identity that representa a person or application that interacts with AWS services and resources. It consists of a name and credentials.

By default, it has no permissions associated with it. And it is recommended to create IAM users for all individual users of a company.

## IAM Policies
An IAM policy is a JSON document lists permission levels to AWS services and resources. 

Best practice is to follow Principle of least privilege when granting permissions.

## IAM Groups
An IAM group is a collection of IAM users. When an IAM policy is assigned to a group, the policy is inherited to all users of the group.

## IAM Roles
An IAM role is an identity that can provide temporary access to permissions. Before an IAM user, application, or service can assume an IAM role (=temporarily take on the permissions and access rights that are associated with the role), they must be granted permissions to switch to the role. When someone assumes an IAM role, they abandon all previous permissions that they had under a previous role and assume the permissions of the new role. Hence, only a single role can be put in action at the same time.

As a best practice, IAM roles are ideal for situations in which access to services or resources needs to be granted temporarily, instead of long-term.