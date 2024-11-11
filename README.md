# Proposta de Arquitetura para o Website da Empresa (Foco em Baixo Custo)

---

## 1. Arquitetura da Nova Solução

A proposta é construir uma arquitetura escalável, segura e de baixo custo, utilizando os melhores serviços AWS, com foco em otimização de custos e proximidade à região de São Paulo (sa-east-1).

### Componentes Principais:

1. **Ambiente Kubernetes (Amazon EKS com instâncias EC2 Spot)**
   
   **Opções:**
   
   - **Amazon EKS (usando instâncias EC2 Spot)**
     - **Prós**:
       - Orquestração de containers simplificada.
       - Alta escalabilidade.
       - Custos reduzidos ao utilizar instâncias EC2 Spot.
     - **Contras**:
       - Instâncias Spot podem ser interrompidas sem aviso, o que exige configuração de tolerância a falhas.
       - Requer conhecimento técnico para configurar e otimizar.
   
   - **Amazon ECS (Elastic Container Service) com Fargate**
     - **Prós**:
       - Gerenciamento mais simples, sem necessidade de gerenciar infraestrutura de instâncias EC2.
       - Pagamento por uso, sem necessidade de provisionamento de recursos.
     - **Contras**:
       - Custo maior para cargas de trabalho intensivas.
       - Menos controle sobre a infraestrutura comparado ao EKS.
   
   - **Self-hosted Kubernetes em EC2 (sem EKS)**
     - **Prós**:
       - Total controle sobre a infraestrutura.
       - Flexibilidade para otimização de custos com instâncias EC2 Spot e On-Demand.
     - **Contras**:
       - Requer mais esforço e conhecimento para a configuração e manutenção do cluster.
       - Não tem os benefícios do gerenciamento automático do EKS.
   
2. **Banco de Dados (Amazon RDS - MySQL ou PostgreSQL)**
   
   **Opções:**
   
   - **Amazon RDS com MySQL/PostgreSQL**
     - **Prós**:
       - Serviço gerenciado, com backups automáticos, escalabilidade e alta disponibilidade.
       - Facilidade de migração e manutenção.
     - **Contras**:
       - Custo maior em comparação com soluções autogerenciadas.
       - Preço pode aumentar com instâncias de alta capacidade ou armazenamento.
   
   - **Amazon Aurora (compatível com MySQL/PostgreSQL)**
     - **Prós**:
       - Desempenho melhor que o RDS tradicional.
       - Alta disponibilidade e escalabilidade automática.
     - **Contras**:
       - Custo mais elevado em relação ao RDS MySQL/PostgreSQL.
       - Necessidade de um banco de dados mais complexo e recursos maiores.
   
   - **Banco de Dados Self-hosted em EC2**
     - **Prós**:
       - Total controle sobre configuração e custos.
       - Pode ser mais barato se for bem gerenciado.
     - **Contras**:
       - Maior responsabilidade pela manutenção e backups.
       - Pode ser difícil escalar e ajustar sem interrupções de serviço.

3. **Plataforma como Serviço (AWS Lightsail)**
   
   **Opções:**
   
   - **AWS Lightsail**
     - **Prós**:
       - Simples de usar, instâncias com configuração fácil e preço fixo.
       - Ideal para aplicativos de pequena a média escala.
     - **Contras**:
       - Menos flexível e escalável comparado com EC2 ou EKS.
       - Recursos limitados para cargas de trabalho mais intensivas.
   
   - **AWS EC2 com Auto Scaling**
     - **Prós**:
       - Flexível e altamente escalável.
       - Total controle sobre a infraestrutura e os recursos.
     - **Contras**:
       - Necessidade de configuração mais complexa.
       - Custo imprevisível, dependendo da utilização.
   
   - **AWS Lambda (serverless)**
     - **Prós**:
       - Pagamento por execução, ideal para cargas intermitentes e pequenas.
       - Não há necessidade de gerenciar servidores.
     - **Contras**:
       - Não adequado para todos os tipos de aplicativos (ex: aplicativos com estado ou de longo tempo de execução).
       - Limitação de tempo de execução para cada função.

4. **Backup e Persistência de Dados**
   
   **Opções:**
   
   - **Amazon S3**
     - **Prós**:
       - Armazenamento barato e altamente durável.
       - Fácil de configurar e gerenciar.
     - **Contras**:
       - Acessar dados frequentemente pode gerar custos adicionais.
       - Sem suporte nativo a bancos de dados em tempo real.
   
   - **Amazon Glacier (para backups frios)**
     - **Prós**:
       - Custo muito baixo para armazenamento de longo prazo.
       - Ideal para arquivamento de dados.
     - **Contras**:
       - Acesso mais lento aos dados, não ideal para backups frequentes.
       - A cobrança de recuperação de dados pode ser alta.
   
   - **Amazon EFS (Elastic File System)**
     - **Prós**:
       - Armazenamento de arquivos simples, escalável e acessível.
       - Ideal para casos de uso com necessidade de acesso constante aos dados.
     - **Contras**:
       - Mais caro que o S3 para armazenamento de grandes volumes de dados.
       - Menos adequado para backup frio.

5. **Balanceamento de Carga**

   **Opções:**
   
   - **Elastic Load Balancer (ALB)**
     - **Prós**:
       - Fácil de configurar e integrar com instâncias EC2 e ECS.
       - Suporta balanceamento de carga com SSL/TLS e outras features avançadas.
     - **Contras**:
       - Custo fixo mensal, pode ser mais caro para tráfego intenso.
   
   - **Network Load Balancer (NLB)**
     - **Prós**:
       - Ideal para alto desempenho e tráfego com latência mínima.
       - Pode lidar com tráfego TCP/UDP.
     - **Contras**:
       - Menos flexível que o ALB para tráfego HTTP/S.
       - Custo mais elevado dependendo do uso.
   
   - **AWS Lightsail Load Balancer**
     - **Prós**:
       - Preço fixo e simples de configurar.
       - Ideal para cargas menores e tráfego previsível.
     - **Contras**:
       - Funcionalidade limitada em comparação com ALB e NLB.
       - Menos escalável para cargas grandes.

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
