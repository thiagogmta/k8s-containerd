# K8s com Kubeadm e Containerd Provisionado via Vagrant e Ansible

Thiago Guimarães Tavares   
thiagogmta@ifto.edu.br

## Descrição

Olá, como vai? Se você busca criar um ambiente local para testes com Kubernetes esse repositório vai ajudar. Este projeto cria um cluster kubernetes com Kubeadm usando Vagrant e Ansible em sua máquina local.

A partir da versão 1.20 do Kubernetes o dockershin foi descontinuado e definitivamente removido na versão 1.24.

Este projeto utiliza a versão 1.23 do kubernetes adotando o **Containerd** em detrimento do **Docker**.

## Vagrant e Ansible

**Vagrant** é uma ferramenta que automatiza a criação de máquinas virtuais. O vagrant irá criar três VM`s com ubuntu server:
- Vm1 - k8s-master - Este será o Master Node do Cluster
- Vm2 - k8s-node1 - Este será o Worker Node 1 do Cluster
- Vm3 - k8s-node2 - Este será o Worker Node 1 do Cluster

**Ansible** é uma ferramenta de automação de infraestrutura. Através dessa ferramenta será automatizado o gerenciamento de pacotes, instalação e configuração dos softwares necessários.

**Worker Nodes**
Caso necesside de mais (ou menos) worker nodes. Altere a quantidade da variável 'N' em Vagrantfile.
```bash
N = 2
```

## Pré-requisitos

As ferramentas a seguir devem estar instaladas em sua máquina:

- [Vagrant](https://developer.hashicorp.com/vagrant/downloads)
- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html)
- [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)

**Característica do Cluster:**

Ubuntu Focal64 20.04 provisionado via Ansbile com as seguintes ferramentas:

- ContainerD
- Kubeadm
- Kubelet
- Kubectl

## Utilização

Clone este repositório:

```bash
git clone https://github.com/thiagogmta/k8s-containerd.git
```

**Iniciando o ambiente com Vagrant**

Acesse o diretório do repo e crie a infraestrutura com:

```bash
$ cd k8s-containerd
$ vagrant up
```

Ao final do deploy teremos a seguintes mensagem:

```bash
PLAY RECAP *********************************************************************
k8s-node2                  : ok=16   changed=15   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

Três máquinas virtuais serão criadas no Virtualbox.

![Vagrant Up](/img/vagrantup.png)

Você pode verificar as Máquinas criadas com:

```bash
$ vagrant status

Current machine states:

k8s-master                running (virtualbox)
k8s-node1                 running (virtualbox)
k8s-node2                 running (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
```

**Acessando o Ambiente**

Para acessar o ambiente basta utilizar o comando a seguir seguido do nome da VM que quer acessar.

```bash
$ vagrant ssh k8s-master
```

Será feito acesso a VM via SSH

![Vagrant SSH](/img/vagrantssh.png)

**Verificando o cluster**

Para verificar os nós do cluster utilize:

```bash
$ kubectl get nodes

NAME         STATUS   ROLES                  AGE     VERSION
k8s-master   Ready    control-plane,master   12m     v1.23.0
node1        Ready    <none>                 9m52s   v1.23.0
node2        Ready    <none>                 6m35s   v1.23.0
```

Enjoy! Seu cluster está pronto para receber aplicações de teste.

## Tratamento de Erros

Caso ocorra o seguinte erro de endereçamento na criação das VM`s:

```bash
The IP address configured for the host-only network is not within the allowed ranges. Please update the address used to be within the allowed ranges and run the command again.

Address: 192.168.10.10 Ranges: 192.168.56.0/21
```

Faça os procedimentos a seguir em seu host e tente executar o Vagrantfile novamente.

```bash
$ sudo su
$ mkdir /etc/vbox/
$ cd /etc/vbox/
$ echo '* 0.0.0.0/0 ::/0' > /etc/vbox/networks.conf
$ chmod 644 /etc/vbox/networks.conf
```
## Referência

Esse repoistório é um fork do repositório de Lorenz Vanthillo disponível em [Vagrant-ansible-kubernetes](https://github.com/lvthillo/vagrant-ansible-kubernetes).