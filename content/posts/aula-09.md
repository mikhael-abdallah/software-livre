+++
title = 'Aula 09 - Primeira Contribuição para o GNOME Clocks'
date = 2025-05-07T14:00:00-03:00
draft = false
tags = ['aula', 'gnome', 'contribuição', 'vala', 'gnome-clocks']
+++

# Aula 09 - Primeira Contribuição para o GNOME Clocks

**Data:** 07/05/2025

## O que aprendi hoje

Na aula de hoje, comecei minha primeira contribuição para o projeto GNOME Clocks, trabalhando na issue [#359](https://gitlab.gnome.org/GNOME/gnome-clocks/-/issues/359). Os principais pontos abordados foram:

1. **Linguagem Vala**
   - Primeiro contato com a linguagem
   - Sintaxe e características específicas

2. **Sistema de Toasts do GNOME**
   - Componente Adw.Toast
   - Gerenciamento de notificações
   - Interação com o usuário

3. **Gerenciamento de Estado**
   - Armazenamento temporário de dados
   - Persistência de configurações
   - Manipulação de eventos

## Atividades realizadas

Durante a aula, realizei as seguintes atividades:

- Análise da issue sobre a falta de confirmação ao deletar alarmes
- Exploração do código fonte do GNOME Clocks
- Implementação da funcionalidade de desfazer deleção
- Testes da nova funcionalidade

A implementação consistiu em:
1. Adicionar variáveis para armazenar o alarme deletado e o toast
2. Modificar a função de remoção para salvar o alarme
3. Implementar a função de mostrar o toast com opção de desfazer
4. Adicionar a lógica de restauração do alarme

## Reflexões

A experiência de trabalhar com Vala foi desafiadora, mas enriquecedora:

1. **Desafios Técnicos**
   - Aprendizado de uma nova linguagem
   - Adaptação ao estilo de código do projeto
   - Entendimento do sistema de toasts

2. **Aprendizados**
   - Gerenciamento de estado em aplicações GTK
   - Padrões de design do GNOME
   - Boas práticas de UX

3. **Satisfação**
   - Implementação bem-sucedida da funcionalidade
   - Contribuição para melhorar a experiência do usuário
   - Superação dos desafios iniciais

## Próximos passos

Com a implementação concluída, os próximos passos incluem:

1. Responder a possíveis feedbacks
2. Continuar explorando outras issues do projeto

A experiência de contribuir para o GNOME Clocks foi muito valiosa, especialmente por ter que aprender uma nova linguagem e trabalhar com um framework diferente. Estou animado para continuar contribuindo e aprendendo mais sobre o ecossistema GNOME.

## Código Implementado

```vala
private Adw.Toast? delete_toast;
private Alarm.Item? deleted_alarm;

// ... existing code ...

row.remove_alarm.connect (() => {
    deleted_alarm = (Item) item;
    alarms.delete_item ((Item) item);
    if (ring_time_toast != null && item == ring_time_toast_alarm) {
        ring_time_toast_alarm = null;
        ring_time_toast.dismiss ();
    }
    show_delete_toast ();
    save ();
});

private void show_delete_toast () {
    if (deleted_alarm == null) {
        return;
    }

    if (ring_time_toast != null) {
        ring_time_toast.dismiss ();
    }

    var window = (Clocks.Window) get_root ();
    delete_toast = new Adw.Toast ("");

    delete_toast.set_title (_("Alarm deleted"));
    delete_toast.set_button_label (_("Undo"));
    delete_toast.button_clicked.connect (() => {
        if (deleted_alarm != null) {
            alarms.add (deleted_alarm);
            connect_item (deleted_alarm);
            deleted_alarm = null;
            save ();
            delete_toast.dismiss ();
        }
    });

    delete_toast.dismissed.connect (() => {
        deleted_alarm = null;
    });

    window.add_toast (delete_toast);
}
``` 