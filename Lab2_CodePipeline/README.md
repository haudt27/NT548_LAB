
# Deploying CloudFormation Nested Stack with CodeCommit, CodeBuild, and CodePipeline

This guide describes how to deploy a **CloudFormation Nested Stack** automatically using AWS services: **CodeCommit**, **CodeBuild**, and **CodePipeline**. This process automates the management of complex resources by breaking them into manageable nested stacks.

---

## Prerequisites

- **AWS Account** with sufficient permissions to access and create the following services:
  - CodeCommit
  - CodeBuild
  - CodePipeline
  - CloudFormation
  - IAM
- **AWS CLI** (latest version).
- **Git** for source code management.
- **S3 Bucket** for storing CloudFormation templates (if needed).


---

## Project Structure

The project is organized as follows:

```
├── main-stack.yaml              # Main stack (references nested stacks)
├── nested-stack1.yaml           # Nested stack 1
├── nested-stack2.yaml           # Nested stack 2
├── buildspec.yml                # Build specification for CodeBuild
├── pipeline.yaml                # CloudFormation template to create CodePipeline
└── README.md                    # Documentation
```

---

## Deployment Steps

### 1. Create a Repository in CodeCommit

1. Log in to the [AWS Management Console](https://aws.amazon.com/console/).
2. Navigate to **CodeCommit** and create a new repository, e.g., `haudt`.
3. Connect the repository to Git on your computer:
   ```bash
   git clone https://git-codecommit.<region>.amazonaws.com/v1/repos/haudt
   cd haudt
   ```

### 2. Add Source Code and Push to CodeCommit

1. Copy `main-stack.yaml`,  and `buildspec.yml` to the repository folder.
2. Commit and push the source code:
   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

### 3. Create IAM Roles

1. Create IAM Roles for **CodeBuild** and **CodePipeline** with the necessary permissions:
   - **CodeBuild**:
     - `cloudformation:CreateStack`
     - `cloudformation:UpdateStack`
     - `s3:PutObject`
     - `s3:GetObject`
   - **CodePipeline**:
     - `codecommit:GetBranch`
     - `codebuild:StartBuild`
     - Permissions for all pipeline actions.

2. Attach a trust policy allowing CodeBuild and CodePipeline to assume these roles.

### 4. Configure BuildSpec

Create the `buildspec.yml` file for CodeBuild:
check **buildspec.yaml**

### 5. Create the Pipeline 

### 6. Activate the Pipeline

1. Navigate to **AWS CodePipeline** and verify the newly created pipeline.
2. Activate the pipeline. It will:
   - Fetch source code from CodeCommit.
   - Build and deploy the main CloudFormation stack.
   - Create nested stacks referenced in `main-stack.yaml`.

---

## Notes

- **S3 Bucket**: Ensure the bucket used for storing templates is configured with the appropriate access permissions.
- **IAM Policies**: Grant only the necessary permissions to each role to minimize security risks.


