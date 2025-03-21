# Interview Questions

What is AWS IAM?
-
- AWS IAM (Identity and Access Management) is a web service that enables us to securely manage access to AWS services and resources.
- It allows us to create and manager users, groups, roles and policies to control who can access which services in our AWS account
- 4 Key components of IAM :-
  - Users :- Represent individual identities with specific permissions. Each user has unique credentials
  - Groups :- Collection of IAM users with shared permissions. Used to assign common permissions to multiple users
  - Roles :- Used to grant permissions to AWS services or external identities (EC2 accessing S3 bucket)
  - Policies :- JSON documents to define permissions to users, groups, roles to specify actions.
 
- Core Features :-
  - Granular permissions
  - Secure access
  - Temporary credentials :- roles can provide temporary credentials for specific tasks
  - Audit and Monitoring
 
--------------------------------------------------------------------------------------------------

What are the key features of IAM?
-
- Centralized access Control
- Granular permissions
- Secure authentication :- Supports MFA to enhance security
- Free to use

- IAM can help to create role for jenkins to securely deploy pipelines, define policies to restrict sensitive data in S3

--------------------------------------------------------------------------------------------------

What is IAM user?
- 
- IAM user is an AWS entity that represents person or application that interacts with AWS services
- Key Characteristics :-
  - Unique Identity of each IAM user
  - Permissions to be granted directly or through groups
  - Group membership to assign common permissions
  - MFA support to enhance security to AWS services
 
- Create IAM user - Assign user to group - Attach policy to group

- Follow principle of least privilege, enable MFA, avoid using root user for routine tasks, regularly review and rotate access keys

--------------------------------------------------------------------------------------------------

What is an IAM Role? How is it different from an IAM User?
-
- IAM role is an AWS identity that defines set of permissions for making AWS service requests.
- Unlike IAM user, role doesnt have permenant credentials, they've temporary ones for entities like AWS services, AWS accounts, etc

- Roles represent set of permissions, not tied to specific person unlike users
- Roles are designed for services while users are for individuals

- Use users for developers, admins or individuals who require direct access to AWS resources
- Use roles for services, apps where temporary credentials are preferred for enhanced security

- Example scenario :-
  - Suppose we've EC2 that needs to read files from S3
  - Create IAM role with S3 read permissions and attach it to EC2 instance. Instance will auto obtain temporary credentials through instance metadata
 
--------------------------------------------------------------------------------------------------

What are IAM Groups?
-
- IAM group is a collection of IAM users that share same set of permissions.
- Groups simplify permission management by allowing you to assign policies to multiple users at once rather than assigning permissions to each user individually

- Key Features
  - User management
  - Permission control
  - Policy inheritance from multiple groups
  - Simplified management
 
- Exaample scenario :-
  - Suppose our organization have teams like dev, admins, support
  - We can create groups for respective teams, assign relevant policies to each group, add users to appropriate groups


--------------------------------------------------------------------------------------------------

How does IAM help manage permissions in AWS?
-
- AWS Identity and Access Management (IAM) is a crucial service for managing access to AWS resources securely. It allows you to control who can access your resources (authentication) and what actions they can perform (authorization).
- To manage permissions we can use
  - Users and groups
  - Policies :- JSON based document to define permissions attached to users, groups, roles
  - Setup MFA
  - Use least privilege access principle
  - Regularly review and audit permissions and policies

--------------------------------------------------------------------------------------------------

What is the difference between an inline policy and a managed policy?
-
- **Inline Policies**
  - Embedded directly within user, group, role
  - Tightly coupled with identity they're attached to, if identity is deleted inline policy is also deleted
  - Used to assign temporary permissions for specific task or role
 
- **Managed Policies**
  - Created independently from users, groups and roles
  - Can be reused across multiple identities
  - 2 types AWS managed (created and maintained by AWS), Customer managed (flexible for customer use).
  - Easier to update as changes are auto applied to attached identities
  - AmazonS3ReadOnlyAccess :- AWS managed
 
--------------------------------------------------------------------------------------------------

What is the maximum number of IAM users that can be created per AWS account?
-
- In AWS we can create upto 5000 IAM users per account. This limit is adjustable by submitting a request to AWS support if we need more users

--------------------------------------------------------------------------------------------------

Can one IAM user belong to multiple groups?
- 
- Yes, an IAM user can belong to multiple IAM groups in AWS. This is useful for assigning multiple sets of permissions efficiently.
- IAM user can be added upto 10 groups
- Users effective permissions are sum of all permissions granted by attached policies in those groups
- If there's a conflict (if one group allows and other denies), deny takes preference

--------------------------------------------------------------------------------------------------

What are IAM access keys and when are they used?
-
- AWS access keys are set of credentials used to authenticate programmatic access to AWS services (APIs) via AWS CLI, AWS SDK or API requests
- Key components
  - Access key ID :- Public identifier like username
  - Secret Access key :- Private key like password.
 
- IAM keys are used when we need to interact with resources using CLI, SDK, API or automate tasks via scripts, pipelines or manage infrastructure using tools like ansible, terraform

- Avoid creating access keys for IAM users instead use IAM roles.
- Rotate them regularly, apply least privilege attaching minimal permissions, store keys in AWS secret manager, never hardcode them

--------------------------------------------------------------------------------------------------

How can you enforce multi-factor authentication (MFA) for IAM users?
-
- Enable MFA for IAM users
  - Select user - Security Creds - Multi Factor Auth add MFA device - Choose MFA device

<img width="959" alt="image" src="https://github.com/user-attachments/assets/0a04e894-c67d-48f8-a926-fb1758900dd4" />

- Enforce MFA using IAM policy
  - To ensure all IAM users can only access AWS resources after enabling MFA, you can create and attach conditional policy
  - This policy denies all actions unless MFA is enabled
 
![image](https://github.com/user-attachments/assets/55321206-643f-4c69-8957-d8620836d8c1)

- Attach Policy
  - Policies - Create Policy - Choose JSON tab and paste MFA enforcement policy - Provide name and create policy
  - Attach to IAM users or gorups
 
![image](https://github.com/user-attachments/assets/e942d536-95ae-4907-a313-280cb564d9f6)

- Test MFA Enforcement
  - Try accessing resources without MFA, request should be denied
  - We need to enable MFA here
