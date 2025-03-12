+++
title = 'Aula 02 - Compilação e Boot de Kernel Linux Personalizado'
date = 2025-03-12T14:00:00-03:00
draft = false
tags = ['aula', 'kernel linux', 'compilação', 'kworkflow', 'problemas']
+++

# Aula 02 - Compilação e Boot de Kernel Linux Personalizado

**Data:** 12/03/2025

## O que aprendi hoje

Na aula de hoje, avançamos para a segunda etapa do desenvolvimento do kernel Linux: a compilação e boot de um kernel personalizado. Os principais tópicos abordados foram:

1. **Introdução ao processo de compilação do kernel Linux**
   - Configuração do kernel usando arquivos .config
   - Opções de compilação e otimização
   - Ferramentas de automatização para compilação do kernel

2. **Kworkflow (kw)**
   - Apresentação do kworkflow como ferramenta para simplificar o desenvolvimento do kernel
   - Comandos básicos e configuração inicial
   - Como ele integra com o ambiente de desenvolvimento

3. **Processo de boot do kernel em sistemas ARM**
   - Particularidades do processo de boot em arquitetura ARM
   - Diferenças em relação a arquiteturas x86
   - Importância do InitRAMFS e Device Tree

## Atividades realizadas

Nesta aula, seguimos o segundo tutorial da série do FLUSP focado na compilação e boot de um kernel Linux personalizado:

- Tentamos configurar e compilar uma versão personalizada do kernel Linux para ARM
- Seguimos o tutorial: "Building and booting a custom Linux kernel for ARM (using kw)" disponível no site do FLUSP
- Aprendemos a usar o kworkflow (kw) para automatizar várias tarefas do ciclo de desenvolvimento

Passos realizados durante o tutorial:
1. Configuração inicial do kworkflow e suas dependências
2. Clonagem do repositório do kernel Linux
3. Configuração do kernel para compilação
4. Compilação do kernel utilizando o kworkflow
5. Tentativa de inicialização da VM com o kernel personalizado usando o comando `create_vm_virsh`

## Problemas enfrentados

Durante a execução do tutorial, enfrentei problemas na etapa final de boot da máquina virtual:

- O comando `create_vm_virsh` não conseguiu iniciar corretamente a máquina virtual
- A VM não iniciava o processo de boot, travando logo no início
- Tentei várias abordagens para solucionar o problema, como verificar os caminhos dos arquivos, permissões e configurações
- O problema persistiu e não consegui concluir esta parte do tutorial, ficando pendente para ser resolvido na próxima aula

## Reflexões

A aula de hoje mostrou que o desenvolvimento de kernel, mesmo em um ambiente controlado, pode apresentar desafios inesperados. A compilação do kernel em si é um processo complexo, mas as ferramentas como o kworkflow tornam o processo mais acessível.

Percebi que problemas no processo de boot podem ter diversas origens, desde configurações incorretas de partições até incompatibilidades de hardware virtual. Isso reforça a importância de um bom ambiente de testes e de ferramentas de diagnóstico.

## Próximos passos

Para a próxima aula, preciso:
- Revisar os passos do tutorial para entender onde pode ter ocorrido o problema
- Buscar informações adicionais sobre configuração de boot em VMs para ARM
- Continuar estudando a documentação do kworkflow para entender melhor suas funções