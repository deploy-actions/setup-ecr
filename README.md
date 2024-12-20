# Login Amazon ECR and Setup ECR Repository

## About

This action logs into the amazon ecr service and creates a repository with the `RepositoryName` if it doesn't already exist. If `ImageTagMutability` is entered, the settings will be updated to match the entered value.

## Simple Example of Usage

```yml
- name: Configure AWS credentials 🔑
  uses: aws-actions/configure-aws-credentials@v4
  with:
    role-to-assume: ${{ vars.AWS_ROLE_ARN }}
    aws-region: ${{ vars.AWS_REGION }}

- name: Login to Amazon ECR
  id: ecr
  uses: aws-actions/amazon-ecr-login@v2

- name: Setup ECR Repository
  uses: deploy-actions/setup-ecr@main
  with:
    RepositoryName: simple-api

- name: Setup Docker Buildx
  uses: docker/setup-buildx-action@v3

- name: Build and push
  uses: docker/build-push-action@v6
  with:
    push: true
    tags: ${{ steps.ecr.outputs.registry }}/simple-api:latest
```

## Inputs

| Name               | Description                                                                                                          | Mandatory | Default                  |
| ------------------ | -------------------------------------------------------------------------------------------------------------------- | --------- | ------------------------ |
| RepositoryName     | ECR Repository Name                                                                                                  | ✅        |                          |
| AwsRegion          | AWS Region, e.g. us-east-2                                                                                           |           | env AWS_REGION           |
| RegistryId         | Repository's Registry ID (AWS Account ID)                                                                            |           | aws configure account id |
| ImageTagMutability | The tag mutability setting for the repository. This input is always applied. Only `MUTABLE` or `IMMUTABLE` is valid. |           |                          |
