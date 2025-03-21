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
