# ${{ values.repositoryName }}

GitOps repository com Helm charts para deploy via ArgoCD.

## 📦 Estrutura

```
helm/
├── Chart.yaml          # Metadata do chart
├── values.yaml         # Valores base (template)
└── templates/          # (criado pelo workflow CI)

environments/
├── values-dev.yaml     # DEV environment
├── values-hml.yaml     # HML environment (se aplicável)
└── values-prd.yaml     # PRD environment (se aplicável)
```

## 🏗️ Informações

- **Sigla**: `${{ values.sigla }}`
- **App**: `${{ values.appName }}`
- **Ambientes**: ${{ values.environments | join(', ') }}
- **Owner**: `${{ values.owner }}`
- **JIRA**: [${{ values.jiraTicket }}](https://devopstia.atlassian.net/browse/${{ values.jiraTicket }})

## 🔄 Fluxo GitOps

1. **CI (APP)**: Workflow da aplicação faz commit aqui após build
2. **ArgoCD**: Sincroniza automaticamente com cluster EKS
3. **Deploy**: Rollout automático por ambiente (dev → hml → prd)

## 📝 Como Atualizar

**Account IDs:**
Edite `environments/values-{env}.yaml`:
```yaml
accountId: "259175803102"  # Seu Account ID real
```

**Recursos:**
Ajuste limites de CPU/Memory conforme necessidade.

## 🔗 Links

- [ArgoCD Dashboard](https://argocd.devopstia.com/applications/${{ values.sigla | lower }}-${{ values.appName }})
- [JIRA Ticket](https://devopstia.atlassian.net/browse/${{ values.jiraTicket }})
