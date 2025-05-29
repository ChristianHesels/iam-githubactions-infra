# Terraform Modul: GitHub OIDC IAM Role

Dieses Modul erstellt eine IAM-Rolle inklusive Policy Attachment, die es GitHub Actions erlaubt, sich über OIDC (OpenID Connect) bei AWS zu authentifizieren und definierte Aktionen auszuführen.

## Features

- Holt den OIDC Identity Provider ARN über ein zentrales `terraform_remote_state`-Backend.
- Erstellt eine IAM-Rolle, die auf ein spezifisches GitHub-Repository beschränkt ist.
- Bindet eine benutzerdefinierte IAM-Policy an die Rolle.

## Input Variablen

| Name | Beschreibung | Typ | Erforderlich |
|------|--------------|-----|--------------|
| `bucket_name_global_infra` | S3-Bucket, in dem der globale Terraform-State liegt | `string` | ✅ |
| `iam_role_name` | Name der IAM-Rolle, die erstellt werden soll | `string` | ✅ |
| `policy_name` | Name der IAM-Policy | `string` | ✅ |
| `json_policy` | Inhalt der Policy als JSON-String | `string` | ✅ |
| `github_repo` | Repo-Pattern für GitHub Actions (z. B. `"repo:username/repo:*"`) | `string` | ✅ |

## Beispielverwendung

```hcl
module "github_oidc_iam" {
  source = "./modules/github-oidc-iam"

  bucket_name_global_infra = "terraform-state-bucket-infra-global"
  iam_role_name            = "github-actions-role"
  policy_name              = "github-s3-cloudfront-access"
  json_policy              = file("policy.json")
  github_repo              = "repo:ChristianHesels/milo030-infra-website:*"
}
