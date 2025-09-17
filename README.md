# ☁️ EC2 - Elastic Cloud Compute

O Amazon EC2 é o serviço de computação em nuvem da AWS que permite criar e gerenciar máquinas virtuais sob demanda.

Ele funciona como o “coração” da infraestrutura de aplicações na nuvem e substitui a necessidade de comprar e manter servidores físicos.

Modelo IaaS (Infrastructure as a Service) onde a AWS cuida da infraestrutura física (hardware, rede, datacenter, energia, redundância) e o cliente gerencia o sistema operacional (atualizações, firewall, dados, aplicações).

Alta flexibilidade: escolha do sistema operacional, quantidade de CPU, memória, rede, armazenamento e região onde a máquina rodará.

## Famílias de Instâncias EC2
Cada instância EC2 pertence a uma “família”, e cada uma possui um propósito diferente.

<img width="616" height="282" alt="image" src="https://github.com/user-attachments/assets/2a39c0f0-7b5c-4330-9a24-5cca158caee0" />

### Convenção do nome dos tipos de instancias
<img width="264" height="187" alt="image" src="https://github.com/user-attachments/assets/6a016963-2638-4f6a-b413-0bc94d7f015d" />

## Tipos de cobrança
- Sob Demanda → taxa fixa por hora/segundo, ideal para testes, ambientes de desenvolvimento e cargas imprevisíveis.
- Spot → até 90% mais barato, mas podem ser encerradas a qualquer momento com aviso de 2 minutos. Indicadas para workloads tolerantes a falhas, como processamento batch ou data analytics.
- Reservadas → contrato de 1 ou 3 anos, com desconto significativo. Ótimas para workloads estáveis e previsíveis.
- Reservadas Conversíveis → permitem alterar o tipo de instância durante o período de contrato, mantendo descontos.
- Savings Plans → alternativa flexível às instâncias reservadas, baseada em compromisso de uso (ex: $/hora).

## Escalabilidade
- Horizontal (Scale Out / In)
  - Adicionar novas instâncias EC2 para distribuir a carga (site que recebe picos de tráfego → Auto Scaling cria novas instâncias e distribui via Load Balancer).
  - Adicionar novos volumes EBS para separar funções (logs, banco, cache).
  - Uso típico: clusters, balanceadores de carga, microsserviços.
- Vertical (Scale Up / Down)
  - Aumentar a capacidade da mesma instância: mais vCPUs, memória RAM, IOPS, throughput de rede ou storage (bancos de dados monolíticos, aplicações que exigem mais poder de uma única máquina).
Usar escalabilidade horizontal sempre que possível (mais resiliente e escalável).

## Integração com Outros Serviços

- EBS → armazenamento em blocos persistente.
- S3 → armazenamento de snapshots e backups.
- Auto Scaling → cria ou remove instâncias automaticamente.
- IAM → controle de acesso e permissões.

----

# 🖼️ AMI - Amazon Machine Image

Imagem de máquina virtual pré-configurada.
A mesma AMI pode ser usada para lançar múltiplas instâncias EC2.

Pode ser criada a partir de instâncias em execução ou paradas.
Existem diversas AMIs públicas (Amazon Linux, Ubuntu, Windows, etc.), mas também é possível criar uma AMI privada para maior segurança e customização.

----

# 💾 EBS - Elastic Block Store

Serviço de armazenamento em blocos anexado ao EC2 (como se fosse um HD/SSD externo).
Persistente (não perde dados ao desligar a instância).

Altamente disponível dentro da mesma AZ (Availability Zone).
Muito usado para armazenar logs, arquivos de aplicação e dados de bancos.

## Tipos de volumes EBS

- General Purpose SSD (gp3/gp2) → equilíbrio entre custo e desempenho.
- Provisioned IOPS SSD (io2/io1) → alto desempenho para bancos de dados críticos.
- Throughput Optimized HDD (st1) → grandes volumes de dados acessados com frequência (ex: big data).
- Cold HDD (sc1) → baixo custo, dados acessados raramente.

## Snapshot EBS
- Serviço de backup nativo → cria cópias pontuais dos volumes EBS e armazena no S3.
- Incrementais → apenas mudanças desde o último snapshot são salvas, otimizando custo.
- Pode ser usado para criar novos volumes em qualquer região.

### Diferença entre AMI e Snapshot:

- AMI → backup completo da instância (sistema operacional, pacotes, configurações + volumes EBS).
- Snapshot → backup individual de volumes.

---

# 📦 S3 - Amazon Simple Storage Service

Armazenamento de objetos (arquivos estáticos).
Escalável e altamente durável (99,999999999%).

Usado para armazenar imagens, vídeos, backups, dados de análise e conteúdo estático de sites.

## Classes de Armazenamento

- Standard → acesso frequente, baixa latência.
- Standard-IA (Infrequent Access) → menor custo, ideal para dados acessados raramente, mas que precisam estar disponíveis rapidamente.
- One Zone-IA → similar ao Standard-IA, mas armazenado em apenas 1 AZ (mais barato, menos redundância).
- Glacier → arquivamento de longo prazo, custo muito baixo. Recuperação pode demorar minutos a horas.
- Glacier Deep Archive → mais barato ainda, recuperação em até 12 horas.

<img width="827" height="246" alt="image" src="https://github.com/user-attachments/assets/2beb4789-5dfa-4275-8086-728d17646a0b" />

← recurso que precisa de uma recuperação rapida |  recurso demora um pouco mais pra poder recuperar os arquivos → 

Exemplo de uso: logs antigos podem ser movidos para Glacier, enquanto arquivos acessados diariamente ficam em Standard.

