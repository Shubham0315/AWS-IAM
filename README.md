# AWS-IAM
How IAM is used for Authentication and Authorization in AWS covering topics like Users, Policies, Groups, Roles

- Suppose in a bank we've service desk, employee services, sensitive data. To keep the things secure, bank give fine grained access to people.
- Here to enter a bank, person should be authenticated (having bank account). Then they verify what kind of authorization that person has like bank employee, account holder and depending on that person is diverted to respective services desk.
- So bank setup a dedicated authentication and authorization process

- Suppose we're workimg for a company and we've AWS account where we've services like EC2, DB, K8S. Account is created using gmail id as root so we can do anything in account
-   If there is no authorization and authentication process, devops engineer will give access to everyone in company. So other people can have root permissions as well and they can access the services of root and can delete our work which is a Disadvantage.

-   Thus AWS comes up with IAM (Identity and Access Management) which does authentication and authorization for us
-   Here when devops engineer who is root doesnt share root access to anyone
-   For any new joiner, needs access to any service, they ask devops engineer for the access. Devops engineer asks for which service they need.  After giving info devops engineer creates user for that person, provides access to specific services using policies
- Devops engineer goes to IAM and create user for employee and provide him required access using IAM policies.
- So user's ID and password is shared with user and he can access the AWS services
  e.g:- If we have given that permission to just read the EC2, he cant delete it as he dont have permission for same

- **Authentication** :- Create user
- **Authorization** :- Provide access

----------------------------------------------------------------------------------------

IAM Components
-
#1. Users
- Creating user comes under authentication part.

#2. Policies
- When we create user, we also attach some policies to it so as to access the services. This comes under authorization

#3. Groups
- In an organization, people join and leave frequently. So creating new user everytime and attach policies to them, it becomes time consuming and manual activity.(As devops is all about efficiency)
- So we can automate the policies. For dev, QA, DBA we can create groups. If a new joiner comes, as a devops engineer we'll ask employee to create JIRA request and tell his group. So depending on it, we'll create user for the employee and put it in the group

#4. Roles
- Policies and groups are related to users/people.
- For a developer who is using DB service in AWS. Instead of creating DB service in local machine, we created DB service in AWS but our app is in private cloud. When customer tries to access application in private cloud, we need to get info from DB service in AWS and give it to customer. Here we cant create user as we're trying to access application from AWS.
- So here we can create role
- So here we can tell developer when he is trying to access AWS, use the role to access AWS. Role will also have username and password which will be temporary.
- Roles are similar to users (username and pass) as they are temporary. Used to talk between 2 AWS accounts
- Roles are created for temporary purpose where outside persons can access any service in our cloud on temporary basis

![image](https://github.com/user-attachments/assets/080f021d-fc14-475d-8c54-e8405484f7bc)

----------------------------------------------------------------------------------------
