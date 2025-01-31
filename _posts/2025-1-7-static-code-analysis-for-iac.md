---
title: "Static Code Analysis for IaC"
categories:
  - Guides
classes: wide
tags:
  - IaC
  - Infrastructure as Code
  - Security
  - Cloud Operations
  - Misconfiguration
  - Terraform
  - CloudFormation
  - Kubernetes
---

Static code analysis is a technique that examines source code without actually running the program. It's like proofreading a document for errors before you publish it. 

Here's why it's so important:
* Early Bug Detection
* Improved Code Quality
* Enhanced Security
* Reduced Development Costs
* Increased Developer Productivity
* Compliance with Standards
* Better Maintainability

## Checkov for IaC

Checkov is a static code analysis tool that scans Infrastructure as Code (IaC) files for security and compliance misconfigurations

1. Proactive Security: Checkov helps you catch security vulnerabilities before they make it into your live environment. This is much better than reacting to a breach after it has occurred.   
2. Compliance: Checkov has built-in checks that align with industry standards and regulations like CIS Benchmarks, PCI DSS, and HIPAA. This helps you ensure your infrastructure meets necessary compliance requirements.   
3. Supports Multiple IaC Frameworks: Whether you use Terraform, CloudFormation, Kubernetes, or other IaC tools, Checkov can likely scan your code.   
4. Customizable: You can tailor Checkov with custom rules to match your organization's specific security policies and best practices.   
5. Integration: Checkov can be integrated into your CI/CD pipeline, providing automated security checks with every code change.   
6. Open Source: Checkov is open source, meaning it's free to use and has a strong community contributing to its development.   
7.  Detailed Reporting: Checkov provides clear reports of its findings, making it easy to understand the issues and how to fix them.

## Checkov inegration with CI/CD 

Checkov offers a variety of options for CI/CD integration, allowing you to seamlessly incorporate security checks into your existing workflows.

**Github Action**

You can use GitHub Actions to run Checkov on every push or pull request to your repository. This allows you to catch security issues early in the development process

```
steps:
  # Checks-out your repository under $GITHUB_WORKSPACE, so follow-up steps can access it
  - uses: actions/checkout@v3

  - name: Checkov GitHub Action
    uses: bridgecrewio/checkov-action@v12
    with:
      # This will add both a CLI output to the console and create a results.sarif file
      output_format: cli,sarif
      output_file_path: console,results.sarif
```

> [!NOTE]
> Currently Checkov Doesn’t support tfvars file. So if you are already using tfvars, you need to use the tfplan as input to scan

```
steps:
- name: Checkout repo
  uses: actions/checkout@master

- name: Terraform Init
  run: |
    terraform init

- name: Terraform Plan
  run: |
    terraform plan -input=false  -out=tfplan

- name: Convert Terraform Plan to JSON
  run: | 
    terraform show -json tfplan > tfplan.json

- name: Run Checkov Action
  uses: bridgecrewio/checkov-action@v12
  with:
    file: tfplan.json
    framework: terraform
    output_format: cli,sarif
    output_file_path: results.sarif
```

**GitLab CI**

Similar to GitHub Actions, GitLab CI/CD allows you to define pipelines that include Checkov scans


```
stages:
    - test

checkov:
  stage: test
  allow_failure: true  # True for AutoDevOps compatibility
  image:
    name: bridgecrew/checkov:latest
    entrypoint:
      - '/usr/bin/env'
      - 'PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin'
  rules:
    - if: $SAST_DISABLED
      when: never
    - if: $CI_COMMIT_BRANCH
      exists:
        - '**/*.yml'
        - '**/*.yaml'
        - '**/*.json'
        - '**/*.template'
        - '**/*.tf'      
        - '**/serverless.yml'
        - '**/serverless.yaml'
  script:
    # Use `script` to emulate `tty` for colored output.
    - script -q -c 'checkov -d . ; echo $? > CKVEXIT'
    - exit $(cat CKVEXIT)

```
> [!NOTE]
> Currently Checkov Doesn’t support tfvars file. So if you are already using tfvars, you need to use the tfplan as input to scan

```
stages:
    - test
    - tfplan
    - tfinitgitlabg

terraform:
  stage: tfplan
  script:
    - terraform plan -out "tfplan"
    - terraform show -json tfplan > tfplan.json

checkov:
  stage: test
  allow_failure: true  # True for AutoDevOps compatibility
  image:
    name: bridgecrew/checkov:latest
    entrypoint:
      - '/usr/bin/env'
      - 'PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin'
  rules:
    - if: $SAST_DISABLED
      when: never
  script:
    - script -q -c 'checkov -f tfplan.json ; echo $? > CKVEXIT'
    - exit $(cat CKVEXIT)
```
