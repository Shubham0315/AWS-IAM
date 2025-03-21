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

--------------------------------------------------------------------------------------------------

What is the principle of least privilege in IAM?
-
- Principle of lest privilege in IAM is a security practice that ensures roles, users and services are granted only permissions required to perform the specific task.
- It reduces risk of accidental data leaks and unauthorized actions. Milits impact of compromised credentials. Helps to maintain secure AWS env

- To implement PoLP in IAM
  - Start with deny by default - IAM follows implicitly deny rule. Only grant permissions necessary
  - Use IAM groups and roles - Instead of individual users to simplify permission management
  - Apply resource level permissions
 
![image](https://github.com/user-attachments/assets/2a384d62-657f-4856-b0c7-bf22da56ffad)

  - Use conditions for granular control - Conditions like restrict access based on IP address, time of day, MFA status, etc

![image](https://github.com/user-attachments/assets/a9bbc675-de6a-4169-bc48-c339acdb3737)

--------------------------------------------------------------------------------------------------

How do you troubleshoot "Access Denied" errors in AWS?
-
- Identify failing user/role and request details
- Verify IAM policies if any incorrect permissions attached
- Confirm MFA and session conditions in policies

--------------------------------------------------------------------------------------------------

What is the difference between identity-based and resource-based policies?
-
- These are 2 types of IAM policies that control access to resources, but they differ in purpose, attachment method and scope

- Identity based Policies
  - Attached tdirectly to IAM identities like user, group and roles to perform specific actions
  - Supports AWS managed and customer managed policies
  - Include allow and deny statements
  - Use conditions to restrict access

![image](https://github.com/user-attachments/assets/a510ed89-9a7b-42e5-8eb5-2fe62f28873b)

- Resource based Policies
  - Attached directly to AWS resources (s3, lambda, SNS)
  - Specify who can access resource and what actions they can perform
  - Only support allow statement
 
![image](https://github.com/user-attachments/assets/e83bcd25-e9d6-4752-9036-079ccd3c9f98)

--------------------------------------------------------------------------------------------------

What is an IAM permissions boundary?
-
- It is an advanced IAM feature that defines maximum permissions IAM user or role can have.
- Ensures identity cannot perform any action outside specified boundary

- It is identity based policy that sets maximum allowed permissions.
- AWS evaluates permissions boundary alongside identity's attached policies
- To perform action, both permissions boundary and identity based policy must allow it.

- If we've IAM role with identity policy that allows full access to S3.
  - In policy we've allowed all s3 action
  - But in permission boundary we've obnly allowed to list objects of S3
  - So the role attached can only read and list the objects
 
- To create and attach permission boundary
  - Create policy
  - Attach policy as Permissions boundary :- select user/role - set permission boundary
 
--------------------------------------------------------------------------------------------------

How can you delegate permissions across AWS accounts?
-

--------------------------------------------------------------------------------------------------

What is the AWS IAM best practice for granting permissions to applications?
-
- Use principle of least privilege and use IAM roles instead of hardcoding credentials
- Use managed policies for common permissions
- Implement IAM permission boundaries
- Enable MFA
- Use resource based policies for cross account access
- Rotate access keys regularly

--------------------------------------------------------------------------------------------------

How can you delegate permissions across AWS accounts?
- We can use IAM roles with cross-account access
- It allows secure, controlled access between AWS accounts without sharing security credentials

1. Identify accounts
- Source (account where trusted user exists) and target account (account where resources are located and IAM role to be created)

2. Create IAM role in target account
- Create roles - Select AWS account as trusted entity - Enter account ID of trusted account (source) - Attach permissions

3. Grant users permission to assume role
- Go to IAM role in source account
- Create policy - Add to user or group

4. Assume role from Source
- Command :- aws sts assume-role \
  --role-arn "arn:aws:iam::123456789012:role/CrossAccountS3Access" \
  --role-session-name "CrossAccountSession"
- We'll get credentials in JSON, use them to access resources in account target
