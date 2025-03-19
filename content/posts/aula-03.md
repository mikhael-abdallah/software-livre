+++
title = 'Aula 03 - Resolução de Problemas e Introdução aos Módulos do Kernel'
date = 2025-03-19T14:00:00-03:00
draft = false
tags = ['aula', 'kernel linux', 'troubleshooting', 'módulos', 'kconfig']
+++

# Aula 03 - Resolução de Problemas e Introdução aos Módulos do Kernel

**Data:** 19/03/2025

## O que aprendi hoje

Na aula de hoje, tivemos dois focos principais: resolver o problema de boot da VM enfrentado na aula anterior e avançar para o estudo de módulos do kernel Linux. Os principais tópicos abordados foram:

1. **Resolução de problemas**
   - Diagnóstico e correção de erros no processo de boot
   - Verificação de configurações e parâmetros da VM

2. **Módulos do Kernel Linux**
   - Conceito e arquitetura de módulos do kernel
   - Diferença entre código compilado diretamente no kernel e módulos carregáveis
   - Ciclo de vida de um módulo: carregamento, inicialização, execução e remoção

3. **Sistema de Build do Kernel e Kconfig**
   - Funcionamento do sistema de configuração do kernel
   - Como definir e gerenciar opções de compilação
   - Uso do menuconfig para habilitar e configurar recursos

## Atividades realizadas

A aula começou com a resolução do problema enfrentado na aula anterior:

- O monitor da disciplina me auxiliou a identificar os problemas no processo de boot da VM
- Refizemos alguns passos do tutorial anterior para corrigir as configurações
- Conseguimos finalmente fazer o boot da VM com sucesso utilizando o kernel personalizado

Após resolvermos o problema, iniciamos o terceiro tutorial:

- Seguimos o tutorial "Introduction to Linux kernel build configuration and modules" disponível no site do FLUSP
- Criamos nosso primeiro módulo simples do kernel (simple_mod.c)
- Configuramos os arquivos Kconfig e Makefile para incluir o novo módulo
- Aprendemos a carregar e descarregar módulos usando comandos como insmod, rmmod e modprobe

Passos realizados durante o tutorial:
1. Criação de um módulo simples que exibe mensagens ao ser carregado e descarregado
2. Configuração do sistema de build para incluir o novo módulo
3. Compilação do módulo usando make
4. Teste do módulo, verificando as mensagens no log do kernel (dmesg)
5. Implementação de dependências entre módulos usando EXPORT_SYMBOL

Iniciei o tutorial durante a aula e consegui finalizar todas as etapas em casa, incluindo os exercícios propostos.

## Reflexões

A aula de hoje foi particularmente valiosa por dois motivos: primeiro, por ter resolvido o problema que estava enfrentando; segundo, por me introduzir ao desenvolvimento de módulos do kernel, que é uma forma mais acessível de começar a contribuir com o kernel Linux sem precisar modificar seu código-fonte principal.

Aprendi que os módulos do kernel são uma maneira elegante de estender as funcionalidades do sistema operacional sem precisar recompilar todo o kernel. Isso facilita o desenvolvimento e testes, permitindo ciclos mais rápidos de iteração.

A experiência de criar um módulo simples, mas funcional, me deu confiança para avançar para implementações mais complexas nas próximas aulas. Também percebi como o sistema de configuração do kernel (Kconfig) é poderoso para gerenciar as inúmeras opções e dependências existentes no código.

## Próximos passos

Para a próxima aula, preciso:
- Revisar os conceitos de módulos do kernel e seu ciclo de vida
- Completar os exercícios adicionais propostos no tutorial
- Explorar mais sobre as APIs do kernel disponíveis para módulos