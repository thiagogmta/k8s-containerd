# K8s com Kubeadm e containerd provisionado via Vagrant e Ansible

Thiago Guimarães Tavares   
thiagogmta@ifto.edu.br

## Descrição

Olá, como vai? Se você busca criar um ambiente local para testes com Kubernetes esse repositório vai ajudar. Este projeto cria um cluster kubernetes com Kubeadm usando Vagrant e Ansible em sua máquina local.

A partir da versão 1.20 do Kubernetes o dockershin foi descontinuado e definitivamente removido na versão 1.24.

Este projeto utiliza a versão 1.23 do kubernetes adotando o **Containerd** em detrimento do **Docker**.

O cluster terá um Master Node e dois Worker Nodes onde cada nó demanda 2gb de memória ram.

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

Iniciando o ambiente com vagrant

```bash
$ cd k8s-containerd
$ vagrant up
```

Acessando o ambiente

```bash
$ vagrant ssh
```
Verificando o cluster

```bash
$ kubectl get pods
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

Esse repoistório é um fork do repositório do Lorenz Vanthillo disponível em [Vagrant-ansible-kubernetes](https://github.com/lvthillo/vagrant-ansible-kubernetes).
