# Secure Ingress Helm Chart

This Helm chart deploys a secure ingress configuration with cert-manager integration for TLS certificate management.

## Prerequisites

- Kubernetes cluster
- Helm v3+
- nginx ingress controller
- cert-manager already installed in the cluster

## Features

- Integration with existing cert-manager installation
- Automatic TLS certificate management
- Self-signed certificate issuer
- Nginx service and ingress configuration

## Installation

### 1. Verify cert-manager installation

```bash
# Verify cert-manager is installed and running
kubectl get pods -n cert-manager
```

### 2. Install the Chart

```bash
# Create namespace if it doesn't exist
kubectl create namespace test-cert

# Update dependencies
helm dependency update

# Install the chart
helm install secure-ingress . --namespace test-cert
```

## Configuration

### Global Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| namespace | Kubernetes namespace | test-cert |

### Service Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| service.name | Name of the service | nginx-service |
| service.port | Service port | 80 |
| service.targetPort | Target port | 80 |
| service.selector.app | Service selector | nginx |

### Ingress Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| ingress.name | Name of the ingress | nginx-ingress |
| ingress.annotations | Ingress annotations | cert-manager.io/cluster-issuer: "selfsigned-issuer" |
| ingress.host | Hostname | test.local |
| ingress.tls.secretName | TLS secret name | test-tls |

### Cert-Manager Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| cert-manager.enabled | Enable cert-manager integration | true |
| cert-manager.installCRDs | Install cert-manager CRDs | false |
| clusterIssuer.create | Create a self-signed ClusterIssuer | true |
| clusterIssuer.name | Name of the ClusterIssuer | selfsigned-issuer |

## Customization

You can override values by creating your own values file:

```yaml
# custom-values.yaml
ingress:
  host: my-custom-domain.com
  tls:
    secretName: my-custom-tls

cert-manager:
  enabled: true
```

Then install with:

```bash
helm install secure-ingress . -f custom-values.yaml --namespace test-cert
```

## Architecture

The chart deploys the following components:
1. Integration with existing cert-manager installation
2. ClusterIssuer for self-signed certificates
3. Nginx service
4. Secure ingress with TLS

## Uninstallation

```bash
helm uninstall secure-ingress -n test-cert
```

## Notes

1. This chart assumes cert-manager is already installed in your cluster
2. A self-signed ClusterIssuer is created by default
3. Update your local hosts file or DNS to point the hostname to your cluster
4. The service assumes an nginx deployment with label app: nginx exists

## Troubleshooting

1. Verify cert-manager integration:
```bash
kubectl get pods -n cert-manager
```

2. Check certificate status:
```bash
kubectl get certificate -n test-cert
```

3. Check ClusterIssuer status:
```bash
kubectl get clusterissuer
kubectl describe clusterissuer selfsigned-issuer
```

4. Check ingress status:
```bash
kubectl get ingress -n test-cert
kubectl describe ingress nginx-ingress -n test-cert
```

5. Common Issues:
   - If certificates are not being issued, check the cert-manager logs
   - If ingress is not working, verify the nginx ingress controller is running
   - If TLS is not working, check the certificate and secret status
