# AWS-IAM
How IAM is used for Authentication and Authorization in AWS covering topics like Users, Policies, Groups, Roles

- Suppose in a bank we've service desk, employee services, sensitive data. To keep the things secure, bank give fine grained access to people.
- Here to enter a bank, person should be authenticated (having bank account). Then they verify what kind of authorization that person has like bank employee, account holder and depending on that person is diverted to respective services desk.
- So bank does setup a dedicated authentication and authorization process

- Suppose we're working for a company and we've AWS account where we've services like EC2, DB, K8S. Account is created using gmail id as root so we can do anything in account
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

# Practical Demo

Users and Policies
-
- Login to AWS console using root account. Using root user we can technically do everything in AWS
- Using IAM we avoid issues of deleting resources by users by use of authentication and authorization.

- Go to IAM. If anyone requests access, we first require to create user and then attach policies to the user
  - Create user - Provide name - Provide user access checkbox - I want to create IAM user - Autogenerated password (for first login, then user can change)
    
![image](https://github.com/user-attachments/assets/158c101a-bb23-4acf-b00f-f18144108285)
![image](https://github.com/user-attachments/assets/6933fc70-026a-4163-a7d1-a74f0e31644a)
![image](https://github.com/user-attachments/assets/fc5c0548-ba0c-47fa-96f4-13e83960d9f6)

  - Now when prompted on next, it will ask to attach policies. For now keep default so no authorization is provided to user. Without any authorization, user cannot create, delete, edit resources

![image](https://github.com/user-attachments/assets/a254333d-06a6-4585-a2c8-b2f61cadadc0)

  - Now when clicked next, only one policy is attached to change the password by default - Create user

![image](https://github.com/user-attachments/assets/eee9ee73-9fa4-427c-a5a8-9de050d4122a)

  - We can check the user details once user is created. This can be shared as is or we can download the CSV and share to user

![image](https://github.com/user-attachments/assets/d45ecc7c-e611-4dbf-a709-b9c3501e8c2b)


- Now logout from root user and login with IAM user. It will ask for account ID first and then user credentials. Account ID is in CSV link

![image](https://github.com/user-attachments/assets/4d53321a-f926-4332-91f5-425856462281)

- This will now ask for password reset. After it, We can see we've logged in to console

![image](https://github.com/user-attachments/assets/915e8e18-ffb3-4624-984e-82343e3fe7e2)
![image](https://github.com/user-attachments/assets/b22eb2fc-0c60-4462-b347-43fb9f73c3d4)

- When we try to check any service, we get access denied as policies to it are not attached

![image](https://github.com/user-attachments/assets/f4566b58-4a24-4f86-9c48-44c4ae93512c)
![image](https://github.com/user-attachments/assets/28886835-a7d5-4d34-9f68-032698d497d6)

- So using authentication, we can just view resources and not modify them.

- This is solved by authorization. We can provide access to any specific resource.

- Now sign in with root user
  - Go to created IAM user
  - Add permissions
  - Attach policies directly. Here are some default policies listed which are AWS managed.
  - To attach custom policies of own, we can write custom policy
 
![image](https://github.com/user-attachments/assets/c476c0a2-d760-4154-9abe-cf57fe8074c4)
<img width="959" alt="image" src="https://github.com/user-attachments/assets/c8a5625f-9a17-40d2-bd8f-c5f7cb6e6363" />
<img width="959" alt="image" src="https://github.com/user-attachments/assets/6809110d-ed89-412f-8150-a1253a59fc39" />

  - If we add AWSS3FullAccess and expand it, we will get JSON formatted file to define policy structure. In custom policies as well we can use this template.
  - Add this permission Policy. Create it. We can check user has the policy attached now

![image](https://github.com/user-attachments/assets/d17b7793-7df0-406b-b7a8-50907bc52140)
<img width="959" alt="image" src="https://github.com/user-attachments/assets/0a757554-e9d9-4c1f-a5b5-761420d84441" />
<img width="959" alt="image" src="https://github.com/user-attachments/assets/5eb07fc2-b528-405d-a2ef-c2a101ed8669" />


- Now we can login using IAM user and check the S3 access is granted or not.
  - Using IAM now, we can list S3 buckets, create, modify them.


Groups
-
- We've created IAM user and have attached policies to it.
- For any new user, we can create user and attach it to a group so the pollicies granted to that group will be inherited to the user directly.
- For custom policies, groups gets organizaed way to attach policies to users. This way we can reduce manual efforts and increase efficiency of authentication and authorization

- Now login using root.
  - Create a group and attach policy 

![image](https://github.com/user-attachments/assets/14503133-cea5-4c9d-9230-ff8f7a9696f6)
<img width="959" alt="image" src="https://github.com/user-attachments/assets/504a0857-3ad3-488a-b8d8-8895ddc57a60" />

  - Now we can add users to group

![image](https://github.com/user-attachments/assets/5cf8d7b4-2bf5-4118-a79c-d2330d890b26)
![image](https://github.com/user-attachments/assets/256d129e-7c40-48e0-a335-9a2e1e021340)
![image](https://github.com/user-attachments/assets/ed252096-058e-40d9-95fe-56726e2457c0)

  - So if after some days, the users in the group requests for another policy, just add the policy to group so that it will be applied to all users inside this specific group
  - Go to group and add permissions

![image](https://github.com/user-attachments/assets/a683d56f-92a3-4aff-b947-d1ea7befa41e)

  
