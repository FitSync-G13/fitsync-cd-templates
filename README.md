# FitSync CD Templates

Reusable GitHub Actions workflow templates for FitSync continuous deployment.

## Available Templates

### K3s Installation (`k3s-install.yml`)
Installs and configures K3s cluster on AWS EC2 instances.

**Inputs:**
- `cluster_name`: K3s cluster name
- `k3s_version`: K3s version (default: latest)
- `master_instances`: Comma-separated master instance IDs
- `worker_instances`: Comma-separated worker instance IDs
- `k3s_api_dns`: K3s API load balancer DNS name
- `aws_region`: AWS region

**Secrets:**
- `AWS_ROLE_ARN`: AWS IAM role for OIDC authentication

**Features:**
- Installs K3s on first master with cluster-init
- Joins additional masters to cluster
- Joins all workers to cluster
- Disables Traefik and ServiceLB (for custom CNI)
- Verifies cluster status

### Coming Soon
- `cilium-install.yml` - Cilium CNI installation
- `istio-install.yml` - Istio service mesh installation
- `app-deploy.yml` - Application deployment templates

## Usage

Call templates from your main repository:

```yaml
jobs:
  install-k3s:
    uses: FitSync-G13/fitsync-cd-templates/.github/workflows/k3s-install.yml@main
    with:
      cluster_name: fitsync-cluster
      master_instances: "i-1234567890abcdef0,i-0987654321fedcba0"
      worker_instances: "i-abcdef1234567890,i-fedcba0987654321"
      k3s_api_dns: "fitsync-k3s-api-nlb-1234567890.us-east-2.elb.amazonaws.com"
      aws_region: "us-east-2"
    secrets:
      AWS_ROLE_ARN: ${{ secrets.AWS_ROLE_ARN }}
```
