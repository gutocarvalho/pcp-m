# PCP-M
## [P]uppet [C]ommunity [P]latform [M]onolithic Installation

#### Tabela de conteudo

1. [Overview](#overview)
2. [Tecnologias](#tecnologias)
3. [Autores](#autores)
4. [Contribuidores](#contribuidores)
5. [Compatibilidade](#compatibilidade)
6. [Requisitos](#requisitos)
7. [Setup](#setup)
8. [Ambiente](#ambiente)
9. [Estrutura](#estrutura)
10. [Mcollective](#mcollective)
11. [PuppetExplorer](#puppetexplorer)
12. [Debian8](#debian8)

## Sobre

O projeto PCP-M tem o objetivo de oferecer um ambiente virtual Puppet completo para testes e desenvolvimento de módulos Puppet, diferente do PCP, esse instala apenas uma VM com todos os serviços.

Usando esse Vagrantfile, subir o Puppet se torna uma tarefa simples e rápida.

## Tecnologias

* Puppet Server 2.1.2
* PuppetDB 3.2.4
* Puppet Agent 1.4.1
* PostgreSQL 9.4.6
* Puppet Explorer 2.0.0
* ActiveMQ 5.9

Todo ambiente é instalado e configurado via Puppet 4.

## Autores

* Guto Carvalho (gutocarvalho@gmail.com)
* Miguel Di Ciurcio Filho (miguel.filho@gmail.com)

## Contribuidores

* Adriano Vieira
* Lauro Silveira

## Compatibilidade

Este projeto foi testado com a vagrant box gutocarvalho/centos7x64

## Requisitos

* Virtualbox >= 4
* Vagrant >= 1.8
  * plugin vagrant-hostsupdater (atua no host)
  * plugin vagrant-hosts (atua no guest)
  * plugin vagrant-proxyconf (caso necessite e esteja atrás de proxy)
* Box gutocarvalho/centos7x64

Você precisa ter pelo menos 2 GB de RAM livre para subir a VM.

## Setup

    vagrant plugin install vagrant-hosts
    vagrant plugin install vagrant-hostsupdater
    vagrant box add gutocarvalho/centos7x64
    git clone https://github.com/gutocarvalho/pcp.git
    cd pcp
    vagrant up

### Proxy Setup

Para o caso de estar atrás de um serviço proxy:

1. instale o plugin para proxy

  ```
  vagrant plugin install vagrant-proxyconf
  ```

2. Configuration

  If necessary set OS proxy environment variable: PROXY | HTTP_PROXY | HTTPS_PROXY="http://proxy:port"

  ```
  $ export PROXY="http://proxy:3128"

  or

  $ export HTTP_PROXY="http://proxy:3128"

  ...
  ```

## Ambiente

Existem 3 VMs no ambiente

* puppetserver.hacklab, 192.168.251.20

### ambiente::puppet-pcpm.hack;ab

Nesta VM serão instaladas todas as tecnologias citadas.

## Uso

### uso::puppet-pcpm

acessando a vm

    vagrant ssh puppet-pcpm.hacklab

## Estrutura

Este projeto utiliza o repositório pcp-controlrepo como source para o r10k instalar
o environment production que será utilizado pelo puppet para configurar as VMs.

    https://github.com/gutocarvalho/pcp-controlrepo

O environment trazido deste repositório contém os arquivos abaixo

```
- environments
- - production
- - - Puppetfile
- - - environment.conf
- - - hieradata
- - - - nodes
- - - - - puppet-pcpm.hacklab.yaml
- - - - Debian-8.yaml
- - - - RedHat-7.yaml
- - - - common.yaml
- - - manifests
- - - - site.pp
- - - site
- - - - profile
- - - - - manifests
- - - - - - mcollective
- - - - - - - client.pp
- - - - - - - server.pp
- - - - - - puppet
- - - - - - - hiera.pp
- - - - - - - agent.pp
- - - - - - - server.pp
- - - - - - puppetdb
- - - - - - - app.pp
- - - - - - - database.pp
- - - - - - - frontend.pp
- - - - - - activemq.pp
- - - - - - ntp.pp
- - - - role
- - - - - manifests
- - - - - - broker.pp
- - - - - - puppetdb.pp
- - - - - - puppetmaster.pp
- - - - - - pcpm.pp
```


## Mcollective

Você pode acessar a VM e testá-lo com o comando find.

    vagrant ssh puppet-pcpm.hacklab
    sudo -i
    mco find

## PuppetExplorer

Acesse o puppet explorer através da URL

    https://puppet-pcpm.hacklab

Aceite o certificado, se possível use firefox ou chrome.

Rode o agente algumas vezes para visualizar mais informações no dashboard.
