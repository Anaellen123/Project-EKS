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



 
