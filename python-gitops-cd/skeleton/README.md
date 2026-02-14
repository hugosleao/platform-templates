# ${{ values.repositoryName }}

GitOps repository com Helm charts para deploy via ArgoCD.

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
helm/
├── Chart.yaml          # Metadata do chart
├── values.yaml         # Valores base por plataforma
└── templates/          # (criado pelo workflow CI das apps)

environments/
├── values-dev.yaml     # DEV environment
├── values-hml.yaml     # HML environment (se aplicável)
└── values-prd.yaml     # PRD environment (se aplicável)
```

## 🔄 Fluxo GitOps

1. **CI (APP)**: Workflow da aplicação faz commit aqui após build
2. **ArgoCD**: Sincroniza automaticamente com {%- if values.platform == 'lbd' %}AWS Lambda{% else %}cluster Kubernetes{% endif %}
3. **Deploy**: Rollout automático por ambiente (dev → hml → prd)

## 📝 Como Atualizar

**Account IDs:**
Edite `environments/values-{env}.yaml`:
```yaml
accountId: "259175803102"  # Seu Account ID real
```

**Recursos:**
{%- if values.platform == 'lbd' %}
Ajuste `lambda.memory` e `lambda.timeout` conforme necessidade.
{%- else %}
Ajuste limites de CPU/Memory e `replicas` conforme necessidade.
{%- endif %}

## 🔗 Links

- [ArgoCD Dashboard](https://argocd.devopstia.com/applications/${{ values.sigla | lower }}-gitops-${{ values.platform }})
- [JIRA Ticket](https://devopstia.atlassian.net/browse/${{ values.jiraTicket }})
