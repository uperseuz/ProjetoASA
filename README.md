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


## 🔧 Provisionamento com Vagrant
O Vagrantfile provisiona automaticamente as quatro máquinas com:

- 📦 Box: debian/bookworm64  
- 🖥️ Provider: VirtualBox  
- 🧠 RAM: 512MB (exceto cli com 1024MB)  
- 🚫 DHCP interno do VirtualBox desativado via gatilho  
- 🆔 MACs fixos para permitir DHCP estático  
- 💽 Configuração de discos, hostname e rede

### ▶️ Subir as VMs:
vagrant up

### ⬇️ Acessar uma VM:
vagrant ssh <nome-da-vm>

## 🤖 Automação com Ansible
O Ansible realiza a configuração automática das máquinas, incluindo:

- 🔄 Atualização completa do sistema  
- ⏱️ Configuração de NTP (chrony) e timezone America/Recife  
- 👥 Criação do grupo ifpb e usuários da equipe  
- 🔐 Configuração avançada do SSH:
  - Apenas chaves públicas  
  - Sem login de root  
  - Acesso limitado a grupos autorizados  
- 📡 Instalação do cliente NFS  
- 🛡️ Permissão de sudo para o grupo ifpb

## 📡 Serviços Configurados

### 📁 Servidor arq
- 🚨 DHCP autoritativo  
- 🌐 DNS com zona direta e reversa  
- 💾 LVM usando 3 discos  
- 📤 NFS exportando /dados/nfs  
- 👤 Usuário dedicado nfs-ifpb

### 🗄️ Servidor db
- 🛢️ MariaDB  
- 🔄 Autofs montando /dados/nfs em /var/nfs

### 🌐 Servidor app
- 🧭 Apache2 com index.html personalizado  
- 🔄 Autofs montando /dados/nfs

### 💻 Cliente cli
- 🌍 Firefox e X11  
- 🔐 SSH com suporte gráfico  
- 🔄 Autofs montando /dados/nfs

## 📁 Estrutura do Repositório
/
├── Vagrantfile
├── inventory/
│   └── hosts.ini
├── ansible/
│   ├── playbook.yml
│   └── roles/
│       ├── base/
│       ├── arq/
│       ├── db/
│       ├── app/
│       └── cli/
└── README.md
