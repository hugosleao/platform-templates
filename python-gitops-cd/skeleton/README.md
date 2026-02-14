# ${{ values.repositoryName }}

GitOps repository para deploy automático via ArgoCD/Crossplane.

## 🏗️ Informações

- **Sigla**: `${{ values.sigla }}`
- **Plataforma**: `${{ values.platform }}`
{%- if values.platform == 'lbd' %} (AWS Lambda)
{%- elif values.platform == 'eks' %} (AWS EKS)
{%- else %} (Azure AKS)
{%- endif %}
- **Ambientes**: ${{ values.environments | join(', ') }}
- **Owner**: `${{ values.owner }}`
- **JIRA**: [${{ values.jiraTicket }}](https://devopstia.atlassian.net/browse/${{ values.jiraTicket }})

## 📦 Estrutura

```
${{ values.repositoryName }}/
├── charts/                              # Manifestos de deploy
│   ├── values-{sigla}-{app1}.yaml      # APP 1
│   ├── values-{sigla}-{app2}.yaml      # APP 2
│   └── README.md
├── .github/
│   └── workflows/
│       └── deploy.yml                   # Deploy automático
└── README.md
```

## 🔄 Fluxo GitOps

### 1. Aplicação faz commit aqui

```bash
# Workflow CI da aplicação
git clone ${{ values.repositoryName }}
git checkout develop  # ou release/master
echo "manifest YAML" > charts/values-{sigla}-{nome}.yaml
git commit -m "deploy: {app} version {tag}"
git push
```

### 2. Workflow detecta mudança

- Monitora `charts/**/*.yaml`
- Detecta arquivos modificados
- Deploy automático por arquivo

### 3. Deploy

{%- if values.platform == 'lbd' %}
- Crossplane cria/atualiza Lambda
- Configuração via YAML (memory, timeout, env vars)
{%- else %}
- Kubectl apply no cluster Kubernetes
- ArgoCD sincroniza continuamente
{%- endif %}

## 🌳 Branches e Ambientes

Cada branch representa um ambiente:

| Branch   | Ambiente | Account ID (exemplo)      |
|----------|----------|---------------------------|
| develop  | DEV      | ${{ values.environments | select('equalto', 'dev') | list | length > 0 ? '259175803102' : 'N/A' }} |
| release  | HML      | ${{ values.environments | select('equalto', 'hml') | list | length > 0 ? '493385093101' : 'N/A' }} |
| master   | PRD      | ${{ values.environments | select('equalto', 'prd') | list | length > 0 ? '924146895830' : 'N/A' }} |

**Importante**: Cada branch tem seus próprios YAMLs. Não fazer merge entre branches!

## 📝 Como Adicionar Nova Aplicação

1. **Criar APP via Backstage** (template `Python FastAPI Application`)
2. **Referenciar este repo** no campo GitOps
3. **CI da APP** commitará YAMLs automaticamente aqui

## 🔗 Links

- [GitHub](https://github.com/${{ values.destination.owner }}/${{ values.repositoryName }})
{%- if values.platform != 'lbd' %}
- [ArgoCD Dashboard](https://argocd.devopstia.com/applications/${{ values.sigla | lower }}-gitops-${{ values.platform }})
{%- endif %}
- [JIRA Ticket](https://devopstia.atlassian.net/browse/${{ values.jiraTicket }})
