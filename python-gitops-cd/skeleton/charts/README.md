# Charts Directory

Este diretório contém os manifestos de deploy das aplicações.

## Estrutura

Cada aplicação que usa este GitOps criará seu próprio arquivo YAML aqui:

```
charts/
├── {sigla}_app_{nome-app1}.yaml
├── {sigla}_app_{nome-app2}.yaml
└── {sigla}_app_{nome-app3}.yaml
```

## Formato do YAML

{%- if values.platform == 'lbd' %}
### Lambda (via Crossplane)

```yaml
apiVersion: serving.lambda/v1
kind: Function
metadata:
  name: {sigla}-{app}-{env}
aws:
  ACCOUNT_ID: 'XXXXX'
  BUCKET_ID: bucket-name
  ENVIRONMENT: DEV|HML|PRD
  HANDLER: lambda_handler.lambda_handler
  REGION: us-east-1
  RUNTIME: python3.12
spec:
  code:
    env:
      - name: LOG_LEVEL
        value: info
    url: https://ghcr.io/hugosleao/{sigla}_app_{nome}:tag
```
{%- else %}
### Kubernetes (Helm)

```yaml
apiVersion: v1
kind: Application
metadata:
  name: {sigla}-{app}-{env}
spec:
  image: ghcr.io/hugosleao/{sigla}_app_{nome}:tag
  replicas: 2
  resources:
    limits:
      cpu: 500m
      memory: 512Mi
```
{%- endif %}

## Workflow

1. **CI (APP)** → Build + push image → **Commit YAML aqui**
2. **Workflow GitOps** → Detecta mudanças → **Deploy automático**
3. **ArgoCD** → Sync contínuo

## Branches

- `develop` → DEV
- `release` → HML
- `master` → PRD

Cada branch tem seus próprios YAMLs com configs específicas do ambiente.
