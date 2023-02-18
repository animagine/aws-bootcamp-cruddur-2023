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


