# ${{ values.repositoryName }}

${{ values.description }}

## 🏗️ Arquitetura

- **Sigla**: `${{ values.sigla }}`
- **App Name**: `${{ values.appName }}`
- **GitOps Repo**: `${{ values.gitopsRepo }}`
- **Ambientes**: ${{ values.environments | join(', ') }}
- **Owner**: `${{ values.owner }}`
- **JIRA**: [${{ values.jiraTicket }}](https://devopstia.atlassian.net/browse/${{ values.jiraTicket }})

## 🚀 Tecnologias

- Python 3.12
- FastAPI
- Uvicorn
- Docker

## 📦 Estrutura

```
${{ values.appName }}/
├── main.py              # Aplicação FastAPI
├── requirements.txt     # Dependências Python
├── Dockerfile          # Container image
├── .pipeline.yaml      # Configuração CI/CD
├── catalog-info.yaml   # Backstage catalog
└── .github/
    └── workflows/
        └── pipeline.yml # GitHub Actions
```

## 🔄 GitFlow & GitOps

### Branches
- `develop` → Deploy DEV
- `release` → Deploy HML
- `main` → Deploy PRD

### Fluxo CI/CD
1. **Push** → GitHub Actions (CI)
2. **Build** → Docker image + push GHCR
3. **GitOps** → Commit em `${{ values.gitopsRepo }}`
4. **ArgoCD** → Sync automático no EKS

## 📝 Configuração `.pipeline.yaml`

Ambientes configurados:
{%- for env in values.environments %}
- **{{ env | upper }}**: Account ID definido no GitOps
{%- endfor %}

- `develop` → Ambiente de Desenvolvimento (DEV)
- `release` → Ambiente de Homologação (HML)
- `main` → Ambiente de Produção (PRD)

## 🛠️ Desenvolvimento Local

```bash
# Instalar dependências
pip install -r requirements.txt

# Executar aplicação
python main.py

# Acessar
http://localhost:8000
```

## 🐳 Docker

```bash
# Build
docker build -t ${{ values.appName }}:latest .

# Run
docker run -p 8000:8000 ${{ values.appName }}:latest
```

## 🔗 Links

- [GitHub Repository](https://github.com/${{ values.destination.owner }}/${{ values.destination.repo }})
- [ArgoCD](https://argocd.devopstia.com/applications/${{ values.sigla | lower }}-${{ values.appName }})
- [Backstage](https://backstage.devopstia.com/catalog/default/component/${{ values.appName }})

## 📝 CI/CD

Pipeline automatizado via GitHub Actions:

1. **CI** (Build & Test)
   - Lint código
   - Testes unitários
   - Build Docker image
   - Push para registry

2. **CD** (Deploy)
   - Atualiza manifests GitOps
   - ArgoCD sincroniza automaticamente
   - Deploy no ambiente conforme branch

## 🎯 Ambientes

| Branch   | Ambiente | AWS Account      | Region     |
|----------|----------|------------------|------------|
| develop  | DEV      | 259175803102     | us-east-1  |
| release  | HML      | 493385093101     | us-east-1  |
| main     | PRD      | 924146895830     | sa-east-1  |
