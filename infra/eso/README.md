# External Secrets Operator Configuration

## ClusterSecretStore Selection

Choose the appropriate ClusterSecretStore based on your cluster type:

### For K3s/Local clusters using static AWS credentials:
Use: `clustersecretstore-k3s.yaml`

This requires:
1. Creating AWS credentials secret manually
2. Updating the access-key-id and secret-access-key values

### For EKS clusters using IRSA (recommended):
Use: `clustersecretstore-eks-irsa.yaml`

This requires:
1. IAM role with appropriate permissions for Secrets Manager
2. ServiceAccount annotation with the role ARN
3. Update the external-secrets-app.yaml to include the ServiceAccount annotation

## Setup Instructions

1. Choose the appropriate ClusterSecretStore file
2. Apply the ClusterSecretStore to your cluster
3. Ensure the External Secrets Operator is installed and running
4. Verify the ClusterSecretStore is ready: `kubectl get clustersecretstore aws-secrets`
