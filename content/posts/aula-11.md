+++
title = 'Aula 11 - Workshop de Contribuição ao Debian'
date = 2025-06-11T14:00:00-03:00
draft = false
tags = ['aula', 'debian', 'contribuição', 'packaging', 'gbp', 'workshop', 'virtualization']
+++

# Aula 11 - Workshop de Contribuição ao Debian

**Data:** 11/06/2025

Na aula de hoje, participei de um workshop sobre contribuição ao Debian que foi extremamente enriquecedor. A experiência me permitiu entender melhor o ecossistema Debian, suas ferramentas e processos de contribuição. Foi minha primeira contribuição oficial aceita no Debian!

## Configuração do Ambiente de Desenvolvimento

### Criação de VMs com QEMU

Um dos primeiros aprendizados foi sobre a criação de máquinas virtuais usando QEMU de forma mais eficiente. Aprendi a usar um comando que otimiza significativamente a performance:

```bash
qemu-system-x86_64 \
   -enable-kvm -cpu host -smp 4 \
   -m 20G \
   -drive file=debian-12-genericcloud-amd64-daily-20250610-2139.qcow2,format=qcow2 \
   -netdev user,id=net0,hostfwd=tcp::232323-:22 \
   -device virtio-net-pci,netdev=net0 \
   -nographic
```

**Benefícios desta configuração:**
- `-enable-kvm -cpu host`: Usa KVM para virtualização de hardware, melhorando drasticamente a performance
- `-smp 4`: Aloca 4 núcleos de CPU para a VM
- `-m 20G`: Define 20GB de RAM para a VM
- `hostfwd=tcp::232323-:22`: Encaminha a porta 232323 do host para a porta 22 da VM (SSH)
- `-nographic`: Roda sem interface gráfica, ideal para desenvolvimento via terminal

Esta configuração tornou o desenvolvimento muito mais fluido comparado às VMs tradicionais que eu costumava usar.

## Ferramentas e Processos do Debian

### Git-buildpackage (gbp)

Uma das ferramentas mais interessantes que aprendi foi o `git-buildpackage` (gbp), que é fundamental para o workflow de empacotamento no Debian:

**Principais comandos aprendidos:**
- `gbp clone`: Clona repositórios Debian com as branches corretas
- `gbp dch`: Gera entradas no changelog automaticamente
- `gbp buildpackage`: Constrói pacotes respeitando o workflow Debian

O gbp integra perfeitamente o Git com o sistema de empacotamento Debian, tornando o processo muito mais organizado e rastreável.

### Debian Changelog

Aprendi sobre a importância e estrutura do arquivo `debian/changelog`:

**Formato padrão:**
```
package (version) distribution; urgency=level

  * Change description
  * Another change description

 -- Maintainer Name <email@domain.com>  Date
```

**Aspectos importantes:**
- Cada entrada documenta mudanças específicas
- A versão segue o padrão Debian (upstream-revision)
- A urgência indica a importância da atualização
- O formato é rígido e deve ser respeitado

### Standards-Version no debian/control

Entendi o conceito de `Standards-Version` no arquivo `debian/control`:

- Indica qual versão da Debian Policy o pacote segue
- Precisa ser atualizado quando o pacote é revisado para conformidade com novas políticas

## Minha Contribuição: brutespray

### O Merge Request

![Merge Request Debian](/software-livre/images/debian_mr.png)

Trabalhei no pacote `brutespray`, fazendo uma atualização simples mas importante: bump da Standards-Version de uma versão anterior para 4.7.2. O trabalho envolveu:

**Mudanças realizadas:**
1. Atualização do `debian/control`:
   - `Standards-Version: 4.7.2`
2. Criação de entrada no `debian/changelog`:
   - Documentação da mudança
   - Versão correta (1.8.1-3)
   - Distribuição: experimental

**Detalhes técnicos:**
- Pacote: brutespray (ferramenta de força bruta para descobrir credenciais)
- Versão: 1.8.1-3
- Equipe: Debian Security Tools
- Co-desenvolvido com: Augusto Bernardi

O [Merge Request #3](https://salsa.debian.org/pkg-security-team/brutespray/-/merge_requests/3) foi criado seguindo o tutorial.


### Aceitação no Debian

![Pacote Aceito](/software-livre/images/debian_accepted.png)

A contribuição foi aceita e o pacote foi incluído no repositório experimental do Debian! Isso pode ser visto no [Debian Package Tracker](https://tracker.debian.org/news/1648762/accepted-brutespray-181-3-source-into-experimental/).
