# EBS (Elastic Block Store)
O Amazon EBS é um serviço de armazenamento em bloco utilizado com instâncias EC2. Ele fornece volumes persistentes que podem ser conectados a instâncias EC2 para armazenamento de dados. Ao contrário do Instance Store, que é efêmero, o EBS é persistente.

Características:
Persistência: Mesmo se uma instância EC2 for interrompida, os dados armazenados no EBS são preservados.

Anexável e Desanexável: Volumes EBS podem ser anexados a instâncias EC2 como discos de armazenamento. Um volume EBS só pode ser anexado a uma única instância, exceto no caso de volumes com Multi-Attach.

Snapshots: É possível criar snapshots de volumes EBS para backup. Esses snapshots podem ser usados para restaurar volumes ou para criar novos volumes.

Resiliente: Todos os volumes EBS são replicados automaticamente dentro de uma zona de disponibilidade (AZ), proporcionando alta durabilidade.

Quando usar: Para dados que precisam ser persistentes, como bancos de dados, sistemas de arquivos e logs de aplicações.
Quando a instância EC2 precisar de armazenamento adicional, separado do ciclo de vida da própria instância.

# AMI (Amazon Machine Image)
A Amazon Machine Image (AMI) é uma imagem que contém a configuração necessária para inicializar instâncias EC2, como o sistema operacional, software pré-configurado, bibliotecas, permissões e volumes de armazenamento.

Tipos de AMI:

Baseadas em EBS: A AMI usa um volume EBS como armazenamento raiz. Isso permite que você pare e reinicie a instância sem perder os dados.

Baseadas em Instance Store: São efêmeras e todos os dados são perdidos se a instância for interrompida ou terminada.

Criação de uma AMI: Você pode criar sua própria AMI a partir de uma instância EC2 existente, capturando uma imagem do sistema em seu estado atual (incluindo as configurações de aplicativos).

Quando usar: Para facilitar o lançamento de novas instâncias com uma configuração predefinida, eliminando a necessidade de configurar manualmente todas as instâncias.

# Instance Store
O Instance Store é um armazenamento temporário que está fisicamente ligado à instância EC2. Ele é ideal para armazenar dados temporários, que não precisam ser persistentes.

Características:

Efêmero: Todos os dados são perdidos quando a instância é encerrada ou interrompida. É ideal para cache, buffers temporários ou qualquer outro dado que possa ser facilmente regenerado.

Baixa latência: Como o armazenamento é local na própria máquina EC2, ele tende a ter baixa latência.

Quando usar: Para armazenamento temporário e rápido, onde a persistência dos dados não é importante.

Para workloads como processamento de dados temporários ou cache, onde os dados podem ser regenerados.

# Tipos de Volumes EBS
O EBS oferece diferentes tipos de volumes para diferentes requisitos de desempenho:

gp3 (General Purpose SSD):
Volume SSD de uso geral com ótimo desempenho em termos de custo-benefício.
Oferece 3.000 IOPS e 125 MB/s de throughput por padrão, que podem ser aumentados.
Usado para aplicações que requerem alta performance, como servidores web e ambientes de desenvolvimento.

gp2 (General Purpose SSD):
Volume SSD de uso geral, mas sem a flexibilidade de gp3.
Até 16.000 IOPS (baseado no tamanho do volume).
Usado para casos de uso com baixa a moderada performance.

io2/io1 (Provisioned IOPS SSD):
Volumes SSD otimizados para IOPS (operações de entrada/saída por segundo).
Suporta até 64.000 IOPS por volume (io2 é a versão mais recente com maior durabilidade).
Usado para bancos de dados de produção ou aplicações com requisitos de IOPS muito altos.

st1 (Throughput Optimized HDD):
Volume de HDD otimizado para throughput em vez de IOPS.
Usado para big data, armazenamento de log, e workloads com grandes volumes de dados acessados de maneira sequencial.

sc1 (Cold HDD):
Volume HDD com custo muito baixo, otimizado para armazenamento de dados acessados com pouca frequência.
Usado para arquivos de grandes volumes que são raramente acessados.

# EBS Multi-Attach
O EBS Multi-Attach permite que um volume EBS seja anexado a várias instâncias EC2 ao mesmo tempo. Isso é útil para aplicações que precisam de acesso compartilhado entre várias instâncias.

Limitações:

Apenas volumes do tipo io1 ou io2 suportam Multi-Attach.
As instâncias devem estar na mesma zona de disponibilidade.
O sistema operacional e o aplicativo precisam ser capazes de gerenciar a simultaneidade de acessos ao volume, evitando corrupção de dados.

Usos:

Cenários de alta disponibilidade onde múltiplas instâncias precisam acessar um mesmo volume de dados ao mesmo tempo.

# EFS (Elastic File System)
O Amazon EFS é um serviço de sistema de arquivos gerenciado que pode ser montado por várias instâncias EC2 ao mesmo tempo, permitindo armazenamento de dados compartilhado.

Características:

Escalabilidade automática: O EFS cresce e encolhe automaticamente conforme você adiciona ou remove arquivos, sem necessidade de alocar armazenamento antecipadamente.

Multizonal: Por padrão, os dados no EFS são replicados automaticamente em várias zonas de disponibilidade (AZs), aumentando a durabilidade.

Montagem múltipla: Várias instâncias EC2 podem acessar simultaneamente o mesmo sistema de arquivos, sendo ideal para aplicativos distribuídos ou ambientes de microsserviços.

Quando usar:

Aplicações que precisam de um sistema de arquivos compartilhado, como grandes sistemas de análise de dados ou sistemas distribuídos.
Ambientes que exigem alta disponibilidade e durabilidade dos dados.

# Diferenças entre EFS e EBS

EFS:

É um sistema de arquivos gerenciado e escalável.
Ideal para múltiplas instâncias que precisam compartilhar o mesmo sistema de arquivos.
Ele é distribuído entre múltiplas zonas de disponibilidade (alta disponibilidade).
Custo maior em comparação ao EBS.

EBS:

Armazenamento em bloco, conectado a uma única instância EC2 (a menos que use Multi-Attach).
Fornece alta performance e baixa latência.
Armazenamento limitado à zona de disponibilidade da instância EC2.
Mais barato do que o EFS, mas não suporta múltiplas instâncias conectadas simultaneamente, exceto com Multi-Attach.

Quando escolher EFS ou EBS:

EFS: Usado para armazenamento compartilhado em sistemas distribuídos ou onde é necessária alta disponibilidade.

EBS: Usado para armazenamento de dados persistentes em instâncias individuais, como bancos de dados e sistemas de arquivos específicos para cada instância.
