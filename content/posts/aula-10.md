+++
title = 'Aula 10 - Explorando Outros Repositórios do GNOME'
date = 2025-05-14T14:00:00-03:00
draft = false
tags = ['aula', 'gnome', 'contribuição', 'gnome-control-center', 'gnome-calls', 'ui', 'accessibility']
+++

# Aula 10 - Explorando Outros Repositórios do GNOME

**Data:** 14/05/2025

## O que aprendi hoje

Após a experiência inicial com o GNOME Clocks, decidi explorar outros repositórios do ecossistema GNOME para ampliar minha contribuição e aprendizado. Durante esta aula, trabalhei em duas issues diferentes: uma no [GNOME Control Center](https://gitlab.gnome.org/GNOME/gnome-control-center/-/issues/3462) sobre terminologia inconsistente e outra no [GNOME Calls](https://gitlab.gnome.org/GNOME/calls/-/issues/710) sobre permitir que contatos favoritos ignorem o modo silencioso.

## Issue #3462 - GNOME Control Center

### Sobre o problema

![Issue #3462](/software-livre/images/issue_3462.png)
![Issue #3462](/software-livre/images/issue_3462_2.png)

A [issue #3462](https://gitlab.gnome.org/GNOME/gnome-control-center/-/issues/3462) relata uma inconsistência terminológica na seção "Users" do GNOME Settings. O problema identificado foi:

**Inconsistências encontradas:**
- O subtítulo da seção diz: "Add and remove accounts, change password"
- Mas os elementos da interface se referem especificamente a "users", não "accounts"
- O diálogo é intitulado "Add User"
- Os botões são rotulados como "Add User" e "Remove User"
- Dentro do diálogo "Add User", o campo é rotulado como "Account Type"

Esta inconsistência pode confundir usuários, já que "accounts" pode sugerir algo diferente (como contas online).

### Discussão da comunidade

![Discussão da Issue #3462](/software-livre/images/issue_3462_discussion_1.png)

A discussão evoluiu com contribuições valiosas da comunidade:

- **Andre Klapper** sugeriu "Add User Account" e "User Account Type"
- **Jamie Gravendeel** expressou preferência por "Users"
- **Sam Hewitt** concordou em manter "Users" mas sugeriu melhorias na estrutura, propondo:
  - Mover o switch "is admin" para baixo sob "Details"
  - Remover o label "Account Type"
  - Reorganizar a hierarquia da interface

**Felipe Borges** (mantenedor) aprovou as sugestões do Sam e transformou a issue em uma tarefa para newcomers, fornecendo orientações específicas sobre como implementar as mudanças.

### Minha contribuição

![Meu comentário na Issue #3462](/software-livre/images/issue_3426_discussion_2.png)

Manifestei interesse em trabalhar na issue e consegui implementar as alterações na estrutura do layout conforme solicitado. No entanto, fiquei em dúvida sobre o que fazer com os arquivos de tradução relacionados ao texto "Account Type" que estava sendo removido


Infelizmente, assim como nas issues anteriores, não obtive resposta dos mantenedores sobre essa questão específica.

## Issue #710 - GNOME Calls

### Sobre o problema

![Issue #710](/software-livre/images/issue_710.png)

A [issue #710](https://gitlab.gnome.org/GNOME/calls/-/issues/710) foi criada pelo próprio mantenedor **Guido Günther** e propõe uma funcionalidade interessante: permitir que contatos favoritos ignorem o modo silencioso do dispositivo.

**Descrição do problema:**
- Muitas vezes o dispositivo está em modo silencioso
- Mas existem contatos importantes que deveriam sempre tocar o telefone
- A solução inicial proposta usa a flag "favorite" dos contatos
- Implementação relativamente simples mas útil

**Proposta técnica:**
```
incoming call -> is favorites-bypass-silent-mode gsetting set -> 
if yes: is incoming call from a favorites contact -> 
if yes, set the important hint on the feedbackd event
```

### Minha proposta de implementação

![Minha proposta inicial](/software-livre/images/issue_710_discussion_1.png)

Analisei o código e propus uma implementação focada nas áreas mencionadas:

**1. Configuração GSettings:**
- Adicionar nova chave boolean `favorites-bypass-silent-mode` em `data/org.gnome.Calls.gschema.xml`

**2. Gerenciamento de Configurações:**
- Introduzir `PROP_FAVORITES_BYPASS_SILENT_MODE` em `src/calls-settings.c` e `src/calls-settings.h`
- Implementar funções getter e setter
- Fazer bind da nova configuração

**3. Adaptação da Lógica do Ringer:**
- Modificar `src/calls-ringer.c`
- Adaptar `have_incoming_call` para considerar contatos favoritos
- Garantir que `update_ring` respeite a nova lógica

### Resposta positiva do mantenedor

![Resposta do mantenedor](/software-livre/images/issue_710_discussion_2.png)

**Guido Günther** respondeu de forma muito positiva e construtiva

Ele sugeriu uma melhoria importante: usar um enum com flags em vez de boolean para permitir expansão futura:

```c
CALLS_BYPASS_SILENT_MODE_NODE = 0
CALLS_BYPASS_SILENT_MODE_FAVORITES = 1 << 0
```

Isso permitiria adicionar outros meios de ignorar o modo silencioso no futuro (como `CALLS_BYPASS_SILENT_MODE_RECENT`). Ele recomendou estudar como o projeto Phosh implementa flags similares.

### Implementação e Merge Request

Consegui implementar a funcionalidade e criar um Merge Request. O mantenedor foi muito responsivo e fez revisões detalhadas:

![Comentário no MR](/software-livre/images/issue_710_mr_comment.png)

![Segundo comentário no MR](/software-livre/images/issue_710_mr_comment_2.png)

**Feedback recebido:**

1. **Sobre enums:** Sugestão para usar o padrão de flags do Phosh
2. **Sobre a implementação:** Proposta de usar flag `important` no sistema de feedback em vez de gerenciar silenciamento no código
3. **Sobre validações:** Comentários sobre verificações desnecessárias de valores nulos

![Revisão geral](/software-livre/images/issue_710_mr_review.png)

A revisão foi muito positiva, indicando que apenas alguns ajustes eram necessários para aprovação.

## Atividades realizadas

Durante esta aula, realizei as seguintes atividades:

- Exploração de repositórios do ecossistema GNOME
- Análise da issue sobre terminologia inconsistente no Control Center
- Implementação da reorganização da interface do diálogo "Add User"
- Estudo do código do GNOME Calls
- Proposta e implementação da funcionalidade de bypass para contatos favoritos
- Criação de Merge Request no GNOME Calls
- Interação construtiva com mantenedores
- Aprendizado sobre padrões de flags e enums no GNOME


### Diferenças entre repositórios

**GNOME Control Center:**
- Issue marcada para newcomers
- Orientações claras do mantenedor
- Falta de resposta para dúvidas específicas
- Foco em melhorias de UX/UI

**GNOME Calls:**
- Issue criada pelo próprio mantenedor
- Resposta rápida e construtiva
- Revisões detalhadas e educativas
- Foco em funcionalidade backend

### Experiência com a comunidade

A diferença na responsividade entre os repositórios foi marcante. No GNOME Calls, a experiência foi muito mais enriquecedora devido à:
- Resposta rápida do mantenedor
- Feedback técnico detalhado
- Sugestões para melhorias
- Orientação sobre padrões do projeto

## Próximos passos

1. **GNOME Calls:** Implementar as melhorias sugeridas na revisão
   - Estudar padrões de flags do Phosh
   - Implementar enum em vez de boolean
   - Usar flag `important` no sistema de feedback
   - Remover validações desnecessárias

2. **GNOME Control Center:** Aguardar resposta sobre arquivos de tradução ou pesquisar em outras issues similares

3. **Exploração continuada:** Buscar outros repositórios com mantenedores responsivos

## Observações

É notável a diferença na experiência de contribuição entre os diferentes repositórios do GNOME. Enquanto no GNOME Control Center não obtive resposta para uma dúvida específica sobre traduções, no GNOME Calls tive uma experiência muito positiva com feedback rápido e construtivo do mantenedor. Isso demonstra como a responsividade da comunidade pode impactar significativamente a motivação e o aprendizado de novos contribuidores.

A experiência no GNOME Calls, em particular, mostrou como um mantenedor engajado pode transformar uma contribuição em uma oportunidade de aprendizado valiosa, fornecendo não apenas feedback sobre o código, mas também orientações sobre melhores práticas e padrões do projeto. 