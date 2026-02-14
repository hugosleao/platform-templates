# DevOpsTiA Platform Templates

Templates oficiais do Backstage para criação de aplicações na plataforma DevOpsTiA.

## 📦 Templates Disponíveis

### Python FastAPI Application
Template completo para criação de APIs REST usando FastAPI com:
- ✅ CI/CD automatizado (GitHub Actions)
- ✅ GitOps com ArgoCD
- ✅ Integração JIRA
- ✅ Backstage Software Catalog
- ✅ Multi-ambiente (DEV/HML/PRD)

**Localização**: `python-api/template.yaml`

## 🚀 Como Usar

1. Acesse o Backstage: https://backstage.devopstia.com
2. Clique em "Create..." no menu lateral
3. Selecione o template desejado
4. Preencha os parâmetros:
   - **Sigla**: Identificador único da aplicação (ex: DVSP)
   - **Nome**: Nome da aplicação (ex: payment-api)
   - **Tipo**: GitOps ou Aplicação de Teste
   - **Owner**: Time ou pessoa responsável
   - **JIRA**: Ticket relacionado (ex: DEVOPS-123)
5. Crie a aplicação

## 🏗️ Estrutura da Arquitetura

```
Backstage (IDP)
    ↓ (cria repo + secrets)
GitHub (Source)
    ↓ (trigger pipeline)
GitHub Actions (CI/CD)
    ↓ (build + push)
GitOps Repository
    ↓ (sync)
ArgoCD (Continuous Delivery)
    ↓ (deploy)
Kubernetes (EKS)
```

## 📋 Requisitos

- Conta GitHub ativa
- Ticket JIRA válido
- Permissões no Backstage

## 🔗 Links Úteis

- [Backstage](https://backstage.devopstia.com)
- [ArgoCD](https://argocd.devopstia.com)
- [JIRA](https://devopstia.atlassian.net)
- [Documentação](https://github.com/hugosleao/platform-templates/wiki)

## 🛠️ Desenvolvimento de Templates

Para criar novos templates, siga a estrutura:

```
nome-template/
├── template.yaml      # Definição do template Backstage
└── skeleton/          # Conteúdo base do projeto
    ├── catalog-info.yaml
    ├── .pipeline.yaml
    └── ...
```

## 📝 Convenções

- **Sigla**: Máximo 6 caracteres maiúsculos
- **Nome**: Apenas letras minúsculas, números e hífens
- **Branches**: 
  - `develop` → DEV
  - `release` → HML
  - `main` → PRD

## 🤝 Contribuindo

1. Fork este repositório
2. Crie uma branch feature
3. Faça suas alterações
4. Envie um Pull Request

## 📄 Licença

Propriedade da DevOpsTiA - Uso interno apenas.
