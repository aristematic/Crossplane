# Crossplane Demo

Practical demo of Crossplane for managing AWS infrastructure via Kubernetes.

## What this covers
- AWS S3 bucket via Managed Resource
- XRD + Composition + Claim pattern
- Drift detection

## Prerequisites
- kind
- kubectl
- helm
- AWS credentials

## Setup

### 1. Create cluster and install Crossplane
\`\`\`bash
kind create cluster --name crossplane-demo

helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update

helm install crossplane \\
  crossplane-stable/crossplane \\
  --namespace crossplane-system \\
  --create-namespace
\`\`\`

### 2. Configure AWS credentials
\`\`\`bash
kubectl create secret generic aws-secret \\
  -n crossplane-system \\
  --from-file=creds=./aws-credentials.txt
\`\`\`

### 3. Apply everything in order
\`\`\`bash
kubectl apply -f functions/
kubectl apply -f providers/
kubectl apply -f compositions/
kubectl apply -f claims/
\`\`\`

### 4. Apply managed resource directly (optional)
\`\`\`bash
kubectl apply -f managed-resources/
\`\`\`

## Drift Detection Test
\`\`\`bash
# Delete bucket from AWS console, watch Crossplane recreate it
kubectl get bucket --watch
\`\`\`
