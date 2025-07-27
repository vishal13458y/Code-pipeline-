
# 🚀 DevOps – CI/CD with AWS & Terraform

A clean and practical DevOps project that sets up a complete CI/CD pipeline using AWS services and Terraform. The goal? Automate everything—from pushing code to deploying it on a live EC2 instance—without clicking around manually.

🔧 **Project by 23bcsc69_umesh – Learn. Automate. Repeat.**

---

## 🧩 What This Project Does

- Automatically deploys your code from GitHub to an EC2 server.
- Uses **Terraform** to provision AWS infrastructure.
- Sets up **AWS CodePipeline**, **CodeBuild**, and **CodeDeploy**.
- Runs basic security checks using **GitHub Actions**.
- Tests the Terraform setup using **Terratest**.

All triggered automatically whenever you push new code.

---

## 🛠️ What's Inside

| Tool/Service     | Purpose                                     |
|------------------|---------------------------------------------|
| **Terraform**    | Infrastructure as Code (IaC)                |
| **AWS CodePipeline** | Automates the deployment process      |
| **CodeBuild**    | Builds and tests your code                  |
| **CodeDeploy**   | Deploys to EC2                              |
| **GitHub Actions** | CI + security scan with `tfsec`          |
| **Terratest**    | Infrastructure validation                   |

---

## 📦 Folder Structure

```
DevOps/
├── .github/workflows/        # GitHub Actions workflow
├── terraform/                # Terraform configs
├── examples/                 # Sample buildspec/appspec/scripts
├── test/                     # Terratest files
├── docker/                   # (Optional) Dockerfile
├── main.tf / outputs.tf      # Infra definition
└── terraform.tfvars.example # Fill in your project config here
```

---

## ⚙️ Setup (Simplified)

> Before you begin, make sure you have Git, AWS CLI, Terraform, and Go installed.

1. **Clone the Repo**

```bash
git clone https://github.com/yourusername/devops.git
cd devops
```

2. **Configure AWS CLI**

```bash
aws configure
# Enter your access key, secret key, region, and format
```

3. **Fill in Your Config**

Copy `terraform.tfvars.example` → `terraform.tfvars` and update:

```hcl
project_name       = "myApp"
bucket_name        = "unique-artifact-bucket"
aws_region         = "ap-south-1"
ami_id             = "ami-xxxxxxxx"
instance_type      = "t3.micro"
key_name           = "your-keypair"
github_owner       = "your-github-username"
github_repo        = "your-repo-name"
github_branch      = "main"
github_token       = "your-github-token"
```

> 🔐 Don't commit this file. It has secrets!

4. **Deploy Infra**

```bash
terraform init
terraform apply --auto-approve
```

That’s it! Pipeline is now live 🎉

---

## 🚀 How It Works (Simple View)

1. Push code to GitHub
2. CodePipeline gets triggered
3. CodeBuild builds + runs tests
4. CodeDeploy sends it to EC2
5. Optional: Terratest checks the infra

---

## ✅ App Deployment Checklist

Make sure your app repo has:

- `buildspec.yml` → tells CodeBuild how to build
- `appspec.yml` → required for CodeDeploy
- `scripts/install.sh` → installs dependencies
- `scripts/start.sh` → starts your app (Node, React, etc.)

You’ll find examples in the `examples/` folder.

---

## 🧪 Infra Testing with Terratest

1. Setup Terratest structure:
```
project/
├── terraform/
└── test/
    └── terraform_pipeline_test.go
```

2. Write test like this:

```go
package test

import (
  "testing"
  "github.com/gruntwork-io/terratest/modules/terraform"
  "github.com/stretchr/testify/assert"
)

func TestTerraformPipeline(t *testing.T) {
  tf := &terraform.Options{ TerraformDir: "../terraform" }
  defer terraform.Destroy(t, tf)
  terraform.InitAndApply(t, tf)
  output := terraform.Output(t, tf, "codepipeline_name")
  assert.NotEmpty(t, output)
}
```

3. Run:

```bash
go mod init devops-test
go get github.com/gruntwork-io/terratest/modules/terraform
go get github.com/stretchr/testify/assert
go test ./test
```

---

## 📸 Screenshots

_Add your AWS screenshots here—CodePipeline, CodeBuild, EC2, etc._
_(You can paste images using GitHub markdown or links.)_

---

## 🛠 Common Issues

| Problem                     | Solution                                                              |
|-----------------------------|-----------------------------------------------------------------------|
| `Pipeline failed at Source` | Check repo name, branch, and GitHub token permissions                 |
| `CodeDeploy EC2 error`      | Ensure codedeploy-agent is installed and running on EC2               |
| `Build fails`               | Double-check your `buildspec.yml` and scripts                        |
| `Access Denied`             | IAM role needs correct `s3:GetObject`, CodeDeploy, EC2 permissions    |

---


## 📬 Contact

**Made with 💻 by 23bcsc69_umesh**
Drop a mail: `cse.23bcsc69@silicon.ac.in`
