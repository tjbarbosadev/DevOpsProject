# Proposta de Arquitetura AWS para Website Empresarial

Esta proposta detalha a arquitetura e os custos estimados para a criação de um ambiente AWS otimizado para o novo website da empresa. O ambiente é projetado para maximizar a segurança, escalabilidade e disponibilidade com foco em práticas DevOps e custo reduzido.

## Escopo da Proposta

O ambiente contará com os seguintes recursos:
- Cluster Kubernetes (EKS) com auto-scaling
- Banco de Dados Amazon RDS com backups diários
- Armazenamento de dados com Amazon S3 e Amazon EFS
- Balanceamento de carga com Application Load Balancer (ALB)
- Pipeline de CI/CD com AWS CodePipeline e CodeBuild
- Monitoramento de logs e métricas com AWS CloudWatch

## 1. Arquitetura da Nova Solução

1. **Cluster Kubernetes (EKS)**
   - Amazon EKS será utilizado para gerenciar o cluster Kubernetes, com instâncias de nós menores e configuração de auto-scaling.
   - Utilização de três instâncias \`t3.small\` para os nós de trabalho, configuradas para escalar conforme a demanda.

2. **Banco de Dados (Amazon RDS)**
   - Amazon RDS com instância \`db.t3.micro\`, configurada em uma única zona de disponibilidade, com backups diários no Amazon S3.
   - Backups armazenados no S3 com um volume médio de 10 GB.

3. **Armazenamento e Backup**
   - Amazon S3 para armazenamento de arquivos estáticos e backups, com volume estimado em 10 GB.
   - Amazon EFS para armazenamento compartilhado no cluster EKS, configurado para um volume reduzido.

4. **Balanceamento de Carga (ALB)**
   - Application Load Balancer para distribuir o tráfego de entrada e verificar a saúde das instâncias.

5. **CI/CD (AWS CodePipeline e CodeBuild)**
   - CodePipeline e CodeBuild configurados para automação de builds e deploys, executados apenas nos commits principais para reduzir o uso.

6. **Monitoramento e Segurança**
   - AWS CloudWatch com monitoramento essencial e logs limitados a 5 GB/mês.
   - AWS IAM para controle de acesso e permissões.

## 2. Prazo de Entrega

**Estimativa de entrega**: **8 semanas**

1. Planejamento e Configuração Inicial: 2 semanas
2. Implementação do Cluster EKS e Banco de Dados: 2 semanas
3. Configuração de segurança, backups e balanceamento de carga: 2 semanas
4. Implementação de CI/CD e Testes: 1 semana
5. Revisão Final e Entrega: 1 semana

## 3. Cronograma Macro de Entregas

| Fase                        | Descrição                                  | Duração Estimada |
|-----------------------------|--------------------------------------------|------------------|
| Planejamento e Requisitos   | Definição dos detalhes e escopo do projeto | 2 semanas       |
| Infraestrutura e Cluster    | Configuração do ambiente AWS e Kubernetes  | 2 semanas       |
| Banco de Dados e Segurança  | Implementação de RDS, backup e segurança   | 2 semanas       |
| CI/CD e Testes              | Configuração do pipeline e testes finais   | 2 semanas       |

## 4. Valores Estimados Mensalmente

| Serviço                           | Valor Estimado Mensal |
|-----------------------------------|------------------------|
| Amazon EKS (Cluster + Nós)        | $195                  |
| Banco de Dados (Amazon RDS)       | $15,23                |
| Armazenamento (S3 + EFS)          | $1,73                 |
| Application Load Balancer         | $23                   |
| CI/CD (CodePipeline/CodeBuild)    | $10                   |
| Monitoramento (CloudWatch)        | $5                    |
| **Total Estimado**                | **$249,96/mês**       |

Esta configuração oferece uma arquitetura AWS econômica, garantindo segurança, escalabilidade e controle de acesso. O total estimado é de **$249,96 mensais**.
