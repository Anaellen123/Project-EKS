# Projeto de Modernização para AWS com EKS

## Descrição do Projeto
Este projeto visa modernizar o sistema fornecido pelo cliente, migrando-o para a infraestrutura de nuvem da AWS e adotando as melhores práticas de arquitetura em nuvem. O sistema atual é composto pelos seguintes componentes:

### Infraestrutura Atual
1. *Banco de Dados MySQL:*
  - Armazenamento: 500 GB de dados.
  - Recursos: 10 GB de RAM e 3 CPUs.
2. *Servidor de Frontend (React):*
  - Dados: 5GB.
  - Recursos: 2 GB de RAM e 1 CPU.
3. *Servidor de Backend:*
  - Funcionalidades: Hospeda 3 APIs e utiliza o Nginx como balanceador de carga. Também armazena arquivos estáticos, como imagens e links.
  - Dados: 5 GB.
  - Recursos: 4 GB de RAM e 2 CPUs.

### Objetivo
Modernizar o sistema para utilizar uma arquitetura baseada em AWS Elastic Kubernetes Service (EKS), atendendo às seguintes diretrizes:
  -  Ambiente Kubernetes;
  -  Banco de dados gerenciado (PaaS e Multi AZ);
  -  Backup de dados;
  -  Sistema para persistência de objetos (imagens, vídeos etc.);
  -  Segurança;


## Cluster EKS:
![EKS Cloud](EKS%20Cloud.png)

#### 1. Amazon EKS:
  - O EKS está no topo do diagrama, representando o plano de controle (control plane) gerenciado pelo serviço da AWS.
  - Função: Coordena as operações do Kubernetes, como agendamento de pods, monitoramento e atualizações automáticas.
  - Vantagem: É altamente disponível, distribuído em múltiplas AZs, e você não precisa gerenciá-lo manualmente.

#### 2. Availability Zones (AZs):
  - São duas AZs representadas, cada uma contendo recursos isolados.
  - Função: Garante alta disponibilidade, pois, se uma AZ falhar, a outra continua operando.
  - Vantagem: Redundância e maior tolerância a falhas.

#### 3. Public Subnet com NAT Gateway:
  - Public Subnet: Hospeda o NAT Gateway.
  - NAT Gateway: Permite que recursos em sub-redes privadas (como nós de trabalho do Kubernetes) acessem a internet para atualizações ou comunicação sem expor seus IPs públicos.
  - Vantagem: Aumenta a segurança, pois os recursos privados não são diretamente acessíveis pela internet.

#### 4. Private Subnet:
  - Contém os nós do Kubernetes (worker nodes) e o plano de controle.
  - Função:
      - Os nós de trabalho (worker nodes) executam os pods (aplicações).
      - O plano de controle gerencia a comunicação entre os nós e os pods.
  - Vantagem: Isolamento e segurança dos componentes principais, protegendo-os de acesso externo.

#### 5. Control Plane
Ele gerencia a operação geral do Kubernetes e é essencial para que o cluster funcione corretamente.
  - Agendar os Pods: Decide em quais worker nodes os contêineres (pods) do frontend e backend serão executados.
  - Garantir que a configuração desejada (definida nos manifests) seja atendida.
  - Monitora os pods nos nós de trabalho para verificar se estão funcionando como esperado.
  - Se algum pod falhar (por exemplo, um contêiner backend cair), o Control Plane inicia automaticamente um novo pod para substituir o que falhou.
  - Facilita a comunicação entre os serviços frontend e backend no cluster.
  - Garante que os serviços sejam registrados corretamente e estejam acessíveis uns para os outros, mesmo que sejam movidos entre os nós.
  - Gerencia configurações de recursos, como:
      - Alocação de memória e CPU para pods.
      - Políticas de segurança (como o isolamento entre pods e namespaces).
  - Trabalha junto com o Auto Scaling para:
      - Ajustar o número de nós de trabalho dinamicamente com base nas cargas de trabalho.
      - Redistribuir pods para novos nós adicionados ao cluster.
  - *Por que ele está em sub-redes privadas?*
      - No diagrama, o Control Plane está isolado em sub-redes privadas, o que aumenta a segurança:
          - Ele não é diretamente acessível pela internet, protegendo-o de acessos não autorizados.
          - A comunicação com ele ocorre apenas via APIs seguras gerenciadas pela AWS (e não diretamente pelos usuários).

#### 6. Worker Nodes (frontend e backend):
  - Distribuídos entre as AZs, cada nó é responsável por executar contêineres de frontend e backend.
  - Frontend: Pode ser uma aplicação web ou interface de usuário.
  - Backend: Lida com lógica de negócios, APIs e comunicação com bancos de dados.
  - Vantagem: Separação de responsabilidades, facilitando escalabilidade independente.

#### 7. Auto Scaling:
  - Responsável por ajustar automaticamente a quantidade de nós do cluster com base na carga de trabalho.
  - Função: Garante que o cluster tenha recursos suficientes para demandas crescentes ou reduz o tamanho para economizar custos.
  - Vantagem: Otimiza custos e desempenho.

#### 8. Comunicação entre Frontend e Backend:
  - O frontend nos nós comunica-se com os pods backend via redes internas.
  - Vantagem: Minimizam latência e aumentam a segurança, pois o tráfego não sai para a internet.

#### 9. Segurança e Redundância:
  - Segurança: Os pods e nós estão em sub-redes privadas, protegidos do acesso externo direto.
  - Redundância: A arquitetura em múltiplas AZs garante alta disponibilidade.
