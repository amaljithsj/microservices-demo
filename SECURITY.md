# Security Notes

## EKS Secrets Encryption — AVD-AWS-0039

Trivy reports `AVD-AWS-0039` for the EKS cluster because the Terraform
configuration does not contain an explicit `encryption_config` block.

This project targets Amazon EKS Kubernetes 1.36. For EKS Kubernetes 1.28
and later, Amazon EKS provides default envelope encryption for Kubernetes
API data, including Kubernetes Secrets, using an AWS-owned key.

A customer-managed AWS KMS key is therefore not provisioned solely to satisfy
the static Trivy rule. This avoids introducing an unnecessary AWS KMS
resource and associated charges for this cost-constrained project.

The EKS cluster is explicitly configured with:

- Kubernetes version 1.36
- Private Kubernetes API endpoint access enabled
- Public Kubernetes API endpoint access disabled

The remaining `AVD-AWS-0039` result is therefore treated as a documented
static-analysis limitation for the modern EKS default encryption model,
rather than as an unaddressed application vulnerability.
