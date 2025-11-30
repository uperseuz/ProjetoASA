📘 Projeto 01 – DevOps com Vagrant e Ansible
Administração de Sistemas Abertos – IFPB

Professor: Leonidas Francisco de Lima Junior
Período: 2025.2

👥 Integrantes da Equipe

João Wictor Ferreira Henriques da Silva – Matrícula: 20241380005

Cauã Victor Fonseca D'Almeida – Matrícula: 20141380021

📄 Descrição Detalhada do Projeto

Este projeto tem como objetivo desenvolver habilidades práticas em DevOps e Infraestrutura como Código (IaC) utilizando Vagrant para provisionamento de máquinas virtuais e Ansible para automação da configuração dos serviços de sistema.

A infraestrutura criada é composta por quatro máquinas virtuais que simulam um ambiente corporativo básico: servidor de arquivos (arq), servidor de banco de dados (db), servidor de aplicação (app) e um cliente (cli). Todos os procedimentos foram automatizados para garantir reprodutibilidade, padronização e eficiência no gerenciamento da infraestrutura.

⚙️ Arquitetura da Infraestrutura

A infraestrutura é criada via VirtualBox + Vagrant, com as seguintes características:

🖥️ Máquinas Virtuais

| Máquina | Função                                    | IP             | Hostname               | Observações                |
| ------- | ----------------------------------------- | -------------- | ---------------------- | -------------------------- |
| arq     | Servidor de arquivos, DHCP, DNS, LVM, NFS | 192.168.56.105 | arq.joao.caua.devops | 3 discos extras de 10GB    |
| db      | Servidor MariaDB                          | DHCP estático  | db.joao.caua.devops  | Usa autofs para montar NFS |
| app     | Servidor Apache                           | DHCP estático  | app.joao.caua.devops | Usa autofs para montar NFS |
| cli     | Host cliente                              | DHCP           | cli.joao.caua.devops | Suporte a X11 e autofs     |


🔧 Provisionamento com Vagrant

O arquivo Vagrantfile cria automaticamente as quatro máquinas com:

 - Box debian/bookworm64

 - RAM de 512MB (exceto cli com 1024MB)

 - Desativação do DHCP interno do VirtualBox via gatilho

 - Configuração de rede, hostname e discos adicionais quando necessário

 - MAC addresses fixos para permitir DHCP estático
