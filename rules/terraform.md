---
name: "Terraform Rules"
description: "Rules for AI agents on writing clean, maintainable, and secure Terraform code"
category: "Infrastructure as Code"
icon: "terraform.svg"
version: "1.0"
globs:
  - "**/*.tf"
  - "**/*.tfvars"
---

- Organize Terraform code into logical files:

```
terraform/
├── main.tf           # Primary resource definitions
├── variables.tf      # Input variable declarations
├── outputs.tf        # Output value declarations
├── versions.tf       # Provider version constraints
├── terraform.tfvars  # Variable values (gitignored if sensitive)
└── modules/          # Reusable modules
    └── vpc/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

- Keep modules focused on a single responsibility
- Use clear, descriptive names for modules
- Document module inputs and outputs
- Version your modules appropriately
- Use descriptive names that indicate the resource type and purpose:

```hcl
resource "aws_instance" "web_server" {
  # Good: Clear purpose
}

resource "aws_instance" "i1" {
  # Bad: Unclear purpose
}
```

- Use snake_case for variable names:

```hcl
variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t3.micro"
}
```

- Always use remote state with encryption
- Enable state locking to prevent concurrent modifications
- Use separate state files for different environments

```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}
```

- Always format code with `terraform fmt`:

```bash
terraform fmt -recursive
```

- Add comments for complex logic:

```hcl
# Calculate the CIDR blocks for private subnets
# Each subnet gets a /24 block from the VPC CIDR
locals {
  private_subnet_cidrs = [
    for i in range(var.az_count) :
    cidrsubnet(var.vpc_cidr, 8, i)
  ]
}
```

- Use validation blocks for variables:

```hcl
variable "environment" {
  type        = string
  description = "Environment name"
  
  validation {
    condition     = contains(["dev", "staging", "prod"], var.environment)
    error_message = "Environment must be dev, staging, or prod."
  }
}
```

- Use locals for complex expressions:

```hcl
locals {
  common_tags = {
    Project     = var.project_name
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
  
  instance_name = "${var.project_name}-${var.environment}-instance"
}

resource "aws_instance" "app" {
  tags = merge(
    local.common_tags,
    {
      Name = local.instance_name
    }
  )
}
```

- Use `depends_on` when implicit dependencies aren't sufficient:

```hcl
resource "aws_iam_role_policy" "example" {
  depends_on = [aws_iam_role.example]
  
  role   = aws_iam_role.example.name
  policy = data.aws_iam_policy_document.example.json
}
```

- Provide useful outputs:

```hcl
output "instance_public_ip" {
  description = "Public IP address of the EC2 instance"
  value       = aws_instance.web.public_ip
}

output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.web.id
}
```

- Pin provider versions:

```hcl
terraform {
  required_version = ">= 1.0"
  
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
```

- Prefer `for_each` over `count` for better resource addressing:

```hcl
# Good: Using for_each
variable "instances" {
  type = map(object({
    instance_type = string
    ami           = string
  }))
}

resource "aws_instance" "app" {
  for_each = var.instances
  
  instance_type = each.value.instance_type
  ami           = each.value.ami
  
  tags = {
    Name = each.key
  }
}
```

- Use `try()` function for safe attribute access
- Provide meaningful error messages in validations
- Use `can()` function to test expressions

```hcl
locals {
  # Safely access potentially missing attributes
  instance_name = try(var.instance_config.name, "default-instance")
}
```

- Always include README.md with module description and usage examples
- Always include variable descriptions
- Always include output descriptions
- Always include examples directory with working configurations
- Write Terraform code that is readable, maintainable, and follows the principle of least privilege for security
