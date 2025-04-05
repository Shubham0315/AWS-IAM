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
-
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

--------------------------------------------------------------------------------------------------

How would you restrict IAM user access to specific IP addresses?
-
- To restrict IAM users access specific IP we can use IAM policy with condition element and "aws:SourceIP" condition key.
- This ensures IAM user can only access AWS resources from trusted IP addresses or ranges
- Add below kind of policy
  - Effect us to deny ensuring it overrides any allow statements
  - NotIpAddress ensures access is denied unless request comes from trusted IP range
  - * in Action and Resource denies all actions unless conditions are met

![image](https://github.com/user-attachments/assets/17dc9862-3cfc-412b-ac10-7cbb4f385e0a)

- Attach policy to IAM user

--------------------------------------------------------------------------------------------------

Explain the purpose of AWS IAM Conditions in policies.
-
- Conditions are used in IAM policies to add fine grained control over when and how permissions are granted
- By adding conditions, you can define specific requirements that must be met for an action to be allowed or denied.

✅ Enhanced Security: Adds extra layers of control beyond standard permission rules.
✅ Granular Control: Limits permissions based on context such as IP address, time, or MFA status.
✅ Flexibility: Allows dynamic conditions without modifying core policy logic.

--------------------------------------------------------------------------------------------------

How do AWS Service Control Policies (SCP) work with IAM?
-
- AWS SCPs are type of pilicies that we can use to manage permissions in our AWS organizations.
- SCPs are designed to enforce guardrails across multiple AWS accounts in organization, ensuring that no user, role or service within account can perform restricted actions even if they've appropriate IAM permissions.

--------------------------------------------------------------------------------------------------

What is AWS STS (Security Token Service) and how is it used with IAM?
-
- AWS STS is a web sevice that enables you to request temporary, limited privilege credentials for AWS resources.
- These creds are ideal for granting secure, short-term access to resources without need to create long term IAM user credentials.
- To create temporary credentials for third-party applications using IAM roles, you can leverage AWS Security Token Service (STS) with the **AssumeRole** API

- Key Features :-
  - Temporary creds
  - Automatic Expiry
  - Cross account access
  - Federated Access
 
- How STS works with IAM
  - IAM role/user requests temporary creds from STS
  - STS issues temporary creds
  - Temporary creds used by AWS requests
  - AWS verifies creds
 
--------------------------------------------------------------------------------------------------

How can you enforce MFA for IAM roles?
- 
- To enforce MFA for IAM roles, we can use IAM policies with conditions that require MFA authentication.
- This ensures only authenticated users with MFA enabled can assume specific roles

1. Create IAM policy to require MFA

![image](https://github.com/user-attachments/assets/a9da5ae3-93fa-4ff1-849a-a38e088490ac)

  - "Effect": "Deny" :- ensures this rule overrides any allow statements
  - {"aws:MultiFactorAuthPresent": "false"} :- blocks access if MFA not active
  - IfExists :- ensures policy wont deny requests from services that dont support MFA

2. Attach policy to IAM role

3. Update trust policy for the role

4. Test MFA requirement

--------------------------------------------------------------------------------------------------

How do you audit IAM permissions to ensure they follow security best practices?
-
- Auditing IAM permissions is crucial to ensure AWS env follows security best practices

1. Review IAM roles, users, groups
- Ensure they align with least privilege
- Check for inactive users or roles, disable or delete them
- Command :- aws iam get-account-authorization-details --query 'UserDetailList[*].[UserName, CreateDate, PasswordLastUsed]'

2. Audit attached policies
- List inline and managed policies for entities
- Ensure permissions are role based rather than user-specific
- Command (list attached policies) :- aws iam list-attached-user-policies --user-name <username>
- Command (list inline policies ) :- aws iam list-user-policies --user-name <username>

3. Use AWS IAM Access Analyzse
- Helps identify resources that're shared publicly or with external accounts

4. Identify and remove unused permissions

5. Enforce MFA for sensitive accounts
- Enable MFA for root accounts, users
- Use IAM policy to deny access unless MFA is enabled

6. Audit and restrict public access
- Review resource policies and apply the principle of least privilege.

7. Implement SCPs

8. Regularly rotate and manage access keys
- Enforce policy that denies user of long-term access keys

--------------------------------------------------------------------------------------------------

Explain the concept of IAM policy evaluation logic.
-
- Its a structured process that AWS follows to determine whether a given request should be allowed or denied

- Key components of IAM policy evaluation
  - Explicit deny :- strongest priority, always overrides any allow
  - Explicit allow :- permits action if no explicit deny exist
  - Implicit deny :- if now allow rule exist, request is denies by default
 
- IAM policy Evaluation Process Flow
  - Evaluate organization SCPs
  - Evaluate resource based policies
  - Evaluate IAM permission boundaries
  - Evaluate identity based policies
  - Evaluate session policiesImplicit deny (default behaviour)
 
--------------------------------------------------------------------------------------------------

How can you rotate IAM access keys securely?
-
- Rotating AWS IAM Access keys is essential to minimise risk of compromised credentials

- Create New access key
  - Command :- **aws iam create-acces-key --user-name shubham315**
  - Write below JSON to create

![image](https://github.com/user-attachments/assets/4559b13e-ac8b-447e-a36e-7405416964d9)

- Update apps with new key
  - Identify all systems, scripts or apps using old access key
  - update those with new access key and secret key
 
- Test new access key

- Deactivate old access key
  - Command :- **aws iam update-access-key --access-key-id <OldAccessKeyID> --status Inactive --user-name <username>**
 
- Confirm no apps using old key
  - Command :- **aws iam get-access-key-last-used --access-key-id <OldAccessKeyID>**

- Delete old access key
  - Command :- **aws iam delete-access-key --access-key-id <OldAccessKeyID> --user-name <username>**

--------------------------------------------------------------------------------------------------

What is a trust policy in AWS IAM, and how does it differ from an identity-based policy?
-
1. Trust Policy (Who can assume a role)
- It is a JSON document defining who is allowed to assume specific role.
- Attached only to IAM roles, defines who can assume role not what actions can they perform
- Used "sts:AssumeRole" action to give permissions

![image](https://github.com/user-attachments/assets/931b7ec3-0376-44ff-bc94-16c63b600ce3)

2. Identity based policy (What actions are allowed)
- Its a JSON document attached to IAM users, groups and roles. Defines what actions are allowed or denied on AWS resources
- Specifies what actions or on which resources user or role can operate
- Can include conditions for additional security controls

- Trust policy grants third party acccount permission to assume role in AWS account
- Identity based policy grants assumed role permission to manage specific resources

--------------------------------------------------------------------------------------------------

How do you use AWS Organizations in combination with IAM for centralized security management?
-
- Combining AWS Organizations with IAM enhances security and governance by enabling centralized control over multiple AWS accounts. This setup is ideal for implementing consistent security policies, managing permissions, and ensuring compliance across your cloud environment.

- Key features of AWS Organizations for security management
  - Centralized policy management
  - SCPs
  - Automated account creation
 
- To use this we can
  - Implement SCPs
  - Enforce Identity based policies for granular control

--------------------------------------------------------------------------------------------------

What are JSON policy elements like Effect, Action, Resource, and Condition?
-
- Effect :- specifies whether statement allows or denies action
- Action :- lists specific AWS actions that policy applies to
- Resource :- Specifies AWS resource the policy applies to
- Condition :- Adds conditional logic to apply permissions only if specific conditions are met

--------------------------------------------------------------------------------------------------

How do IAM roles integrate with Amazon EC2 instances securely?
-
- IAM roles integrate with EC2 securely using instance profiles, which allow EC2 instances to obtain temporary security creds automatically.
- This way we can securely grant permissions to apps in EC2 without hardcoding access keys

- Create IAM role for EC2
- Attach role to EC2
- Verify role access from EC2
- Access AWS services securely

--------------------------------------------------------------------------------------------------

What is the difference between AWS:SourceIp and AWS:VpcSourceIp in IAM conditions?
-
- In AWS IAM policies, the aws:SourceIp and aws:VpcSourceIp condition keys are both used to specify IP address-based conditions for granting or denying access, but they operate in slightly different contexts

- **aws:SourceIP** :- Refers to IP of client making request, from anywhere. Use this when we want to limit access based on public or external IP addresses
- **aws:VpcSourceIp** :- Refers to IP within VPC that makes request. Use this when we need to control access based on network of our VPC such as enforcing that only EC2 within certain subnet can access resource
