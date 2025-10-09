# 🧭 Workspace DevOps Automation

Automação de infraestrutura em nuvem utilizando **Terraform**, **Ansible** e **GitHub Actions** para provisionamento e configuração de recursos em ambientes Azure.

Este projeto foi desenvolvido com foco em **práticas modernas de DevOps**, integrando infraestrutura como código (IaC), pipelines CI/CD e padronização de ambientes de desenvolvimento.

---

## 🏗️ Arquitetura Geral

O fluxo completo de automação é composto por três camadas principais:

1. **Infraestrutura (Terraform)**  
   - Criação de Resource Groups, VMs, redes virtuais e IPs públicos.  
   - Integração com Service Principal da Azure.  
   - Aplicação de boas práticas de versionamento e modularização.

2. **Configuração (Ansible)**  
   - Aplicação de playbooks para instalação e configuração de pacotes.  
   - Gerenciamento de usuários, permissões e serviços em VMs provisionadas.

3. **Pipeline (GitHub Actions)**  
   - Execução automática de lint, validação e plano Terraform.  
   - Integração segura com secrets da Azure.  
   - Validação contínua de sintaxe e formatação de código.

---

## 📂 Estrutura do Repositório

```
.
├── .github/
│   └── workflows/
│       ├── terraform-validate.yml      # Pipeline CI Terraform
│       └── ansible-deploy.yml          # Pipeline CD Ansible (exemplo)
│
├── infra/
│   ├── main.tf                         # Definição principal da infraestrutura
│   ├── variables.tf                    # Declaração de variáveis
│   ├── outputs.tf                      # Saídas exportadas do Terraform
│   ├── providers.tf                    # Configuração do provider AzureRM
│   └── backend.tf                      # (opcional) Configuração de backend remoto
│
├── ansible/
│   ├── playbooks/                      # Scripts de automação Ansible
│   └── inventory/                      # Arquivos de inventário
│
└── README.md
```

---

## ⚙️ Configuração Inicial

Antes de executar o pipeline, é necessário configurar **secrets no GitHub** (menu:  
`Settings → Secrets and variables → Actions`):

| Nome do Secret | Descrição | Tipo |
|----------------|------------|------|
| `ARM_CLIENT_ID` | ID da aplicação registrada no Azure AD | Obrigatório |
| `ARM_CLIENT_SECRET` | Chave secreta do Service Principal | Obrigatório |
| `ARM_SUBSCRIPTION_ID` | Identificador da assinatura Azure | Obrigatório |
| `ARM_TENANT_ID` | ID do diretório (tenant) | Obrigatório |

> 🔒 Nenhum desses valores é versionado. Todos são gerenciados via **GitHub Secrets**, garantindo que dados sensíveis não fiquem expostos no código-fonte.

---

## 🚀 Execução Local (Terraform)

```bash
cd infra

# Inicializa o Terraform e baixa os providers necessários
terraform init

# Valida a formatação e sintaxe
terraform fmt -check
terraform validate

# Gera o plano de execução
terraform plan -out=tfplan

# Aplica o plano
terraform apply tfplan
```

Para destruir os recursos criados:
```bash
terraform destroy
```

---

## 🔁 Fluxo de CI/CD (GitHub Actions)

Cada _push_ ou _pull request_ aciona automaticamente a esteira `terraform-validate.yml`, que executa:

1. `terraform init`
2. `terraform fmt -check`
3. `terraform validate`
4. `terraform plan -out=tfplan`

> ⚙️ A execução é feita em runners Linux, com as variáveis de ambiente exportadas a partir dos secrets configurados.

---

## 🧱 Recursos Provisionados (exemplo)

- Resource Group principal (`rg-vm-automation`)  
- Rede virtual e sub-rede associada  
- IP público SKU Standard  
- Interface de rede vinculada à VM  
- Máquina virtual Linux provisionada via Terraform  
- Configuração pós-provisionamento via Ansible  

---

## 🧩 Boas Práticas Adotadas

- IaC com versionamento e validação automática  
- Nenhum dado sensível versionado em repositório  
- Uso de **SKU Standard** para compatibilidade entre regiões  
- Modularização e padronização dos scripts  
- Documentação centralizada neste README  

---

## 🧠 Troubleshooting Comum

| Erro | Causa provável | Solução |
|------|----------------|----------|
| `IPv4BasicSkuPublicIpCountLimitReached` | Limite de IPs públicos Basic atingido | Use `sku = "Standard"` ou mude de região |
| `UnauthorizedClient` | Credenciais incorretas no Service Principal | Verifique `ARM_CLIENT_ID` e `ARM_CLIENT_SECRET` |
| `Provider not registered` | Provider Azure não registrado | Execute `az provider register --namespace Microsoft.Network` |
| `EOF while looking for matching backtick` | Secret com caractere inválido | Remova crases, aspas e quebras de linha dos secrets |

---

## 🤝 Contribuições

Contribuições são bem-vindas!  
Abra uma **issue** ou envie um **pull request** com melhorias e sugestões.

---

## 📜 Licença

Este projeto é distribuído sob a licença **MIT**.  
Sinta-se livre para utilizar e adaptar conforme suas necessidades.

---

## 🧩 Autor

**Workspace DevOps Automation**  
Desenvolvido para fins de estudo, automação e padronização de práticas de DevOps.

---
