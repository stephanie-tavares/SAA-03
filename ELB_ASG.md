# Conceitos Fundamentais de Escalabilidade
Escalabilidade refere-se à capacidade de um sistema aumentar ou diminuir seus recursos de forma eficiente, conforme necessário, para lidar com o aumento ou diminuição da demanda. Existem dois tipos principais de escalabilidade:

# Escalabilidade Vertical (Vertical Scaling):
Envolve aumentar o tamanho dos recursos existentes.

Um exemplo é aumentar o tamanho de uma instância EC2, de um tipo menor para um tipo maior (por exemplo, de t3.micro para m5.2xlarge).

Limitação: Há um limite para o quanto um recurso pode ser ampliado verticalmente.

Quando usar? Quando uma única máquina precisa ser mais poderosa para lidar com a carga.

# Escalabilidade Horizontal (Horizontal Scaling):
Envolve adicionar mais recursos para distribuir a carga.
Um exemplo é adicionar mais instâncias EC2 ou servidores ao seu sistema, distribuindo o tráfego entre eles.
Auto Scaling Groups (ASG) é o recurso AWS que lida com isso, ajustando automaticamente o número de instâncias EC2 com base na demanda.

Quando usar? Quando você precisa de um sistema que distribui a carga em várias máquinas para lidar com grandes volumes de tráfego ou processamento.

# Conceitos de Alta Disponibilidade (High Availability)
Alta disponibilidade (HA) refere-se à capacidade de uma aplicação ou sistema continuar funcionando mesmo que uma parte do sistema falhe. A AWS oferece vários serviços que permitem que você crie aplicações tolerantes a falhas.

Zonas de Disponibilidade (Availability Zones - AZs):
Cada região AWS contém várias AZs, que são locais físicos distintos. As AZs são projetadas para serem isoladas uma da outra, para que falhas em uma não afetem as outras.
Projetar sistemas de alta disponibilidade significa distribuir seus recursos em várias AZs para evitar um ponto único de falha (single point of failure).

Regiões AWS:
Para garantir maior resiliência, você pode replicar seus serviços entre regiões, mas é mais caro e geralmente usado para cenários de Disaster Recovery (DR).

# Auto Scaling & Elasticity
O Auto Scaling é um serviço AWS que ajusta automaticamente a capacidade de seus recursos para manter o desempenho ideal com o menor custo possível.

# Auto Scaling Group (ASG):
Os ASGs garantem que um número mínimo de instâncias EC2 esteja sempre em execução e, quando a demanda aumenta, novas instâncias são lançadas para lidar com a carga.

Você pode definir métricas para Auto Scaling, como CPU Utilization, Network In/Out, ou usar métricas customizadas via CloudWatch.
Elasticidade refere-se à capacidade de aumentar ou diminuir recursos automaticamente, conforme necessário, o que é habilitado por ASGs.

# Load Balancers e Elastic Load Balancing (ELB):
O Elastic Load Balancer (ELB) distribui automaticamente o tráfego de entrada entre várias instâncias, aumentando a disponibilidade e a escalabilidade de suas aplicações.

Existem três tipos de load balancers:

Application Load Balancer (ALB): Operacional na camada 7 (HTTP/HTTPS). Ideal para aplicações baseadas em HTTP, pois permite roteamento inteligente baseado em conteúdo (ex: URL).

Network Load Balancer (NLB): Operacional na camada 4 (TCP). Usado para cenários que exigem performance ultra-rápida e baixa latência.

Gateway Load Balancer (GWLB): Usado para balanceamento de tráfego de rede entre firewalls ou appliances de terceiros.

# Design Patterns para Alta Disponibilidade e Resiliência
Para atingir alta disponibilidade e tolerância a falhas, existem vários padrões de arquitetura que você deve considerar:

# Multi-AZ Deployments:
Implantar suas instâncias EC2, bancos de dados RDS, e outros serviços críticos em múltiplas AZs para garantir que, se uma AZ falhar, a aplicação continue funcionando.
Para Amazon RDS, o modo Multi-AZ replica automaticamente os dados para uma instância standby em outra AZ.

# Stateless Applications:
Aplicações stateless (sem estado) não armazenam dados na própria instância de aplicação. Isso permite que você escale horizontalmente com facilidade.
Para gerenciar estado de forma centralizada, você pode usar serviços como Elasticache (para cache) ou DynamoDB (para sessões de usuários).

# Disaster Recovery (DR):
Um plano de recuperação de desastres é crucial para garantir que você possa restaurar seus serviços em caso de falhas graves, como falhas em uma região inteira.
Existem diferentes estratégias de DR:

Backup and Restore: Faça backups regulares de dados críticos e restaure quando necessário.

Pilot Light: Mantenha um ambiente de DR minimamente funcional que pode ser ativado rapidamente em caso de falha.

Warm Standby: Mantenha um ambiente menor em funcionamento contínuo, que pode ser expandido rapidamente.

Multi-Site Active-Active: Execute cargas de trabalho em várias regiões simultaneamente para alta disponibilidade máxima.

# Elastic Load Balancer (ELB)
O ELB distribui o tráfego de rede ou de aplicações entre várias instâncias EC2 para aumentar a tolerância a falhas e a escalabilidade.

Características:

Health Checks: O ELB faz health checks nas instâncias EC2 e, se detectar uma instância inativa, redireciona o tráfego para instâncias saudáveis.

Cross-Zone Load Balancing: Distribui o tráfego entre instâncias EC2 em diferentes zonas de disponibilidade.

Quando usar: Sempre que você precisar distribuir tráfego de maneira eficiente entre várias instâncias EC2 para evitar sobrecarga em uma única instância.
Para aumentar a disponibilidade ao rotear o tráfego entre AZs e verificar o status das instâncias.

# Services para Gerenciamento de Failover
Route 53 Health Checks e Failover:
O Amazon Route 53 oferece funcionalidades de failover DNS. Se uma região ou instância falhar, ele pode redirecionar o tráfego automaticamente para outra região ou recurso.
Health Checks garantem que os endpoints ainda estão ativos e respondendo ao tráfego.

Failover Policies: Você pode configurar políticas de failover baseadas em saúde e latência para garantir que o tráfego sempre vá para a rota mais eficiente.

# Elasticity e Otimização de Custos
Escalabilidade e alta disponibilidade nem sempre significam custos altos. AWS oferece recursos que ajudam a otimizar custos:

# EC2 Spot Instances:
Oferecem capacidade sob demanda a preços reduzidos, permitindo que você compre capacidade não utilizada da AWS.
Ideal para workloads tolerantes a interrupções, como processamento de big data, rendering, ou aplicações batch.

# Savings Plans e Reserved Instances:

Para workloads previsíveis, o Savings Plans ou Reserved Instances oferecem descontos significativos em troca de um compromisso de uso de longo prazo (1 ou 3 anos).
Reserved Instances garantem capacidade e oferecem desconto em regiões específicas.

# Auto Scaling Policies:
Defina políticas de escalabilidade baseadas em custos para garantir que os recursos estejam ajustados dinamicamente à demanda, evitando a sobreprovisionamento.

# Exemplos de Serviços AWS para Alta Disponibilidade e Escalabilidade

Amazon S3:
Oferece alta disponibilidade e durabilidade (99,999999999% de durabilidade).
Cross-region replication pode ser configurado para garantir que os dados sejam replicados em várias regiões.

Amazon RDS Multi-AZ:
Para bancos de dados relacionais, o RDS oferece suporte a implantações Multi-AZ, replicando os dados automaticamente para aumentar a disponibilidade.

Amazon DynamoDB:
Oferece alta escalabilidade e alta disponibilidade com replicação automática de dados em várias AZs.
Suporte a global tables para replicação multi-região.

Amazon ElastiCache:
Proporciona caching distribuído para aumentar a performance de suas aplicações.
Pode ser implementado em configuração de cluster para alta disponibilidade e resiliência.

# Key Takeaways para a Prova
Entenda a diferença entre escalabilidade vertical e horizontal e saiba como usar os serviços AWS para implementar cada tipo.
Seja capaz de projetar arquiteturas de alta disponibilidade, usando múltiplas AZs, ELB, ASGs e failover com Route 53.
Saiba configurar Auto Scaling Policies e como ajustar a capacidade de acordo com métricas personalizadas.
Conheça os diferentes tipos de Load Balancers e quando usar cada um.
Domine estratégias de Disaster Recovery, especialmente aquelas que envolvem replicação entre regiões.
Entenda como usar instâncias Spot e Savings Plans para reduzir custos enquanto mantém a escalabilidade e disponibilidade.
