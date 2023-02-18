# Week 0 — Billing and Architecture



I learned about the AWS CLI for the first time and had to set it up in my gitpod environment

**Installing AWS CLI**
I used the following command to update my gitpod environment

```
tasks:
  - name: aws-cli
    env:
      AWS_CLI_AUTO_PROMPT: on-partial
    init: |
      cd /workspace
      curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      unzip awscliv2.zip
      sudo ./aws/install
      cd $THEIA_WORKSPACE_ROOT
      
  ```


<img width="970" alt="Screenshot 2023-02-18 at 5 55 14 AM" src="https://user-images.githubusercontent.com/1076924/219838099-f81ca5b8-3afc-4d37-8f7a-a993a138cde0.png">

## Creating a new user and generating AWS credentials

- On my AWS console I created a new user and granted AdministratorAccess to that user
- I created an access key for this user and saved the credentials
- 
<img width="1304" alt="Screenshot 2023-02-18 at 5 58 30 AM" src="https://user-images.githubusercontent.com/1076924/219838802-e3b17e7c-49cf-478d-b0e7-a2e576821ff1.png">


##Setting environment variables
In the bash terminal i had to set these credentials

```
export AWS_ACCESS_KEY_ID=""
export AWS_SECRET_ACCESS_KEY=""
export AWS_DEFAULT_REGION=us-east-1

```
Then ensure that gitpod remembers
```
gp env AWS_ACCESS_KEY_ID=""
gp env AWS_SECRET_ACCESS_KEY=""
gp env AWS_DEFAULT_REGION=us-east-1

```

## Checking that AWS CLI is working and that I am the expected user

`aws sts get-caller-identity`

<img width="675" alt="Screenshot 2023-02-18 at 10 57 38 AM" src="https://user-images.githubusercontent.com/1076924/219854166-c3a7212f-30a5-4bb7-a033-b2e87f9c8539.png">

## Enabling billing


