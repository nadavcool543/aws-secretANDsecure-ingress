# AWS External Secrets Helm Chart

This Helm chart deploys the necessary components to manage AWS Secrets Manager integration using External Secrets Operator.

## Prerequisites

- Kubernetes cluster
- Helm v3+
- External Secrets Operator installed in the cluster
- AWS credentials with access to Secrets Manager

## Installation

### For Production

```bash
# Install the chart in production mode
helm install aws-secrets . -f values.yaml -f values-prod.yaml --namespace prod --create-namespace
```

### For Development

```bash
# Install the chart in development mode
helm install aws-secrets . -f values.yaml -f values-dev.yaml --namespace dev --create-namespace
```

## Configuration

### AWS Credentials

The AWS credentials should be managed securely. You can override them using:

- Helm values override files
- CI/CD secrets
- Secret management tools

Never commit actual AWS credentials to version control.

### Important Values

| Parameter | Description | Default |
|-----------|-------------|---------|
| global.environment | Environment (prod/dev) | prod |
| aws.region | AWS region | us-east-1 |
| externalSecrets.refreshInterval | Secret refresh interval | 1h |
| appConfig.secretPath.prod | Production secrets path | prod/app-config |
| appConfig.secretPath.dev | Development secrets path | dev/app-config |

### Adding New Secrets

To add new secrets, update the `appConfig.keys` section in `values.yaml`:

```yaml
appConfig:
  keys:
    - name: new_secret_key
```

## Uninstallation

```bash
# For production
helm uninstall aws-secrets -n prod

# For development
helm uninstall aws-secrets -n dev
```

## Security Considerations

1. Always use separate AWS credentials for production and development
2. Limit IAM permissions to only the required secrets
3. Use different secret paths for different environments
4. Regularly rotate AWS credentials
5. Monitor secret access through AWS CloudTrail
