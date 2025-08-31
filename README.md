# Projeto Grafana IAC

Automatização de infraestrutura e instalação do Grafana usando Terraform e Ansible com GitHub Actions.

## Visão Geral

Este projeto automatiza o provisionamento de infraestrutura e a instalação do Grafana usando Terraform e Ansible, com pipelines do GitHub Actions para integração contínua.

## Estrutura do Projeto

IAC/
├── .ansible
├── .vscode
├── lab/
│   ├── .ansible
│   ├── automation/
│   │   ├── group_vars/
│   │   ├── inventory.sh
│   │   └── playbook.yml
│   ├── compute/
│   │   ├── .terraform/
│   │   ├── main.tf
│   │   ├── outputs.tf
│   │   ├── providers.tf
│   │   ├── variables.tf
│   │   ├── terraform.tfstate
│   │   └── terraform.tfstate.backup
│   └── devops-pdi.pem
├── module-f/
│   └── main.tf
└── pipelines/
    └── .github/
        └── workflows/
            └── grafana-pipeline-deploy.yml


## Descrição dos Diretórios

### lab/compute
Contém os arquivos Terraform que provisionam a infraestrutura do Grafana.

### lab/automation
Contém os playbooks Ansible e inventário para configurar a infraestrutura criada pelo Terraform.

### module-f
Módulo pai do Terraform com todas as variáveis definidas, reutilizado pelo módulo filho.

### pipelines
Contém os workflows do GitHub Actions para executar o deploy completo.

## Fluxo do Projeto

1. **Pipeline do GitHub Actions** é acionada:
   - Localização: `pipelines/.github/workflows/grafana-pipeline-deploy.yml`

2. **Terraform** cria a infraestrutura no provedor (ex.: AWS), usando o módulo pai `module-f`

3. **Ansible** provisiona a infraestrutura criada com o Grafana, usando:
   - `inventory.sh` para gerar o inventário dinâmico
   - `playbook.yml` para instalar e configurar o Grafana

## Como Executar

### Pré-requisitos
- Terraform instalado
- Ansible instalado
- Acesso às chaves e variáveis de ambiente necessárias para o provedor (AWS, Azure, etc.)

### Passos
1. Configurar variáveis e secrets no repositório para autenticação do provedor
2. Executar a pipeline no GitHub Actions
3. O Terraform cria a infraestrutura automaticamente
4. Após a infraestrutura estar pronta, o Ansible provisiona o Grafana usando o inventário dinâmico

## Observações

- O módulo pai (`module-f`) contém todos os valores padrão para facilitar reutilização e padronização
- O fluxo é totalmente automatizado via pipeline; não é necessário executar Terraform ou Ansible manualmente
- Os playbooks Ansible podem ser ajustados para adicionar novas configurações no Grafana sem modificar a infraestrutura