+++
title = 'Aula 01 - Introdução ao Desenvolvimento de Software Livre'
date = 2025-02-26T14:00:00-03:00
draft = false
tags = ['aula', 'introdução', 'kernel linux', 'qemu', 'libvirt']
+++

# Aula 01 - Introdução ao Desenvolvimento de Software Livre

**Data:** 26/02/2025

## O que aprendi hoje

Na aula de hoje, tivemos uma introdução à disciplina de Desenvolvimento de Software Livre. Os principais tópicos abordados foram:

1. **Apresentação da disciplina**
   - Objetivos e metodologia do curso
   - Cronograma de atividades

2. **Introdução ao desenvolvimento de kernel Linux**
   - Importância do kernel no sistema operacional
   - Ciclo de desenvolvimento do kernel Linux
   - Como contribuir para o kernel

## Atividades realizadas

O foco principal da aula foi seguir o tutorial do FLUSP sobre configuração de ambiente para desenvolvimento do kernel Linux:

- Configuramos um ambiente de teste para desenvolvimento do kernel Linux usando QEMU e libvirt
- Seguimos o tutorial disponível em: [Setting up a test environment for Linux Kernel Dev using QEMU and libvirt](https://flusp.ime.usp.br/kernel/qemu-libvirt-setup/)
- Aprendemos a criar e configurar máquinas virtuais específicas para teste de modificações no kernel

Passos realizados durante o tutorial:
1. Instalação das dependências necessárias
2. Preparação do diretório de ambiente de teste
3. Configuração de uma VM usando QEMU e libvirt
4. Configuração de acesso SSH do host para a VM
5. Obtenção da lista de módulos carregados no kernel da máquina virtual

## Reflexões

Achei muito interessante a abordagem prática logo na primeira aula. O tutorial foi bem detalhado e consegui seguir todos os passos sem problemas. A utilização de máquinas virtuais para testes de kernel é uma abordagem segura e eficiente, evitando riscos ao sistema de desenvolvimento principal.

Compreendi a importância de ter um ambiente isolado para testar modificações no kernel Linux, já que qualquer erro poderia comprometer o funcionamento do sistema operacional.

## Próximos passos

Para a próxima aula, preciso:
- Revisar os conceitos apresentados no tutorial
- Explorar mais a documentação do QEMU e libvirt
- Me familiarizar com os comandos básicos de manipulação de VMs
- Começar a estudar a estrutura do código do kernel Linux
