# Proposta de Arquitetura para o Website da Empresa (Foco em Baixo Custo)

---

## 1. Arquitetura da Nova Solução

A proposta é construir uma arquitetura escalável, segura e de baixo custo, utilizando os melhores serviços AWS, com foco em otimização de custos e proximidade à região de São Paulo (sa-east-1).

### Componentes Principais:

1. **Ambiente Kubernetes (Amazon EKS com instâncias EC2 Spot)**:
   - Utilização do **Amazon EKS** para orquestrar os contêineres Docker.
   - Uso de **instâncias EC2 Spot** para reduzir custos, principalmente para nós de worker no Kubernetes.

2. **Banco de Dados (Amazon RDS - MySQL ou PostgreSQL)**:
   - **Amazon RDS** para bancos de dados MySQL ou PostgreSQL, com instâncias menores (\`db.t3.micro\` ou \`db.t3.small\`) para reduzir custos.
   - **Backup diário** e **snapshots automatizados** para garantir a segurança dos dados.

3. **Plataforma como Serviço (AWS Lightsail)**:
   - **AWS Lightsail** para a implantação da aplicação de forma econômica, com instâncias simples e custos previsíveis.

4. **Backup e Persistência de Dados**:
   - **Amazon S3** para armazenamento de backups e arquivos estáticos.

5. **Balanceamento de Carga**:
   - **Elastic Load Balancer (ALB)** para balanceamento de carga de forma econômica.

6. **Segurança**:
   - **AWS IAM**, **VPC**, **Security Groups**, e **AWS WAF** para controlar o acesso e proteger a aplicação.

7. **CI/CD e DevOps**:
   - **AWS CodePipeline** e **CodeBuild** para automação do processo de CI/CD.

---

## 2. Prazo de Entrega

O prazo estimado para a construção da solução é de **2 meses** com o seguinte cronograma:

| **Fase** | **Atividade**                       | **Duração**  | **Entrega Esperada**  |
|----------|--------------------------------------|--------------|-----------------------|
| Fase 1   | Planejamento e Design                | 1 semana     | Mês 1, Semana 1       |
| Fase 2   | Implementação da Infraestrutura      | 3 semanas    | Mês 1, Semana 2-4     |
| Fase 3   | Implementação de Backup e Persistência | 1 semana    | Mês 2, Semana 1       |
| Fase 4   | Implementação de Balanceamento de Carga e Segurança | 2 semanas | Mês 2, Semana 2-3 |
| Fase 5   | Implementação de CI/CD e Testes      | 2 semanas    | Mês 2, Semana 4      |
| Fase 6   | Testes de Performance e Segurança    | 1 semana     | Mês 2, Semana 4      |

---

## 3. Cronograma Macro de Entregas

| **Fase** | **Atividade**                       | **Duração**  | **Entrega Esperada**  |
|----------|--------------------------------------|--------------|-----------------------|
| Fase 1   | Planejamento e Design                | 1 semana     | Mês 1, Semana 1       |
| Fase 2   | Implementação da Infraestrutura      | 3 semanas    | Mês 1, Semana 2-4     |
| Fase 3   | Implementação de Backup e Persistência | 1 semana    | Mês 2, Semana 1       |
| Fase 4   | Implementação de Balanceamento de Carga e Segurança | 2 semanas | Mês 2, Semana 2-3 |
| Fase 5   | Implementação de CI/CD e Testes      | 2 semanas    | Mês 2, Semana 4      |
| Fase 6   | Testes de Performance e Segurança    | 1 semana     | Mês 2, Semana 4      |

---

## 4. Valores Estimados

### Serviços AWS e Estimativas de Custo:

1. **Amazon EKS (usando instâncias EC2 Spot)**:
   - Instâncias EC2 Spot (\`t3.medium\`): **US$ 0,014/hora**.
   - Custo mensal de instâncias EC2 Spot (considerando 730 horas/mês): **US$ 10,22/mês**.

2. **Amazon RDS (MySQL/PostgreSQL)**:
   - Instância \`db.t3.micro\`: **US$ 0,017/hora**.
   - Custo mensal de instância RDS: **US$ 12,41/mês**.
   - Armazenamento (100GB): **US$ 10,00/mês**.
   - Backup e snapshots: **US$ 9,50/mês** (com backup de 100GB).

3. **AWS Lightsail**:
   - Instância Lightsail (1 vCPU, 1GB RAM): **US$ 3,50/mês**.
   - Armazenamento adicional em S3 (100GB): **US$ 2,30/mês**.

4. **Elastic Load Balancer (ALB)**:
   - Custo fixo: **US$ 0,0225/hora**.
   - Custo mensal de ALB: **US$ 16,50/mês** (considerando 730 horas/mês).
   - Custo adicional por GB de tráfego: **US$ 0,008/GB**.

5. **Amazon S3**:
   - Armazenamento: **US$ 0,023/GB/mês**.
   - Para 100GB de backup: **US$ 2,30/mês**.

6. **AWS WAF**:
   - Custo básico: **US$ 5/mês**.
   - Custos adicionais por regras e solicitações: **US$ 0,60/milhão de solicitações**.

7. **Monitoramento (CloudWatch)**:
   - Custo de logs: **US$ 0,30/GB**.
   - Monitoramento básico de instâncias EC2: **US$ 3/instância/mês**.

8. **CI/CD (AWS CodePipeline e CodeBuild)**:
   - CodePipeline: **US$ 1/pipeline/mês**.
   - CodeBuild: **US$ 0,0035/minuto de compilação**.

---

### Resumo dos Custos Mensais Estimados:

| **Serviço**                          | **Custo Estimado**   |
|--------------------------------------|----------------------|
| **Amazon EKS (instâncias EC2 Spot)** | **US$ 10,22/mês**     |
| **Amazon RDS (db.t3.micro)**         | **US$ 22,91/mês**     |
| **AWS Lightsail (instância + S3)**   | **US$ 5,80/mês**      |
| **Elastic Load Balancer (ALB)**      | **US$ 16,50/mês**     |
| **Amazon S3 (backup)**               | **US$ 2,30/mês**      |
| **AWS WAF**                          | **US$ 5,00/mês**      |
| **Monitoramento (CloudWatch)**       | **US$ 3,30/mês**      |
| **CI/CD (CodePipeline + CodeBuild)** | **US$ 2,50/mês**      |

**Total Estimado de Custos Mensais**: **US$ 58,53/mês**

---

## Conclusão

A solução apresentada oferece uma arquitetura otimizada para custos, utilizando serviços da AWS mais econômicos como **EC2 Spot** e **AWS Lightsail**, mantendo a escalabilidade, segurança e confiabilidade necessárias. O custo mensal estimado para a solução é de **US$ 58,53/mês**, com potencial para ajustar conforme o uso real.
